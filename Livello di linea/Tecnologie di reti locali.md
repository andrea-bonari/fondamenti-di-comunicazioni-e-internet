>[!note]
>L'Institute for Electrical and Electronic Engineers (IEEE) è il principale organismo di standardizzazione per le reti locali. Sono standardizzate diverse tecnologie da IEEE 802, in queste i livelli LLC e superiori sono in comuni, e MAC e il livello fisico sono diversi.

### Ethernet
>[!note]
>Ethernet è stato standardizzato da IEEE 802.3. Il mezzo fisico adottato originariamente era un cavo coassiale passivo a cui si connettevano le stazioni mediante un transceiver. Solitamente le grandezze maggiori erano 100 metri, ma in certi casi anche 1 chilometro. Siccome le prestazioni calano all'aumentare della distanza si sono adottati dei ripetitori a livello fisico.

>[!tip] Formato cavi coassiali
>Il tipo di cavo coassiale è determinato dal nome $$\text{Ethernet } X\text{Base}Y$$
>dove:
>- $X$: bitrate in $Mb/s$
>- Base: trasmissione in banda base
>- $Y$: massima lunghezza in decametri

I BUS con cavo coassiale sono poi stati sostituiti da topologie a stella, che si basano su un ripetitore di segnale a livello visico multi-porta, chiamati HUB. In questa occasione il mezzo trasmissivo è stato rimpiazzato da doppini di rame ($\text{Ethernet } 10\text{Base}TX$).

>[!tip] Fast Ethernet
>Il rate di trasmissione è aumentato da 10 $MB/s$ a 100 $MB/s$ con Fast Ethernet. In questa occasione, in aggiunta al doppino di rame, si comincia ad usare la fibra ottica ($\text{Ethernet }10\text{Base}FX$)

>[!tip] Gigabit Ethernet
>Il rate di trasmissione venne poi aumentato da 100 $MB/s$ a 1 $GB/s$, e successivamente a 10 $GB/s$. Sono ancora utilizzati doppini di tipi diversi e fibre ottiche, multimodali e monomodali.

### IEEE 802.3 protocol stack
>[!note]
>Esiste una versione dello stack per Ethernet che mette insieme LLC e MAC in un unico "livello Ethernet". 
>![[Pasted image 20240620134342.png]]
>
>Di seguito il significato dei campi:
>- `Sync`: Preambolo di sincronizzazione di livello fisico ($10101010$ ripetuto 7 volte)
>- `FD`: Delimitatore inizio trama ($10101011$)
>- Indirizzi destinazione e sorgente: definiti dal costruttore della scheda di rete (48 bit)
>- `Type`: Per la multiplazione di livelli superiori
>- `Dati`: Campo dati per il PDU del livello superiore
>- `FCS`: Frame Check Sequence per il controllo d'errore (CRC)

>[!tip] Indirizzi MAC
>Gli indirizzi della rete locale sono detti indirizzi MAC, e sono unici globalmente alla NIC. È un indirizzo di 6 byte dove, i primi 3 byte identificano il costruttore, e i secondi 3 byte identificano la scheda. Sono solitamente indicati con notazione esadecimale. L'indirizzo con tutti i bit impostati a 1 è l'indirizzo di broadcast.
>Esempio di indirizzo MAC:
>$$62:f3:6b:97:27:e0$$

### WiFi
>[!note]
>La tecnologia WiFi è standardizzata dal gruppo di lavoro IEEE 802.11, ed è la versione wireless di Ethernet. Definiamo il Basic Service Set (BSS) come una rete wireless e Access Point (AP) come punto di collegamento alla rete.
>![[Pasted image 20240620135415.png]]
>![[Pasted image 20240620135436.png]]
>L'header WiFi è estremamente simile a quell Ethernet, tuttavia:
>- Ha 4 indirizzi per trama, per riconoscere le varie modalità di trasmissione
>- Necessita di un numero di sequenza perché ogni trama deve essere riscontrata
>- È indicata una durata temporale della trasmissione della trama (usato per aiutare il meccanismo anti-collisioni)
>- Il campo `Frame Control` definisce la versione del protocollo, il tipo di trama, la gestione energetica dei dispositivi, frammentazione, etc



