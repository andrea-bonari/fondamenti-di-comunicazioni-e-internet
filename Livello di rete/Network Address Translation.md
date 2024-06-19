>[!tip] Intranet
>Un intranet non è altro che una rete privata che utilizza tecnologia di interconnessione IP, dotata degli stessi servizi dell'internet come server HTTP, posta, etc

L'evoluzione di servizi e protocolli ha reso però le Intranet strutturalmente differenti dalle reti pubbliche, ci sono problemi di sicurezza, di gestione degli indirizzi e distinzione tra i servizi offerti ai soli utenti della intranet e i servizi offerti anche agli utenti di internet.

Una rete privata normalmente ha una serie di servizi accessibili dalla rete pubblica, e i server per questi servizi devono avere un indirizzo pubblico mentre gli host interni alla rete devono avere indirizzo privato. Sorge quindi l'esigenza di consentire lo scambio di pacchetti tra host con l'indirizzo pubblico e host con indirizzo privato.

### Connessione con router semplice
Se l'intranet adotta indirizzamento pubblico, non è più definibile intranet in quanto fa parte di internet, e quindi sono possibili le comunicazioni da e verso internet. Questo metodo tuttavia fornisce una scarsa sicurezza.

### Connessione tramite Proxy Applicativo
Questo metodo funziona sia con indirizzamento pubblico che privato. L'intranet e internet sono scollegate a livello di IP. Ogni richiesta è inviata al proxy che la inoltra con il proprio IP pubblico, tuttavia questo metodo è scomodo poiché occorre avere un proxy per tutte le applicazioni.

### NAT
>[!note]
>I NAT hanno tutte le funzionalità dei router classici, e in più sanno anche gestire il mapping di uno spazio di indirizzamento privato in un altro spazio di indirizzamento pubblico. Consente quindi di associare, un numero ridotto di indirizzi pubblici ai numeri della numerazione privata.

Poiché il colloquio sia bidirezionale occorre creare una tabella di NAT, che può avere corrispondenza statica o dinamica.

Ogni NAT ha come caratteristiche:
- Transparent Address Translation: associazione trasparente alle stazione con modalità di associazione statica o dinamica.
- Transparent Routing: il routing è gestito in maniera coerente con l'indirizzamento
- ICMP Packet Translation: I messaggi ICMP contengono indirizzi IP, quindi vanno mappati.

>[!tip] Associazione dinamica nel NAT
>L'assegnamento si basa sul concetto di sessione. Quando il NAT vede il primo pacchetto di una sessione crea l'associazione tra indirizzo privato e pubblico, e al termine della sessione viene rilasciato.
>
>Nel caso di TCP l'inizio della sessione è identificato dal pacchetto di `SYN` mentre la fine è identificata da `FIN`. Nel caso dei protocolli UDP e ICMP, dove non esiste il concetto di sessione in quanto connectionless, esistono diversi metodi.
>
>Esiste sempre un sistema di timeout per recuperare situazioni d'errore o perdita di pacchetti.

>[!tip] Application Level Gateway
>Alcune applicazioni trasportano nel loro payload degli indirizzi IP (in formato ASCII o binario) e numeri di porta. Gli Application Level Gateway (ALG) sono funzionalità aggiuntive che si preoccupano di modificare i messaggi applicativi in transito, e in caso adattare i segmenti TCP.

Per quanto il NAT possa essere comodo:
- Impone il ricalcolo del checksum su ogni header e la sostituzione degli indirizzi IP (perdita di performance).
- Problemi con gli ALG nel trasporto di indirizzi a livello applicativo.

### Traditional NAT
>[!note]
>È una tipologia di NAT che permette solo sessioni iniziate dall'interno, quindi le informazioni di routing possono essere distribuite dall'esterno verso l'interno ma non viceversa. È anche chiamata Outbound NAT.

Esistono 2 sottotipi:
- Basic NAT: viene associato solo l'indirizzo IP. L'associazione è uno ad uno, quindi due host non possono usare lo stesso indirizzo pubblico contemporaneamente. È più facile un blocco a causa dello scarso numero di indirizzi pubblici.
- NAPT: viene associato sia la porta che l'indirizzo IP, quindi più indirizzi interni possono usare un indirizzo interno. Questo metodo però ci pone problemi con flussi diversi da TCP e UDP.

### Bidirectional NAT
>[!note]
>È una tipologia di NAT che permette di iniziare una sessione in entrambe le direzioni. Nel caso un host pubblico provi ad iniziare una sessione con un host privato occorre usare dei nomi simbolici con un servizio DNS privato.

