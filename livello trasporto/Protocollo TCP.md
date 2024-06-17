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

### Formato e connessioni
##### Header del segmento TCP
![[Pasted image 20240322074105.png]]

Definiamo i contenuti dell'header:
- Sequence Number: numero di sequenza del primo byte nel payload
- Acknowledge Number: numero di sequenza del prossimo byte che si intende ricevere
- HLEN: lunghezza totale dell'header TCP, multiplo di 32
- Window: valore della finestra di ricezione come comunicato dal ricevitore al trasmettitore
- Checksum: medesimo di UDP
- Flags:
	- `URG`: Vale `1` se vi sono dei dati urgenti e quindi TCP deve passare in modalità urgente.
	- `ACK`: Vale `1` se il pacchetto è un ACK valido, cioè se contiene un acknowledge number valido
	- `PSH`: Vale `1` quando il trasmettitore intende usare il comando di PUSH: il ricevitore può anche ignorare il comando
	- `RST`: reimposta la connessione senza un tear down esplicito
	- `SYN`: usato durante il setup per comunicare i numeri di sequenza iniziale
	- `FIN`: usato per la chiusura esplicita di una connessione


