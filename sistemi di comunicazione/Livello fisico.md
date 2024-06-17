>[!note]
>Generalmente l'informazione si può trasmettere in due tipi di segnali:
>
>- segnali logici e digitali, quindi nativamente numerici e discreti
>- segnali fisici e analogici, quindi continui (stream) e associati a grandezze fisiche

I segnali analogici devono essere campionati e quantizzati per essere trasformati in una sequenza di bit, mentre i segnali digitali sono già pronti. Verranno poi entrambi modulati, e quindi creeranno un segnale continuo e analogico.

### Caratterizzazione spettrale dei segnali analogici
>[!tip] Analisi di Fourier
>I segnali periodici di periodo $t$ e di frequenza $f= \frac{1}{t}$ possono essere scomposti in un numero discreto di sinusoidi di frequenza multipla di quella del segnale (serie di Fourier) $$f(t)= \frac{a_{0}}{2}+\sum\limits_{k=1}^{n}a_{k}\cos(kt)+b_{k}\sin(kt)$$
>
>Ogni sinusoide è detta armonica.

Siccome spesso il segnale non è periodico, è necessario usare la trasformata di Fourier, che utilizza lo stesso concetto dell'analisi e permette di approssimare un segnale con una serie infinita di armoniche. La funzione che descrive le ampiezze e le fasi delle sinusoidi componenti è lo spettro del segnale (dominio delle frequenze).

>[!note] Banda del segnale
>Si dice banda del segnale l'intervallo di frequenze in cui sono contenute tutte le armoniche significative di quel certo segnale (massimo campo di frequenze usato).
>
>Può essere:
>- banda stretta: segnali che variano lentamente nel tempo
>- banda larga: segnali che variano velocemente nel tempo
>
>>[!example] Esempi di bande
>>
>>| Segnali | Banda |
| ---- | ---- |
| Segnale telefonico | 300-4000 Hz |
| Voce | 300-8000 Hz |
| Musica | 100-20.000 Hz |
| TV (PAL) | 0-5.000.000 Hz |
| Cinema | 0-500 MHz |

### Conversione analogico/digitale

Siccome gli elaboratori gestiscono soltanto l'informazione discreta è necessario trasformare i segnali analogici in un loro equivalente segnale.

![[Pasted image 20240221123129.png]]

#### Campionamento

Per non perdere informazione (lossless) utilizziamo il Teorema di Nyqiust

>[!tip] Teorema di Nyquist
>Un segnale del tempo è completamente determinato dai suoi campioni presi a distanza $T$ dale che $T \leq 2B$, dove $B$ è la banda del segnale, o usando la frequenza di campionamento $f_{c}= \frac{1}{T}$ $$f_{c}\geq 2B= f_{n}$$
>![[Pasted image 20240221123613.png]]

>[!tip]
>Ogni segnale analogico di banda $B$ può essere ricostruito interamente in base ai suoi campioni presi a frequenza $2B$ tramite un filtro passa-basso, che taglia le frequenze oltre $2B$.

>[!example] Frequenze di campionamento tipiche
>
>| Segnali | Banda | Frequenza di campionamento |
| ---- | ---- | ---- |
| Segnale telefonico | 300-4000 Hz | 8000 Hz |
| Voce | 300-8000 Hz | 16000 Hz |
| Musica | 100-20.000 Hz | 40.000 Hz |
| TV (PAL) | 0-5.000.000 Hz | 10 MHz |
| Cinema | 0-500 MHz | 1 GHz|

#### Quantizzazione

>[!note] 
>La quantizzazione è l'operazione in con cui una grandezza continua è trasformata in una discreta. Viene assegnato un valore dell'intervallo in cui cade ed essendoci un numero di bin finito anche i livelli saranno finiti (lossy). $$\text{livelli} = 2^\text{bit}$$
La qualità del segnale aumenta all'aumentare di livelli.
>
>![[Pasted image 20240221124434.png]]
#### Riepilogo

![[Pasted image 20240221124854.png]]

- **Blu**: Campionamento
- **Verde**: Quantizzazione

>[!example] Flussi binari equivalenti
>
>
| Segnali | Banda | Frequenza di campionamento | Livelli di quantizzazione | Flusso binario |
| ---- | ---- | ---- | ---- | ---- |
| Segnale telefonico | 300-4000 Hz | 8000 Hz | 8 bit | 64 kb/s |
| Voce | 300-8000 Hz | 16000 Hz | 16 bit | 256 kb/s |
| Musica | 100-20.000 Hz | 40.000 Hz | 16 bit | 704 kb/s |
| TV (PAL) | 0-5.000.000 Hz | 10 MHz | 24 bit | 240 Mb/s |
| Cinema | 0-500 MHz | 1 GHz | 24 bit | 24 Gb/s |

### Modulazione del canale trasmissivo
>[!note]
>La modulazione è il processo per cui si modifica un certo parametro di un segnale (es: frequenza o ampiezza) per trasmettere un segnale digitale su un canale fisico.
>![[Pasted image 20240223085646.png]]

La modulazione può essere in **banda base** o in **banda passante (o traslata)**:

##### Modulazione in banda base
![[Pasted image 20240223085959.png]]

##### Modulazione in banda traslata
>[!note]
>Si usa un'onda elettromagnetica sinusoidale detta portante (**carrier**) ad una determinata frequenza $f_{p}$ per traslare lo spettro del segnale intorno alla frequenza della portante.
![[Pasted image 20240223090205.png]]

Le onde elettromagnetica portanti vengono classificate principalmente in base alla frequenza. Basti però sapere che più alta è la frequenza del segnale, e più alta è la quantità di informazione trasferibile in un certo lasso di tempo.

Alcuni tipi di modulazione possono essere:
- Modulazione di ampiezza ASK
- Modulazione di ampiezza FSK
- Modulazione di fase PSK
- Modulazione QAM (misto di ampiezza e fase)

![[Pasted image 20240223090931.png]]

### Banda passante del mezzo trasmissivo

![[Pasted image 20240223091426.png]]

Dato un mezzo trasmissivo è necessario che il segnale non subisca distorsioni, per questo, in generale, si ha: $$\text{banda passante del mezzo }>\text{ banda occupata dal segnale}$$
Inoltre ogni mezzo trasmissivo è caratterizzato dalla velocità di trasmissione e da un ritardo di propagazione. 

La distorsione temporale del segnale è determinata dalla formula $$\triangle t_{e}\simeq \frac{1}{2B_{e}}$$
![[Pasted image 20240223092015.png]]

Affinché gli impulsi siano distinguibili occorre che $$T\geq \triangle t_{e}\simeq \frac{1}{2B_{e}}$$
La velocità del canale massima è: $$V= \frac{1}{T}\simeq 2B_{e}$$

### Modulazione multilivello

Per aumentare la capacità del canale ($bit/s$) si può usare una tecnica chiamata modulazione multilivello. Basta definire un alfabeto di $N$ simboli, un flusso di bit diviso in gruppi da $n=\log_{2}(N)$ e una convenzione di assegnamento tra un simbolo e $n$ bit.

Esempio in modulazione di ampiezza
![[Pasted image 20240223092924.png]]

Utilizzando però la modulazione multilivello la velocità di trasmissione massima diventa $$R=n\cdot 2B_{e} \text{ bit/s}$$
>[!bug]
>La velocità massima non può essere aumentata aumentando i livelli a causa del rumore che può far equivocare il livello di ricezione, causando quindi un errore.

### Errori di ricezione

A causa del rumore del segnale è possibile che venga riconosciuta una sequenza di bit diversa da quella trasmessa, il rumore può essere causato da:
- rumore termico
- interferenze da altre trasmissioni sullo stesso mezzo
- disturbi elettromagnetici
- perdite di sincronismo

##### Classificazione dei mezzi trasmissivi

- Mezzi trasmissivi guidati
	- Mezzi elettrici: ad ogni bit è associato un particolare valore di tensione o corrente, oppure determinate variazioni di tali grandezze.
	- Mezzi ottici: basati sulla propagazione guidata della luce.
- Mezzi non guidati
	- Onde radio: il segnale è associato ad un'onda elettromagnetica che si propaga nello spazio che ha la proprietà di riprodurre a distanza una corrente elettrica con un dispositivo ricevente.

>[!note] Attenuazione
>Se il segnale di ingresso ha una potenza $P_{IN}$ e il segnale di uscita ha una potenza $P_{OUT}$ si definisce attenuazione del collegamento il rapporto $$A= \frac{P_{OUT}}{P_{IN}}$$Tale rapporto può essere anche rappresentato in $dB$: $$A_{db}=10\log\left( \frac{P_{OUT}}{P_{IN}}\right)$$
>
>Solitamente i valori di attenuazione sono:
>- Mezzi wireless $A=kd^{-x}\text{ con } x=2,3,4,5$
>- Mezzi guidati:
>	- Doppino telefonico CAT5: $\frac{20db}{100m}$
>	- Cavi coassiali thin: $\frac{15db}{100m}$
>	- Cavi coassiali thick: $\frac{5-10db}{100m}$
>- Fibre ottiche:
>	- prima finestra (850 nm): $\frac{2db}{km}$
>	- seconda finestra (1310 nm): $\frac{0.4db}{100m}$
>	- terza finestra (1550 nm): $\frac{0.2db}{100m}$

### Codici correttori

Si aggiunge al sistema di trasmissione un blocco di codifica di di correzione di errore, che sono degli algoritmi applicati alla stringa di bit che riducono la probabilità di errore. I più utilizzati sono i FEC (Forward Error Correction).

Generalmente si aggiungono dei bit di parità in modo che, se limitati in numero, possono essere corretti.

>[!example] codice a ripetizione
>Consiste nel ripetere $n$ volte il bit da trasmettere. Tuttavia questo riduce il bit rate informativo in quanto è in parte dedicato al bit rate di correzzione.

### Ritrasmissione
Se un codice non riesce a correggere un errore può spesso riuscire a rilevarlo, infatti nella commutazione di pacchetto è possibile rilevare gli errori e richiedere la ritrasmissione del pacchetto errato (ARQ: Automatic Repeat reQuest)
### Capacità del canale
Nonostante codici e ritrasmissione, esiste un limite fisico alla velocità massima di un canale. Questa velocità è detta limite di Shannon: $$C=B\log_{2}\left(1+ \frac{S}{N}\right)$$
Dove:
- $C$: capacità del canale ($bps$)
- $B$: banda del canale ($Hz$)
- $S$: potenza del canale ($w$)
- $N$: potenza del rumore ($w$)
