Ogni pagina web è fatta da oggetti, che possono essere file HTML, CSS, JS, immagini, applet Java, media, ecc.

Ogni oggetto è indirizzato da un Uniform Resource Locator (URL):
$$\underbrace{\text{http}}_\text{protocollo applicativo}://\underbrace{\text{www.polimi.it}}_{\text{indirizzo di rete del server}}:\underbrace{\text{80}}_{\text{numero di porta}}/\underbrace{\text{index.html}}_\text{risorsa richiesta}$$
### La comunicazione HTTP
Viene utilizzata l'architettura Client-Server pura, cioè nessuna memoria delle richieste viene mantenuta nei server sulle richieste ricevute dal client (protocollo stateless).

Per avviare una comunicazione HTTP:
1. Il client HTTP inizia una connessione TCP verso il server
2. Il server HTTP accetta connessioni TCP da client HTTP
3. Client e server HTTP si scambiano informazioni
4. La connessione TCP tra client e server HTTP viene chiusa

### Modalità di connessione HTTP

##### Connessione non persistente
>[!note]
>È una connessione TCP usata per una sola sessione richiesta-risposta. Il server chiude la la connessione TCP una volta inviato l'oggetto.

##### Connessione persistente
>[!note]
>La connessione TCP rimane aperta ed è usata per trasferire più oggetti della stessa pagina web. Può essere utilizzata con pipelining (richieste HTTP inviate in serie) o senza pipelining (richieste HTTP inviate in parallelo).

### Stima del tempo di trasferimento in HTTP

>[!tip] Round Trip Time (RTT)
>Il Round Trip Time è il tempo per trasferire un messaggio dal client al server e viceversa.

Il tempo di risposta di HTTP è composto da un RTT per iniziare la connessione TCP, un RTT per inviare i primi byte della richiesta HTTP e ricevere i primi byte di risposta e il tempo di trasmissione dell'oggetto.

Il tempo di trasmissione per trasmettere $n$ oggetti dove il primo è una pagina HTML è:
$$T_{\text{non persistente}}=\sum\limits_{i=1}^{n}2\text{RTT}+ T_{i}$$
$$T_\text{persistente} = \text{RTT}+\sum\limits_{i=1}^{n}\text{RTT}+T_{i}$$
### Richiesta HTTP
![[Pasted image 20240304121641.png]]
##### Metodi HTTP
>[!note] GET
>Usato quando il client vuole scaricare una risorsa dal server. Il documento è specificato solo nell'url (non esiste il body). Il server risponde con la risorsa richiesta nel body

>[!note] HEAD
>Usato quando il client vuole scaricare solo alcune informazioni riguardo una risorsa

>[!note] POST
>Usato per caricare dei dati sul server da utilizzare per un oggetto identificato nell'url

>[!note] PUT
>Utilizzato per memorizzare dei dati nel server. La risorsa è fornita nel `body` e la posizione è nell'url

>[!note] DELETE
>Cancella la risorsa specificata nell'url

>[!tip] Conditional GET
>Aggiungendo un header `If-modified-since: <data>` a una richiesta HTTP il server ci restituirà l'oggetto solo se la copia presente sul client è aggiornata. In caso alternativo la status line della risposta sarà `HTTP/1.0 304 Not Modified`

### Risposta HTTP
![[Pasted image 20240304122806.png]]

Lo status code può essere utilizzato per identificare i contenuti del messaggio, generalmente sono identificati in:
- **1xx**: informazione
- **2xx**: successo
- **3xx**: redirezione
- **4xx**: errore lato client
- **5xx**: errore lato server

