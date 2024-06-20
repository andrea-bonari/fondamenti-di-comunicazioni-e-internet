### Servizio di trasporto
>[!note]
>Il livello trasporto ha il compito di instaurare un collegamento logico tra le applicazioni residenti su host remoti, e di conseguenza rende trasparente il trasporto fisico alle applicazioni.
>
>Siccome più applicazioni possono essere attive su gli host il livello trasporto svolge funzioni di multiplexing/demultiplexing.

### Indirizzamento, socket e multiplazione
Le funzioni di multiplexing/demultiplexing sono gestite tramite indirizzi lunghi 16 bit contenuti nelle PDU del livello trasporto, di conseguenza i numeri di porta possono assumere valori compresi tra 0 e 65535.
![[Pasted image 20240322071009.png]]

### Servizio di buffering
Quando un processo viene associato ad una porta viene associato dal sistema operativo a due code, una d'ingresso e una d'uscita, che forniscono una funzionalità di buffering dati.

### Affidabilità della connessione
Siccome il servizio di rete non è affidabile, il servizio trasporto cerca di mitigare questo problema, infatti si può distinguere in:
- Trasporto affidabile (garanzia di consegna dei messaggi nell'ordine corretto)
- Trasporto non affidabile (solo funzionalità di multiplexing)
Inoltre si può dividere anche in trasporto orientato o non orientato alla connessione.

Distinguiamo quindi i protocolli trasporto del modello TCP/IP:
- TCP (Transmission Control Protocol): Orientato alla connessione e affidabile
- UDP (User Datagram Protocol): senza connessione e non  affidabile