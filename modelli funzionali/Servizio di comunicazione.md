>[!note] Servizio di comunicazione
>Date due o più entità remote possiamo descrivere il servizio di comunicazione per lo scambio di messaggi come un fornitore del servizio di trasporto dell'informazione

![[Pasted image 20240221134132.png]]

### Primitive di servizio
>[!note]
>Le primitive di servizio servono a descrivere il servizio, a richiedere e a ricevere informazioni sul servizio dal fornitore. Queste primitive possono essere:
>- Messaggi applicativi (parole, messaggi di email, ecc.)
>- Flussi multimediali (audio, video, ecc.)
>- Gruppi di bit (pacchetti)
>- Bit

Le primitive di servizio sono caratterizzate da parametri tra cui:
- informazioni da trasferire
- indicazione del destinatario
- caratteristiche del servizio richiesto

![[Pasted image 20240221134904.png]]

### Modalità di colloquio

>[!note] Modalità a connessione
>Strutturata in 3 fasi:
>1. instaurazione della connessione
>2. trasferimento dell'informazione
>3. rilascio della connessione

>[!note] Modalità senza connessione
>Strutturata in una singola fase dove tutte le informazioni accessorie vengono trasferite assieme all'informazione stessa.

### Protocollo di comunicazione
>[!note] Protocollo
>Il protocollo è un insieme di regole che gestisce il colloquio tra entità. Determina regole come:
>- formato dei messaggi
>- informazioni di servizio
>- algoritmi di trasferimento
>- ecc.

>[!tip] Packet Data Units (PDU)
>Un protocollo utilizza per il colloquio tra entità dello stesso livello delle unità di trasferimento dati dette PDU. Le PDU sono composte da:
>- Header: informazione di servizio necessaria al coordinamento tra le entità
>- Dati: informazione ricevuta dai livelli superiori

