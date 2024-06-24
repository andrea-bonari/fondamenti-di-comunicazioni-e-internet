>[!note]
>La tecnologia WiFi è la versione Wireless di Ethernet usata per le reti locali ed è standardizzata da IEEE 802.11.

### Struttura rete WLAN
>[!tip] Basic Service Set
>Rete di dispositivi wireless, può essere in modalità Access Point, o ad hoc.

>[!tip] Extended Service Set
>Diversi BSS messi in comunicazione tramite un Distribution System (DS), che può essere cablato e wireless.

### WLAN
Anche se un mezzo è condiviso l'elevata attenuazione dei segnali tipica dei canali wireless determina un raggio di copertura di ciascuna stazione. Il protocollo CSMA-CD non è più utilizzato a causa del problema della stazione nascosta, e quindi è necessario l'uso dell'`ACK`.

![[Pasted image 20240624101418.png]]

### CSMA Collision Avoidance (CSMA-CA)
>[!note]
>In questo protocollo il mittente invia un esplicita richiesta di autorizzazione a trasmettere (`RTS`) e il destinatario risponde con esplicita autorizzazione (`CTS`). Un messaggio di `ACK` è richiesto per ogni trama ricevuta correttamente.
>![[Pasted image 20240624101703.png]]

### CSMA-CA con Backoff
>[!note]
>Il Collision Avoidance con Backoff è una tecnica usata per gestire l'accesso al canale trasmissivo nei sistemi di comunicazione wireless, assicurando che le trasmissioni avvengano senza collisioni. Quando una stazione rileva che il canale è occupato, essa continua ad ascoltare fino a quando il canale non diventa libero. Una volta che il canale è libero, la stazione aspetta un intervallo di tempo pari a DIFS (Distributed Inter-Frame Space) più un periodo di backoff, che è un numero di slot temporali scelto casualmente. Se il canale si occupa nuovamente durante il periodo di backoff, il conteggio del backoff si ferma e riprende solo quando il canale diventa libero.

Questo meccanismo è applicato anche per la trasmissione di trame consecutive: la stazione deve comunque aspettare un periodo di DIFS più un eventuale backoff, anche se il canale è libero. In caso di collisioni, l'intervallo di backoff viene scelto da un intervallo più ampio per ridurre la probabilità di ulteriori collisioni, similmente a quanto avviene nel protocollo CSMA/CD (Carrier Sense Multiple Access with Collision Detection).

L'accesso al mezzo trasmissivo può essere gestito anche in modalità Point Coordination Function (PCF), oltre che in modalità distribuita (DCF). In modalità PCF, una stazione assume il ruolo di Point Coordinator (PC) e controlla centralmente l'accesso al mezzo, dando alle varie stazioni l'autorizzazione esplicita a trasmettere. La modalità PCF viene attivata mediante l'invio di una trama speciale dopo un intervallo di tempo chiamato PIFS (PCF Interframe Space), che è tale che SIFS (Short Interframe Space) è inferiore a PIFS, il quale a sua volta è inferiore a DIFS. Questo garantisce che non possa iniziare una finestra di contesa per trame "normali", che richiedono l'attesa di un intervallo DIFS, ma permette l'invio prioritario di `CTS` (Clear to Send) o `ACK` (Acknowledgement) dopo un intervallo SIFS. Durante la modalità PCF, il Point Coordinator interroga le singole stazioni (poll), che rispondono dopo un intervallo SIFS.
