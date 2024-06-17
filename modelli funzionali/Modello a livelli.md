Un servizio di comunicazione è strutturato a molteplici livelli. Per trasferire dati ogni livello aggiunge un suo header e lo passa al livello inferiore, e viceversa per ricevere ogni livello rimuoverà il suo header e passerà i dati al livello superiore.
![[Pasted image 20240221140605.png]]

### Il modello TCP/IP
>[!note]
>Il modello TCP/IP è un implementazione del modello ISO/OSI (modello teorico) utilizzato da internet. Si basa su 5 livelli qui rappresentati:
>![[Pasted image 20240221140946.png]]

### Esempi di funzioni dei livelli

>[!example] Multiplazione e demultiplazione
>Permette a più livelli superiori di condividere lo stesso servizio di comunicazione, etichettando le informazioni per permettere la corretta demultiplazione. (Le applicazioni sono distinte in base a un numero di porta identificata da un etichetta di 16 bit)

>[!example] Funzione di controllo d'errore
>Inserisce in un pacchetto delle informazioni aggiuntive utilizzate per controllare il messaggio tramite un operazione matematica, garantendo quindi l'affidabilità delle comunicazioni anche in caso di errori sul canale. In caso non viene ricevuta l'informazione giusta il ricevente non invia il segnale di `ACK` e dopo un timeout il mittente re-invia il messaggio.

>[!example] Funzione di routing
>Implementa il meccanismo che ci permette di dare informazioni e instradare nel percorso giusto verso la destinazione. Il livello superiore passa un parametro indirizzo,  grazie al quale l'entità instradante decide dove inoltrare il pacchetto in base alla tabella di instradamento. Questa funzione in base alle circostanze può essere implementata in Router IP, LAN Switch e Proxy

### Tabella di routing
Le tabelle di routing sono delle tabelle che mappano un indirizzo a una porta fisica di router. Possono essere definite nei seguenti metodi:

>[!note] Human Defined Networking
>Le tabelle di instradamento sono scritte a mano. Nei router IP le rotte configurate manualmente sono dette rotte statiche.

>[!note] Protocollo di instradamento distribuiti
>Questo approccio si basa su uno scambio di informazioni tra i router, che consente di compilare le tabelle di instradamento in moto automatico e distribuito. Degli esempi di questo approccio possono essere MAC Learning o Instradamento per cammini minimi.

>[!note] Software Defined Neworking
>Un nuovo approccio permette di operare in un approccio manuale, ma con un applicazione centralizzata che compila e modifica le tabelle di instradamento autonomamente.