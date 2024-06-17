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

---
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

