### Introduzione
>[!note]
>Il TCP è un protocollo di trasporto che assicura il trasporto affidabile. Mediante TCP è possibile costruire applicazioni che si basano sul trasferimento di file senza errori tra host remoti. Il protocollo TCP effettua anche un controllo di congestione end-to-end che limita il traffico di rete e consente agli utenti di condividere in modo equo le risorse.

##### Connection oriented
TCP è orientato alla connessione, di conseguenza prima di cominciare il trasferimento dati è necessario instaurare una connessione mediante opportuna segnalazione. Inoltre le connessioni TCP si appoggiano su datagram e sono di tipo full-duplex.

##### Flusso di dati
Il TCP è orientato alla trasmissione di flussi continue di dati: i flussi sono convertiti in segmenti di dimensione variabile e sono accumulati in un buffer. Periodicamente il TCP prende una parte dei dati nel buffer e forma un segmento. Siccome la dimensione del segmento è critica per le prestazioni TCP cerca di attendere fino a che un ammontare ragionevole di dati non sia presente nel buffer di trasmissione.

##### Numerazione di bit e riscontri
Il TCP adotta un meccanismo per il controllo delle perdite di pacchetti di tipo Go-Back-N.

Il TCP numera ogni byte trasmesso, I segmenti sono formati da gruppi di byte. Nell'header TCP è trasportato il numero di sequenza del primo byte, e il ricevitore deve riscontrare i dati ricevuti inviando il numero di sequenza del prossimo byte che ci si aspetta. Se un riscontro non arriva entro un dato timeout, i dati sono ritrasmessi.

### Formato
>[!note] Header del segmento TCP
>![[Pasted image 20240322074105.png]]
>
>Definiamo i contenuti dell'header:
>- Sequence Number: numero di sequenza del primo byte nel payload
>- Acknowledge Number: numero di sequenza del prossimo byte che si intende ricevere
>- HLEN: lunghezza totale dell'header TCP, multiplo di 32
>- Window: valore della finestra di ricezione come comunicato dal ricevitore al trasmettitore
>- Checksum: medesimo di UDP
>- Flags:
>	- `URG`: Vale `1` se vi sono dei dati urgenti e quindi TCP deve passare in modalità urgente.
>	- `ACK`: Vale `1` se il pacchetto è un ACK valido, cioè se contiene un acknowledge number valido
>	- `PSH`: Vale `1` quando il trasmettitore intende usare il comando di PUSH: il ricevitore può anche ignorare il comando
>	- `RST`: reimposta la connessione senza un tear down esplicito
>	- `SYN`: usato durante il setup per comunicare i numeri di sequenza iniziale
>	- `FIN`: usato per la chiusura esplicita di una connessione
>- Options e padding: riempimento (fino a 32 bit) e campi opzionali

Di seguito alcune opzioni aggiunte:
>[!tip] Maximum Segment Size
>Definisce la dimensione massima del segmento che verrà usata nella connessione TCP. La dimensione è decisa dal mittente durante la fase di setup. Di default è 536 byte.

>[!tip] Fattore di scala della finestra
>Definisce il fattore di scala della finestra. Fa sì che venga moltiplicato il valore del campo Window di un fattore pari a 2 elevato al valore contenuto nel campo fattore di scala. Di default è 1.

### Esempi di connessioni

>[!note] Setup
>1. Il server fa una Passive Open, che comunica al TCP locale che è pronto per accettare nuove connessioni.
>2. Il client che desidera comunicare fa una Active Open, che comunica al TCP locale che l'applicativo intende effettuare una connessione verso un dato socket.
>3. Il client TCP estrae un numero di sequenza iniziale e invia un messaggio `SYN`=1 contenente il numero di sequenza. Eventualmente indica anche i parametri aggiuntivi. Il numero casuale serve a evitare problemi nel caso il setup non vada a buon fine.
>4. Quando riceve `SYN`, il server TCP estrae un numero di sequenza iniziale e manda un segmento `SYN`=1 e `ACK`=1 contenendo `AN` pari al numero estratto dal client.
>5. Il client TCP riceve il messaggio `SYN`/`ACK` del server e invia `ACK` con `AN` pari al numero estratto dal server. Inserisce anche la dimensione di finestra del server.
>6. Il client notifica all'applicazione che la connessione è aperta.
>7. Quando il TCP server riceve l'`ACK` del client, notifica al suo applicativo che la connessione è aperta.
>![[Pasted image 20240617103944.png]]

>[!note] Tear down
>1. Il TCP che chiude la connessione invia un messaggio `FIN`=1 con gli ultimi dati.
>2. Il TCP che riceve il messaggio invia `ACK` per confermare, lasciando comunque la connessione aperta.
>3. Il TCP che ha ricevuto il messaggio invia quindi un messaggio `FIN`=1.
>4. Il TCP che ha inviato il messaggio iniziale risponde con un `ACK` finale.
>![[Pasted image 20240617105047.png]]

>[!note] Reset
>La connessione può essere chiusa senza scambio di messaggio nei due versi, in quanto è possibile semplicemente interrompere la connessione impostando il flag di `RESET`.

### Controllo di flusso
Il lato TCP ricevente controlla il flusso di quello ricevente. In entrambi i lati esistono dei Buffer che accumulano byte ricevuti/in attesa di essere trasmessi.

![[Pasted image 20240617105121.png]]

Lo spazio del buffer in ricezione disponibile per ricevere nuovi dati è detto `RCVWND`, e si estende dall'ultimo byte inoltrato all'applicazione fino alla fine del buffer. Il buffer di ricezione può riempirsi in certi casi. La sua dimensione è segnalata in ogni segmento inviato dal ricevitore al trasmettitore.
Una il buffer di invio è detto `SNDWND` e rappresenta i byte che possono essere trasmessi senza attendere ulteriori riscontri. Contiene inoltre la dimensione della finestra di ricezione del partner. Si estende dal primo byte non riscontrato all'estremo a destra della finestra di ricezione del ricevitore.

>[!error] Silly window syndrome
>Questo problema si manifesta sia nel buffer di ricezione che in quello di invio e rallenta la trasmissione.
>
>Nel ricevitore il buffer di ricezione viene svuotato lentamente, di conseguenza i segmenti sono inviati con una finestra molto piccola e il trasmettitore deve inviare segmenti corti con molto overhead.
>
>Nel trasmettitore l'applicazione genera dati lentamente, e quindi invia segmenti molto piccoli man mano che vengono prodotti.

>[!tip] Algoritmo di Clark
>Per evitare la Silly window syndrome nel ricevitore, esso mente al trasmettitore indicando una finestra nulla sino a che il suo buffer di ricezione non si è svuotato per metà o per una porzione almeno pari al MSS: $$\text{Finestra}=\max\left(\frac{\text{Receive buffer size}}{2}, \text{MSS} \right)$$

>[!tip] Algoritmo di Nagle
>Per evitare la Silly window syndrome nel trasmettitore, esso invia la prima porzione di dati anche se corta, e gli altri dati vengono inviati solo se il buffer di uscita contiene dati sufficienti a riempire una `MMS` oppure, quando si riceve un `AN` per un segmento precedente.

### Funzione Push

>[!note]
>TCP originariamente prevedeva una gestione speciale per i dati che richiedono di essere consegnati immediatamente, per ottenere l'invio immediato infatti l'applicazione può impostare il campo `PUSH`, anche se generalmente non è disponibile nelle interfacce offerte dai linguaggi di programmazione, infatti è automaticamente impostato nell'ultimo segmento che svuota il buffer.

>[!warning] Dati URGENT
>TCP dispone di una modalità urgent, che però per dei cambiamenti negli standard RFC non è stabile. Gli RFC successivi ribadiscono il cambiamento più recente, tuttavia alcuni dispositivi seguono ancora quello vecchi. Inoltre i sistemi operativi implementano urgent in maniera differente, è consigliabile non usare.

### Controllo d'errore
>[!note]
>Il meccanismo di controllo d'errore del TCP serve a recuperare pacchetti persi in rete. La causa principale di tale perdita è l'overflow di una delle code dei router attraversati a causa della congestione. I dati vengono ritrasmessi usando Go-Back-N con timeout:
>- TCP mantiene nel buffer anche i segmenti fuori ordine.
>- Quando arrivano segmenti mancanti la finestra scorre in avanti fino al primo segmento non riscontrato in quelli ricevuti fuori ordine.
>- Viene mandato un `ACK` che riscontra collettivamente anche i segmenti fuori ordine.
>
>La finestra di trasmissione dipende dal meccanismo di controllo di flusso e di congestione.

Uno dei problemi è stabilire il valore ottimo del timeout. Il suo valore ottimale dipende fortemente dal ritardo di rete, quindi il TCP calcola dinamicamente un valore opportuno per il timeout stimando il RTT.

Il TCP adatta il timeout di trasmissione alle condizioni reali della rete tramite gli algoritmi di Karn e Jacobson: campioni di RTT sono definiti come il tempo che passa tra la trasmissione di un segmento e la ricezione del relativo riscontro. Quindi tramite l'algoritmo di Jacobson viene calcolato SRR (Smoothed Round Trip Time): $$\text{SRTT}^{i}=(1-\alpha)\text{SRTT}^{i-1}+\alpha\text{RTT}^{i}\qquad 0<\alpha\left( \frac{1}{8}\right)<1$$
Viene stimata anche la deviazione standard dei $\text{RTT}$: $$\text{DEV}= |\text{RTT}^{i}- \text{SRTT}^{i-1}|$$
E di essa viene calcolato un valore filtrato: $$\text{SDEV}^{i}= \frac{3}{4}\text{SDEV}^{i-1}+ \frac{1}{4}\text{DEV}^{i}$$

Sulla base di questi valori il timeout è calcolato come $$\text{TIMEOUT}=\text{SRTT}+4\text{SDEV}$$
Inizialmente vale $1s$.

Dopo la ritrasmissione è necessario passare all'algoritmo di Karn:
- $\text{RTT}$ non è aggiornato
- Il timeout è moltiplicato per un valore fisso (tipicamente 2)
- Il timeout cresce fino ad un valore massimo
- Dopo un numero massimo di ritrasmissioni la connessione viene chiusa.

>[!tip] Persistenza
>Se il destinatario fissa a 0 la finestra di ricezione la sorgente TCP interrompe la trasmissione, e riprende solo quando il destinatario invia un `ACK` con una dimensione della finestra diversa da 0. Tuttavia se questo `ACK` andasse perso la connessione rimarrebbe bloccata.
>
>Per evitare questa situazione si usa un timer di persistenza che viene attivato quando arriva un segmento con finestra nulla. Se il timer di persistenza scade, viene inviato un piccolo segmento sonda, in questo modo se viene ricevuto un `ACK` si esce dallo stato critico, altrimenti al nuovo scadere del timeout si re invia di nuovo.

### Controllo della congestione
>[!note]
>Il controllo di flusso dipende dalla capacità del ricevitore. Tuttavia attualmente non esistono meccanismi di controllo di congestione a livello rete, esso è delegato a TCP e quindi è end-to-end. Il modo più naturale per controllare il ritmo di immissione in rete dei dati per il TCP è quello di regolare la finestra di trasmissione.
>
>Il trasmettitore mantiene una Congestion Window (`CWND`) che varia in base agli eventi osservati. Il trasmettitore non può trasmettere più del minimo tra `RCVWND` e `CWND`: $$\text{Rate}=\min(\text{RCVWND},\text{CWND})$$
>L'idea di base del controllo di congestione del TCP è quella di interpretare la perdita di un segmento, come un evento di congestione, e la reazione è quella di ridurre `CWND`, quindi riduco il rate equivalente sulla connessione TCP che sto valutando.

Il valore di `CWND` viene aggiornato dal trasmettitore TCP in base ad un algoritmo: Il modo in cuoi avviene l'aggiornamento si basa sulla fase del trasmettitore (Slow start e Congestion Avoidance). Lo stato è dettato dalla variabile `SSTHRESH`:
- Se `CWND`$<$`SSTHRESH` si è in Slow Start.
- Se `CWND`$>$`SSTHRESH` si è in Congestion Avoidance.

Inizialmente, il trasmettitore pone `CWND` a 1 `MSS` e la `SSTHRESH` a un valore molto elevato, di conseguenza si parte in Slow Start.  In questa modalità, la `CWND` viene incrementata di 1 per ogni `ACK` ricevuto. Al contrario di quanto il nome faccia credere infatti, l'incremento della finestra avviene in modo esponenziale. L'incremento può andare avanti fino a uno di questi eventi:
- Evento di congestione.
- Fino a che `CWND`$<$`SSTHRESH`
- Fino a che `CWND`$<$`RCWND`

Insieme alla finestra aumenta anche il rate di trasmissione, che è stimato come $$R=\frac{\text{CWND}}{\text{RTT}}\qquad \left[ \frac{\text{bit}}{s}\right]$$

>[!tip] Evento di congestione
>Un evento di congestione si verifica quando il ritmo di trasmissione porta in congestione un link sul percorso in rete verso la destinazione. Un link è definito congestionato quando la somma dei ritmi di trasmissione dei flussi che lo attraversano è maggiore delle sue capacità.
>
>In caso scada il timeout di ritrasmissione il TCP reagisce ponendo `SSTHRESH` pari alla metà dei byte trasmessi ma non riscontrati: $$\text{SSTHRESH}=\max\left(2\text{MSS}, \frac{\text{FlightSize}}{2}\right)\qquad \text{CWND}=1\text{MSS}$$
>
>Di solito i "byte in volo" sono una buona approssimazione pari all'ultima `CWND`. Di conseguenza:
>- `CWND` è minore di `SSTHRESH` e si entra nella fase di Slow Start
>- Il trasmettitore invia un segmento e la sua `CWND` è incrementata di 1 ad ogni `ACK`.
>
>A questo punto il trasmettitore trasmette tutti i segmenti a partire da quello per cui il timeout è fallito (Go-Back-N), e il valore a cui è posta `SSTHRESH` è una stima della finestra ottimale che eviterebbe futuri eventi di congestione.

La slow start continua fino a che `CWND` diventa grande come `SSTHRESH` e poi parte la fase di Congestion Avoidance. Durante questa fase si incrementa `CWND` di $\frac{1}{\text{CWND}}$ ad ogni `ACK` ricevuto. Se la `CWND` consente di trasmettere $n$ segmenti, la ricezione degli `ACK` relativi a tutti gli $n$ segmenti porta la `CWND` ad aumentare di 1 segmento.
In questa modalità quindi si attua un incremento lineare della finestra di congestione.

### Altre funzionalità e informazioni

>[!note] Condivisione equa delle risorse
>È possibile dimostrare che in condizioni ideali il meccanismo di controllo del TCP è in grado di:
>- Limitare la congestione in rete
>- Consentire di dividere in modo equo la capacità dei link tra i diversi flussi
>
>Le condizioni ideali sono alterate tra l'altro da:
>- Differenti RTT per i diversi flussi
>- Buffer nei nodi minori del prodotto bandaritardo

>[!note] Fast Retransmit e Fast Recovery
>Questa feature aggiuntiva al TCP classico è usata per ottimizzare la ritrasmissione del singolo pacchetto perso.
>
>Se arrivano degli `ACK` duplicati il pacchetto sarà andato perso, tuttavia saranno arrivati i pacchetti successivi. Se non c'è congestione possiamo incrementare `CWND` del numero di pacchetti sicuramente arrivati. Grazie a questa feature il trasmettitore trasmette il pacchetto perso dopo il terzo `ACK` duplicato:
>
>1. Alla ricezione del terzo `ACK` consecutivo duplicato: $$\text{SSTHRESH}=\max\left( 2\text{MSS}, \frac{\text{FlightSize}}{2}\right)\qquad\text{CWND}=1\text{MSS}$$
>2. Viene ritrasmesso il pacchetto indicato dall'`AN`.
>3. Si pone: $$\text{CWND}=\text{SSTHRESH}+3\text{MSS}$$
>4. Per ogni ulteriore `ACK` duplicato ricevuto la `CWND` viene incrementata di 1.
>5. Vengono trasmessi nuovi segmenti se consentito dai valori di `CWND` e `RWND`.
>6. Appena arriva un `ACK` che riscontra nuovi dati si esce dalla fase di fast recovery e si pone di nuovo: $$\text{CWND}=\text{SSTHRESH}=\max\left(2\text{MSS}, \frac{\text{FlightSize}}{2}\right)\qquad \text{CWND}=1\text{MSS}$$

>[!note] TCP nei sistemi operativi
>Esistono diverse versioni di TCP, ma sono tutte retrocompatibili con le versioni base. TCP è implementato nel sistema operativo, infatti esistono versioni come ECN che viola la comunicazione tra livelli differenti.
>Esistono inoltre versioni adatte a connessioni ibride, dove le perdite sono dovute sia alla congestione che alla perdita sporadica legata a problemi di connessione radio.
>
>La versione base TCP si chiama Tahoe.


