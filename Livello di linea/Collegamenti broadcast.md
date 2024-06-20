>[!note]
>La funzione di rete richiede una grande capacità computazionale, e per ovviare il problema era semplicemente più facile usare un canale broadcast, dove tutti ricevono le trame, ma solo il destinatario preleva la trama e la inoltra a livelli superiori. Tuttavia l'invio di trasmissioni contemporanee provoca collisioni.

### Accesso multiplo fisico
>[!note]
>L'accesso multiplo è la funzione che consente di regolare l'accesso al canale ed evitare le collisioni. La funzione di accesso multiplo può essere implementata sia a livello fisico, dividendo staticamente le risorse tra le stazioni, sia a livello di linea gestendo l'accesso pacchetto per pacchetto.
>
>Questo sistema è equivalente alla multiplazione, ma è relativo al caso in cui i diversi sottocanali sono gestiti da trasmettitori diversi.

>[!tip] Frequency Division Multiple Access (FDMA)
>Completamente equivalente al FDM.

>[!tip] Time Division Multiple Access (TDMA)
>È analogo al TDM, tuttavia vengono definiti degli slot temporali dedicati alla trasmissione delle diverse stazioni. Il flusso di bit non è sincrono (con alcune eccezioni), quindi il ricevitore deve sincronizzarsi su un particolare flusso. Vanno adottati dei tempi di guardia fra gli slot.
>![[Pasted image 20240620110406.png]]

### Duplexing
>[!note]
>Il duplexing è la modalità per cui si ricavano due sensi di trasmissione da un unico mezzo trasmissivo, può essere infatti visto come un caso particolare di accesso multiplo. In alcuni casi particolari si può ottenere che si possa trasmettere e ricevere contemporaneamente (FULL DUPLEX)

In caso un canale non sia full duplex occorre ricorrere a tecniche di suddivisione della banda:
- Divisione di spazio
- Frequency Division Duplexing (FDD)
- Time Division Duplexing (TDD)

### Accesso multiplo casuale
>[!note]
>In questa modalità una stazione decide autonomamente quando trasmettere osservando il canale. Se la trasmissione collide, le stazioni coinvolte ritrasmettono dopo un tempo casuale, questo consente una buona probabilità che la collisione si ripresenti.

>[!tip] Suddivisione livello di linea
>L'accesso multiplo è gestito con dei meccanismi che regolano l'istante di trasmissione dei singoli pacchetti. Può essere gestito da un unità centrale, ma più spesso è gestito in modo distribuito dalle singole stazioni. Nell'ultimo caso il livello di linea è suddiviso in due sotto-livelli:
>- Medium Access Control (MAC)
>- Logical Link Control (LLC)
>
>Il livello MAC si occupa dell'accesso multiplo mentre LLC delle altre funzioni tipiche del livello di linea.

### ALOHA
>[!note]
>Questo protocollo è della prima rete locale mai utilizzata ed è estremamente semplice: una stazione che ha un pacchetto da trasmettere lo trasmette subito senza osservare il canale, e se rileva una collisione lo ritrasmette dopo un tempo casuale.

### Slotted ALOHA
>[!note]
>È una variante di ALOHA che assume trasmissioni sincronizzate in slot.

>[!tldr] Analisi
>Consideriamo uno scenario con:
>- $N$ stazioni
>- Ogni stazione ha un pacchetto sempre pronto per la trasmissione
>- Ogni stazione trasmette in uno slot con probabilità $p$.
>
>Se una stazione trasmette, la probabilità di successo è data da: $$P_{S}= (1-p)^{N-1}$$
>E dunque la probabilità che una qualunque stazione trasmetta e abbia successo è: $$S=Np(1-p)^{N-1}$$Questo è anche un numero medio di trasmissioni con successo in uno slot, che chiamiamo throughput ($S:\quad 0\leq S\leq 1$)
>Il numero medio di trasmissioni sul canale, che chiamiamo $G$ è dato da: $$G=Np\iff p=\frac{G}{N}$$
>Sostituendo nella formula del throughput: $$S=G \left(1- \frac{G}{N}\right)^{N-1}$$
>Questa formula ci dà il numero medio di successi in funzione del numero medio di trasmissioni. $$\lim_{N\to+\infty} S=Ge^{-G}$$
>Che ha massimo in $G=1$ e $S= \frac{1}{e}\simeq 0.37$.
>ALOHA senza slot è molto simile, ma la collisione è più probabile, e dunque l'efficienza è minore.

### Carrier Sense Multiple Access (CSMA)
>[!note]
>ALOHA non verifica che qualcuno stia usando il canale prima di trasmettere, mentre CSMA ascolta il canale prima di trasmettere.

>[!tldr] Analisi
>Se $B$ inizia a trasmettere prima che il segnale $A$ si sia propagato avviene una collisione, di conseguenza se $A$ inizia a trasmettere prima che il segnale di $B$ si sia propagato avviene una collisione.
>![[Pasted image 20240620112816.png]]![[Pasted image 20240620112826.png]]
>Sappiamo quindi che esiste un periodo di vulnerabilità pari a 2$\tau$.
>
>L'efficienza del meccanismo dipende quindi dal rapporto: $a= \frac{\tau}{T}$
>se $a>1$, il primo bit arriva quando la trasmissione è già conclusa, e quindi ascoltare il canale non serve a niente, mentre se $a<<1$ allora l'efficienza di CSMA è molto elevata.

Formula del throughput: $$S=\frac{Ge^{-aG}}{G(1+2a)+e^{aG}}$$
### CSMA - Collision Detect (CSMA-CD)
>[!note]
>Variante di CSMA dove anche le stazioni che trasmettono possono accorgersi della collisione, e di conseguenza interrompere la trasmissione per risparmiare tempo. Solitamente prima di interrompere la trasmissione si aspetta un intervallo di tempo $\delta$. La condizione necessaria affinché il meccanismo funzioni è $$T\geq 2\tau$$

Formula del throughput: $$S=\frac{Ge^{aG}}{G(1+2a)+e^{-aG}-G(1-\delta)(1-e^{-aG})}$$Che approssimata al massimo è: $$S_{\text{max}}= \frac{1}{1+5a}$$

Il collision detect si basa sul fatto che nelle reti locali cablate come Ethernet l'attenuazione del segnale è piccolo, e il livello di segnale ricevuto dalle altre stazioni è simile al proprio, quindi ci si può accorgere se c'è più di una trasmissione. Nelle reti wireless non è possibile usare CD.

Si usano quindi due messaggi:
- `RTS`: Request To Send
- `CTS`: Clear To Send

Siccome la collisione può avvenire solo su `RTS`, se si riceve `CTS` si prosegue con l'invio della trama.

![[Pasted image 20240620114256.png]]
