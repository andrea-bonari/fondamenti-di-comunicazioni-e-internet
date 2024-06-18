In una architettura di file sharing P2P gli utenti si collegano in modo intermittente a internet, utilizzando indirizzi IP dinamici (che cambiano ogni volta). Ogni utente sceglie da quale degli altri utenti scaricare il file tramite un protocollo come HTTP. È importante sapere che le applicazioni P2P sono sia client che server.

Di seguito riportiamo dei metodi per effettuare del file sharing P2P.

### P2P directory centralizzata
Questo è il metodo usato dal software Napster

Quando i peer si connettono informano un server centrale del loro indirizzo IP e dei file condivisi. Quando un peer vuole richiedere un file interroga il server centrale, che indirizzerà il peer richiedente al peer del file.

Avendo un nodo centrale con cosi tante informazioni il server è un bottleneck e un rischio, sia legale (accuse riguardo i file distribuiti) che tecnico (se si blocca il server il sistema smette di funzionare). Inoltre questo metodo distribuisce solo il download dei file, ma non l'indicizzazione.

### P2P directory completamente distribuita
Questo metodo è usato da Gnutella.

A differenza del protocollo precedente non ha nessun server centrale e il protocollo è di pubblico dominio, questo permette a molteplici software di basarsi sullo stesso protocollo.

È basato su un grafo di overlay, dove i peer si attivano e si collegano ad un numero (< 10) di altri "vicini" (neighbors), e la loro ricerca è distribuita.

Di seguito degli esempi di questo protocollo:

**Ricerca di un file**:
- I messaggi di richiesta vengono diffusi sulla rete di overlay
- I peer inoltrano le richieste fino a una certa distanza
- Le risposte vengono inviate sul cammino opposto

**Accesso alla rete**:
- Per iniziare l'accesso il peer $X$ si basa su liste note per trovare degli altri peer
- $X$ scandisce la lista fino a quando un peer $Y$ non risponde.
- $X$ invia un messaggio di ping a $Y$, che lo inoltrerà nella rete di overlay
- Tutti i peer che ricevono i messaggi di ping rispondono con un messaggio di pong, e $X$ può scegliere a chi connettersi

### BitTorrent
Il protocollo di P2P file sharing moderno più conosciuto.

In questo protocollo i file sono divisi in chunk di $256\text{ kB}$. Per effettuare l'accesso un peer si registra presso un tracker e ottiene una lista di peer attivi. Il peer entrante stabilisce connessioni TCP con un sottoinsieme dei peer della lista, che invieranno al peer entrante la lista dei chunk disponibili. A questo punto il peer entrante può scegliere quale chunk scaricare secondo meccanismi euristici.

**Meccanismo di richiesta chunk**:
Si utilizza il principio Rarest-First:
Il peer entrante , tra tutti i chunk mancanti, scarica prima i chunk più rari nelle liste di chunk ricevute da tutti i neighboring peer.

**Meccanismo di invio chunk**:
Il peer entrante risponde alle richieste che provengono dagli $x$ peer che inviano chunk al massimo rate, tutti gli altri sono strozzati. I migliori $x$ peer sono ricalcolati periodicamente, e ogni 30 secondi un nuovo peer viene scelto casualmente per l'invio di chunk (optimistic unchoking).

