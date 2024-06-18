>[!note]
>Lo strato di rete si incarica di trasferire i dati tra gli host che ospitano due processi comunicanti. È suddiviso in due piani:
>- Piano dati: Protocolli per il trasferimento dati
>- Piano di controllo: Protocolli di segnalazione per supportare il trasferimento dati
>
>![[Pasted image 20240618100012.png]]

Questi protocolli sono implementati in ogni host e ogni router.
### Funzioni dello strato di rete

Le funzioni principali sono:
- Indirizzamento: fornisce un identificazione univoca dell'interfaccia di rete di un host/router
- Forwarding: funzione locale con cui il router trasferisce i pacchetti dall'ingresso in uscita
- Routing: Processo che determina i percorsi dei pacchetti dalla sorgente alla destinazione, è svolto dagli algoritmi di routing. Può essere centralizzato/distribuito, statico/dinamico, manuale/automatico.

### Data plane a livello di rete

I segmenti dello strato di trasporto vengono trasferiti dallo strato di rete dell'host sorgente all'host destinazione nel seguente processo: 
1. I dati nel lato sorgente vengono incapsulati in datagram.
2. I datagram sono inoltrati hop-by-hop fino a destinazione.
3. I router esaminano i campi dell'header di ciascun datagram IP che li attraversa.
4. Al lato destinazione i datagram sono spacchettati e i segmenti sono consegnati allo strato trasporto.

Nel data plane risiede il protocollo IP, che trasferisce l'informazione (best effort, connectionless). Ogni router che riceve un datagramma legge l'header e decide come e dove inoltrare il datagramma sulla base dell'indirizzo di destinazione e sulla sua tabella di instradamento. Infatti i pacchetti possono percorrere strade diverse tra sorgente e destinazione. Non ci sono controlli di errore.