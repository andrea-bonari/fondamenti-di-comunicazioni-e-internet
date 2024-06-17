Un'applicazione di rete è un software che può essere eseguito su diversi terminali e possa comunicare con la rete, un esempio possono essere i web browser. La difficoltà nel creare un applicazione di rete sta nel creare un'applicazione che non richiede un software applicativo su tutti nodi della rete, rendendo quindi facile la diffusione solo nei terminali.

### Comunicazione tra processi
>[!tip] Host
>L'host è un dispositivo di rete utilizzato da un utente, per esempio laptop, smartphone, server.

>[!tip] Processo
>Un processo è un programma in esecuzione su un'host.

>[!tip] Comunicazione inter-processo
>Le tecnologie software di comunicazione inter-processo (IPC) hanno come scopo di consentire a diversi processi di comunicare scambiandosi informazioni e dati. Questa comunicazione può avvenire tra host diverso o sullo stesso host.

È fondamentale stabilire un protocollo da utilizzare, che definisce:
- I tipi di messaggi scambiati
- Sintassi dei messaggi
- semantica dei messaggi
- Regole su come e quando inviare e ricevere i messaggi

Per indirizzare ai servizi dei livelli inferiori si utilizzano i SAP (Service Access Point), associato ad ogni processo. Ad ogni SAP è associato un indirizzo IP dell'host e un numero di porta. Questa coppia di dati viene definita come socket.

>[!tip] Socket
>Le socket sono delle porte di comunicazione dove il processo trasmittente mette il messaggio fuori dalla porta, e successivamente la rete raccoglie il messaggio e lo trasporta fino alla porta del destinatario.

### Requisiti delle applicazioni

##### Affidabilità
Alcune applicazioni possono tollerare la perdita di pacchetti e informazione, mentre altre richiedono precisione assoluta.

##### Ritardo
Alcune applicazioni richiedono basso ritardo di trasmissione.

##### Banda

Alcune applicazioni richiedono una velocità minima di trasferimento, mentre altre si adattano alla velocità disponibile

---

In base a questi criteri possiamo scegliere due tipi di socket, che si basano sui protocolli TCP e UDP:

##### Servizio TCP
È orientato alla connessione, quindi prima dello scambio dei dati si instaura una connessione. È affidabile, quindi senza perdita di dati. Il trasmettitore regola la velocità in base al ricevitore. Controlla la congestione per non sovraccaricare la rete. Fornisce anche garanzie di ritardo e banda.

##### Servizio UDP
Non è orientato alla connessione, il trasferimento non è affidabile, non controlla il traffico e non fornisce garanzie, ma è estremamente veloce.

### Architetture Client-Server
>[!note]
>Nell'architettura client-server i dispositivi coinvolti nella comunicazione implementano o solo il processo client o solo il processo server, questo è perché hanno caratteristiche totalmente diverse. Infatti i client possono solo eseguire richieste mentre i server possono solo rispondere a queste richieste.

Per funzionare l'architettura client-server deve avere:
- server: Sempre attivo e con IP permanente
- server: Possibilità di usare macchine in cluster
- server: Possibilità di ricevere più richieste da più client
- client: Comunicano solo con il server e non con altri client

### Architetture Peer-to-Peer (P2P)
>[!note]
>È un architettura facilmente scalabile ma estremamente difficile da gestire. Questo perché I dispositivi implementano tutti sia il processo client che quello server. E non ci sono sempre server connessi, o facilmente indentificabili.
