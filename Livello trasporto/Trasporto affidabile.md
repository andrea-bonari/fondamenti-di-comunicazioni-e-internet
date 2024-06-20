>[!note]
>Un collegamento è detto ideale se tutto ciò che viene trasmesso arriva nello stesso ordine e viene correttamente interpretato a destinazione.

In un protocollo di ritrasmissione ciascuna trama ricevuta viene riscontrata da un messaggio di `ACK` o `NACK`. La mancanza di `ACK` o la presenza di `NACK` comportano la necessità di ritrasmettere il messaggio, e la procedura è ripetuta fino a quando la trama ricevuta non è corretta.

>[!tip] Controllo di integrità
>Queste procedure sono attivate a qualunque livello, è presente a livello trasporto per il recupero end-to-end.

### Protocolli di ritrasmissione
>[!note]
>I protocolli di ritrasmissione hanno come obiettivo l'integrità del messaggio, l'ordine della sequenza di pacchetti e evitare la duplicazione, usando i messaggi `ACK` e `NACK` e meccanismi come il timeout e la finestra di trasmissione

#### Protocollo Stop & Wait
>[!note]
>Questo protocollo utilizza soltanto gli `ACK` e un contatore di timeout. Ogni messaggio ricevuto correttamente è riscontrato con un `ACK`.

Il trasmettitore trasmette un pacchetto e inizializza un contatore di timeout, in base all'evento successivo decide cosa fare:
- In caso di `ACK` trasmette il pacchetto successivo
- In caso di timeout ritrasmette il pacchetto corrente

![[Pasted image 20240502154850.png]]

Il protocollo ha funzionamento corretto se $$\text{timeout}\geq 2\tau+T_{\text{ack}}$$

---
Ogni pacchetto e `ACK` viene numerato per evitare errori dati dalla trasmissione multipla di pacchetti. I pacchetti sono numerati con `SN`, mentre gli `ACK` sono numerati con `AN`.

Quando il trasmettitore riceve un `ACK` deve poterlo assegnare al pacchetto corretto.

---
L'efficienza del protocollo, cioè la frazione di tempo in cui il canale è usato per trasmettere informazione utile in assenza di errori, si calcola con la formula: $$ƞ= \frac{T}{T+T_\text{ack}+2\tau}=\frac{1}{1+\frac{T_\text{ack}}{T}+\frac{2\tau}{T}}$$
Ha un efficienza molto bassa se $T<<\tau$. Infatti non è adatto a situazioni con elevato ritardo di propagazione o a elevato ritmo di trasmissione. 

È spesso utilizzato in modalità half-duplex.

#### Protocollo Go-back-N
>[!note]
>È una variante di Stop & Wait, dove si possono trasmettere fino a $N$ pacchetti (finestra) senza aver avuto il riscontro.
>![[Pasted image 20240502155933.png]]
>
>Se il riscontro del primo pacchetto arriva prima della fine della finestra, la finestra viene fatta scorrere di una posizione.
>![[Pasted image 20240502160022.png]]

Quando si verifica un errore si ricomincia a trasmettere la finestra dal primo pacchetto non riscontrato.

Il timeout funziona come nello Stop & Wait: raggiunto l'ultimo pacchetto della finestra, la trasmissione si blocca in attesa di un nuovo `ACK` o della scadenza del timeout.
![[Pasted image 20240502160835.png]]

Questo sistema può causare la ritrasmissione di pacchetti corretti ma semplifica il funzionamento dato che permette al ricevitore di ignorare ricezioni fuori sequenza, in quanto per i pacchetti ignorati non è trasmesso alcun `ACK`.

In caso il timeout non scada e i pacchetti non siano fuori sequenza un `ACK` può essere collettivo.

La finestra ottima coincide con il Round Trip Time:
![[Pasted image 20240502161336.png]]
$$N=\left\lceil \frac{T+T_\text{ack}+2\tau}{T} \right\rceil$$

La finestra può essere dimensionata in tempo, in byte e in molti altri modi, tuttavia il dimensionamento si complica se i tempi di attraversamento della rete ($\tau$) variano con il tempo.

Per rimediare possiamo:
- Rendere la finestra grande:
	- Non pregiudica il funzionamento in assenza d'errore
	- In caso di errore aumenta il ritardo con cui si scopre ed il numero di ritrasmissioni inutili
	- Uso del `NACK`
- Stimare il tempo del RTT e adattare la finestra o il timeout.

#### Protocollo Go-back-N full duplex
>[!tip] Piggy backing
Gli `ACK` possono essere inseriti negli header dei pacchetti che viaggiano in direzione opposta (Piggy-backing). Per farlo introduciamo i campi `SN` e `AN`, dove `SN` è il numero di sequenza del pacchetto trasmesso e `AN` è un riscontro cumulativo dei pacchetti fino a `SN`$-1$ ed è inviato come risposta dal ricevente.

Le regole del protocollo Go-back-N in modalità full duplex (canale di trasmissione doppio bidirezionale) per il trasmettitore, definendo $N$ come la dimensione della finestra, $N_\text{last}$ come l'ultimo riscontro ricevuto e $N_\text{C}$ come numero corrente disponibile per la trasmissione, sono:
- Ogni nuovo pacchetto viene messo in attesa se $N_\text{C}\geq N_\text{last} + N$, altrimenti, se $N_\text{C}<N_\text{last}+N$, il pacchetto viene trasmesso con `SN` pari a $N_\text{C}$, viene inizializzato il timer di timeout e il valore di $N_\text{C}$ viene incrementato di uno.
- Ad ogni riscontro `AN` ricevuto si pone $N_\text{last}=$`AN`.
- I pacchetti nella finestra vengono trasmessi senza vincoli di temporizzazione.
- In caso di scadenza di timeout la ritrasmissione deve ripartire da $N_\text{last}$

L'intervallo della finestra di trasmissione è : $$(N_\text{last},N_\text{last}+N-1)$$

Analogamente, per il ricevitore, definendo `AN` come stato dei riscontri corrente:
- Se viene ricevuto correttamente un pacchetto con `SN`$=$`AN`, questo viene inoltrato ai livelli superiori e si incrementa `AN` di uno.
- Ad istanti arbitrari ma con ritardo finito, `AN` viene trasmesso al mittente utilizzando i pacchetti in direzione opposta.

Per evitare errori è necessario inizializzare `SN` e `AN` e deve esistere un momento $t=0$ non equivocabile per scambiare l'informazione per l'inizializzazione.

#### Controllo di flusso a finestra mobile
Il buffer di ricezione è limitato a `W` posizioni, e il ritmo di assorbimento dell'utente può essere arbitrario. L'obiettivo è regolare il ritmo di invio per evitare che i pacchetti vadano persi perché all'arrivo trovano il buffer pieno.

Per gestire il controllo di flusso si può utilizzare un meccanismo a finestra mobile, come il Go-Back-N. In questo schema, la sorgente non può inviare più di `W` trame senza aver ricevuto il riscontro. I riscontri vengono inviati dal ricevitore solo quando i pacchetti vengono letti dal livello superiore e rimossi dal buffer.

Un problema significativo è quello delle ritrasmissioni. Se il ricevitore ritarda molto l'invio dei riscontri a causa della lentezza del livello superiore, il trasmettitore inizia la ritrasmissione perché scade il timeout. Questo dimostra come il controllo di flusso a finestra mobile e il controllo d'errore a finestra mobile siano fortemente legati. Tuttavia, aumentare il timeout non è una soluzione efficace, in quanto aumenterebbe i ritardi in caso di errore.

Una soluzione radicale per questo problema consiste nel separare i meccanismi di controllo d'errore e di controllo di flusso a finestra. Si può inserire nei riscontri, o nell'header dei pacchetti in direzione opposta, un campo finestra `W`, oltre a quello del Go-Back-N. In questo modo, il ricevitore invia i riscontri sulla base dell'arrivo dei pacchetti e utilizza il campo W per indicare lo spazio rimanente nel buffer.

La gestione della finestra del controllo di flusso può essere ulteriormente ottimizzata. Non è necessario che il ricevitore dichiari esattamente lo spazio rimanente `R`; può mantenere un margine di sicurezza (ad esempio, $W=R-m$), dichiarando il buffer vuoto anche se non lo è completamente, garantendo robustezza nel caso in cui arrivino altri segmenti prima che il nuovo valore di `W` sia ricevuto. Inoltre, il ricevitore può aspettare che il buffer si svuoti per una frazione significativa (ad esempio, dichiarando $W=0$ se $R < \frac{\text{buffer}}{2}$ e $W=R$ altrimenti), evitando così l'invio di segmenti troppo brevi. Infine, è possibile usare meccanismi adattativi per regolare dinamicamente il valore di `W` in base alla situazione del buffer.

Questi accorgimenti permettono una gestione più efficiente del controllo di flusso e riducono la probabilità di perdita di pacchetti, migliorando la robustezza e l'affidabilità della comunicazione.
