>[!note]
>La prima funzione del livello logico è individuare il significato dei bit scambiati con il livello fisico. Per questo scopo i bit sono raggruppati in trame. Per delimitare le trame si usano:
>- delimitatori di trama
>- segnalazioni passate dal livello fisico

### Delimitazione di trama
Si usano dei flag per trovare l'allineamento di trame, cioè si inserisce una sequenza di bit all'inizio e alla fine di una trama.

>[!tip] HDLC
>Una sequenza conosciuta è HDLC: $$01111110$$
>Tuttavia rimane il problema di impedire una sequenza uguale casuale nei dati. Per farlo si usa il bit stuffing, cioè si inserisce uno 0 dopo aver osservato cinque 1 consecutivi.

### Controllo d'errore
A differenza del livello trasporto dove l'obiettivo è il recupero dei segmenti persi, nel livello di linea l'obiettivo è il recupero degli errori di livello fisico. Esiste la ritrasmissione anche nel caso di collegamento broadcast e può servire a recuperare anche le collisioni sul canale.

### Multiplazione
>[!note]
>Nei collegamenti punto-punto i protocolli di linea possono essere istanziati su più canali fisici, in alcuni casi il canale viene diviso in più sotto-canali a livello fisico, quest'operazione è detta multiplazione fisica.

>[!tip] Multiplazione fisica
>La multiplazione a livello fisico consiste nel suddividere la capacità di un canale a velocità costante in sottocanali di velocità costante e inferiore.
![[Pasted image 20240620104201.png]]

La multiplazione si divide in tipologie diverse in base alla caratteristica fisica scelta per la separazione dei canali:
- Divisione di spazio
- Frequency Division Multiplexing (FDM)
- Time Division Multiplexing (TDM)
- Code Division Multiplexing (CDM)
- Wavelength Division Multiplexing (WDM)

### Multiplazione FDM
>[!note]
>Il mezzo trasmissivo è caratterizzato da una banda di frequenze utilizzabili. La banda complessiva può essere divisa in sotto-bande cui associare il canale.
>![[Pasted image 20240620104753.png]]![[Pasted image 20240620104827.png]]

### Multiplazione TDM
>[!note]
>I bit di $N$ flussi vengono raccolti in code e trasmessi sul flusso di uscita a gruppi di $K$
>![[Pasted image 20240620104615.png]]
>La durata della trama deve eguagliare l'intervallo di tempo in cui sul singolo canale in entrata arrivano i bit in numero pari a quelli trasmessi nella trama.

Definiamo:
- $V$: velocità del flusso tributario (entrata)
- $W$: velocità del multiplex
- $N$: numero di tributari
- $k$: grado di interlacciamento
- $T_{T}$: durata della trama
- $T_{S}$: durata dello slot

Avremo quindi che:
$$T_{T}= \frac{k}{V}\qquad T_{T}= \frac{Nk}{W}\qquad T_{S}= \frac{T_{T}}{N}\qquad V= \frac{k}{T_{T}}= \frac{W}{N}$$
