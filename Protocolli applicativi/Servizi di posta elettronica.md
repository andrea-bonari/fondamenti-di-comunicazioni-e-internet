I componenti necessari per il funzionamento della posta elettronica sono:
- Client d'utente (`user-agent`)
- Mail server
- Protocolli di invio e d'accesso al mail server (SMTP, POP3 e IMAP)

![[Pasted image 20240311115522.png]]

### Mail server
I mail server sono sono dei server che per ogni client controllato gestiscono una coda di email in ingresso e una coda di email in uscita. Comunicano con gli altri server utilizzando i protocolli SMTP e POP3/IMAP

### SMTP
>[!note]
>SMTP è un protocollo applicativo client-server, codificato in modo testuale, e l'interazione tra client e server SMTP è di tipo comando/risposta.
>
>I documenti binari devono essere convertiti in ASCII 7-bit

>[!example] Esempio di ricezione messaggio da client utente
>- Inserisce il messaggio in coda
>- Apre una connessione TCP (porta 25) con il server del destinatario
>- Trasferisce il messaggio
>- Chiude la connessione TCP

### Formato email
>[!note]
>Il formato dei messaggi inviati email è composto da:
>- `Header`
>	- `To`
>	- `From`
>	- `Subject`
>- `Body`
>
>  L'estensione MIME estende il formato dei messaggi email per supportare contenuti multimediali, definisce nell'header le opzioni `MIME-Version`, `Content-Transfer-Encoding` e `Content-Type`.
>  
>  MIME consente anche il trasferimento di più oggetti come parti di uno stesso messaggio.

### Protocolli di accesso al mailbox
Esistono diversi protocolli per permettere il colloquio tra user agent e server per la lettura del mailbox, alcuni sono:
- POP3: consente il download dei messaggi dal server, e i messaggi sono poi eliminati dal server.
- IMAP: consente il download dei messaggi e la gestione della mailbox sul mail server.
- HTTP

