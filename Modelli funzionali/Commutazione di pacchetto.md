>[!note]
>La commutazione di pacchetto prevede la pacchettizzazione del messaggio e l'invio, anche su percorsi diversi, di pacchetti, che vengono poi ricomposti asincronamente dal ricevente.

Il processo di commutazione di pacchetto comprende:
1. Serializzazione del file
2. Pacchettizzazione (scomposizione dei bit dei file in "gruppetti")
3. Trasporto dei pacchetti
4. Riassemblaggio del dato

A differenza della [[Commutazione di circuito|commutazione di circuito]], nella commutazione di pacchetto non è stabilito un percorso, il percorso è deciso pacchetto per pacchetto dai singoli nodi.

La rete deve gestire ogni pacchetto quindi oltre all'informazione deve contenere altri dati, detti header

>[!example]
>Per esempio un header di un pacchetto conterrà sicuramente l'indirizzo di destinazione del pacchetto.

### Routing e trasferimento

Il router legge l'header del pacchetto e lo instrada verso il prossimo nodo. Talvolta è possibile che dei pacchetti dello stesso flusso seguano dei percorsi differenti per poi arrivare alla stessa destinazione.

L'instradamento è gestito e compiuto dai router, che contengono delle tabelle (dette tabelle di routing) che associano ad ogni indirizzo di destinazione un indirizzo di instradamento per il nodo successivo.

La commutazione di pacchetto ottimizza il trasferimento e nel caso medio è più rapida, tuttavia si devono aggiungere i bit dell'header, gestire i pacchetti e riassemblare l'informazione, e quindi le prestazioni non sono garantite. Inoltre sono necessari i registri aggiuntivi per le tabelle di routing, e dei buffer di entrata e uscita utili nel caso in cui il router sia impegnato.

### Modello di nodo

I pacchetti arrivano al ricevente in modo asincrono, cioè in tempo e ordine diverso. Questo perché la capacità dei collegamenti è arbitraria. Per questo motivo ci potrebbero essere dei conflitti temporali e di conseguenza serve avere due code per memorizzare. Una coda è posta all'ingresso l'indirizzo di destinazione, mentre l'altra è posta all'uscita per gestire i conflitti.

>[!note] Store and Forward
>La coda in ingresso utilizza il metodo **Store and Forward**:
>Il commutatore deve ricevere l'intero pacchetto prima di poter cominciare a trasmettere sul collegamento in uscita.

>[!note] Multiplazione statistica
>Mentre la coda in uscita usa il metodo **Multiplazione statistica**:
>I pacchetti sono accodati e si attende per utilizzare il collegamento

### Tipi di commutazione di pacchetto

>[!note] Datagram
>La scelta della porta d'uscita viene fatta sulla base del solo indirizzo di destinazione. Per questo motivo i pacchetti possono seguire percorsi diversi poiché la tabella di instradamento potrebbe cambiare

>[!note] Circuito virtuale
>Si forza il router a operare come per commutazione di circuito.
>
>Tutti i pacchetti vengono identificati con uno stesso identificativo, che i router utilizzano per instaurare, in una fase di setup, un percorso virtuale dove i pacchetti saranno instradati.

### Prestazioni: ritardi e throughput

>[!note] Velocità di trasmissione
>È la velocità (rate) $R$ con la quale l'informazione digitale viene trasmessa su una linea, essa viene misurata in $bit/s$ ($bps$: bit per second).

Le unità di misura sono:

- $1 B = 8bit$
- $1kbps=10^{3}bps$
- $1Mbps=10^{6}bps$
- $1Gbps=10^{9}bps$

>[!note] Tempo di trasmissione
>Il tempo $T$ per trasmettere $L$ bit dipende dalla velocità di trasmissione $R$, si ha quindi:
>$$T= \frac{L}{R}$$

>[!note] Ritardo di propagazione
>Il tempo $\tau$ affinché un impulso trasmesso dal trasmettitore $TX$ raggiunga il ricevitore $RX$ dipede dalla distanza $D$ (in metri) e dalla velocità di propagazione $v$ ($m/s$, questa velocità è prossima alla velocità della luce)
>$$\tau= \frac{D}{v}$$

Quindi il tempo di attraversamento del canale è dato fra la trasmissione del primo bit e la ricezione dell'ultimo: $$T_\text{tot}=T+\tau$$
Questo ritardo è indipendente dal numero di bit, ma dipende dalla struttura fisica.

![[Pasted image 20240221112343.png]]

In base al metodo di trasmissione di un nodo la commutazione di pacchetto si distingue in:

>[!note] Store & Forward
>Il router riceve tutti i bit prima di cominciare a trasmettere il pacchetto.

>[!note] Cut-Through
>Il router riceve tutti bit dell'header prima di cominciare a trasmettere il pacchetto. Questa tecnica è poco utilizzata perché è necessario che il rate di entrata e di uscita siano in un dato rapporto.

### Architettura di un nodo
![[Pasted image 20240221114944.png]]

>[!note] Network Interface Controller
>Interfacce con l'esterno dotate di piccola potenza di calcolo (e quindi di un microprocessore) per compiere azioni come lettura, scrittura, codifica e decodifica.

1. Una NIC riceve un pacchetto e lo trasferisce tramite DMA nella memoria principale
2. La CPU elabora i dati nella memoria principale trovando la NIC da usare in base alla tabella di instradamento
3. Il pacchetto contenuto nella memoria principale viene trasferito alla NIC scelta dal processore, che procederà a inviarlo.

Questo processo comporta l'aggiunta di un ritardo di elaborazione che va aggiunto alla formula precedente, ma solitamente è trascurabile.

### Ritardo di accodamento

Se la linea di uscita è occupata allora occorre aspettare in coda:
![[Pasted image 20240221115552.png]]
ricordando però che la coda esiste solo se i pacchetti sono indirizzati allo stesso nodo:
![[Pasted image 20240221115650.png]]![[Pasted image 20240221115711.png]]

>[!tip]
>Più in generale il ritardo di accodamento dipende dalla multiplazione statistica dovuto all'arrivo asincrono dei pacchetti alle code d'uscita. Se la linea è occupata si deve attendere in coda, ma il ritardo si verifica se:
>- Due pacchetti si trovano nella stessa coda, e di conseguenza hanno lo stesso `next-op`
>- Sufficiente ma non necessaria: La velocità di uscita è inferiore alla velocità di entrata.


>[!abstract] Ritardo di accodamento
>Del ritardo di accodamento medio $T_{a}$ si possono fare dei modelli statistici basati sulla teoria delle code. Definiamo:
>
>$R$ = velocità del link [$b/s$]
>$L$ = lunghezza del pacchetto [$bits$]
>$\lambda$ = frequenza di arrivo dei pacchetti [$pack/s$]
>$\mu$ = frequenza di trasmissione dei pacchetti [$pack/s$]
>
>Si ha $$\mu= \frac{R}{L}$$e quindi: $$T_{a}=\frac{1}{\mu-\lambda}- \frac{1}{\mu}$$
>
>Questa è una curva esponenziale, e quindi il nostro obiettivo sarà ridurre $T_{a}$ il più possibile. 
>![[Pasted image 20240221120732.png]]

