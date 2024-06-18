>[!note]
>IP è una tecnica di internetworking, di conseguenza nel trasferimento di datagrammi tra due host si serve della capacità di inoltro delle reti locali attraversate. Infatti si parla di:
>- Inoltro diretto: quando la destinazione è nella stessa rete IP
>- Inoltro indiretto: quando la destinazione non è nella stessa rete IP

Quando il pacchetto è trasmesso tra reti locali è incapsulato in trame di secondo livello e si basa sugli indirizzi di secondo livello (indirizzi MAC).

### Inoltro negli host
Consideriamo la rete locale contenente gli host A e B, e il router C.

**Inoltro diretto**
1. A deve spedire un pacchetto a B.
2. A Riconosce che B fa parte della stessa rete.
3. A consulta la sua tabella di routing della rete e reperisce l'indirizzo MAC di B.
4. A questo punto A passa il pacchetto al livello inferiore che creerà una trama con destinazione il MAC di A.

**Inoltro indiretto**
1. A deve spedire un pacchetto a D, che è fuori dalla rete locale
2. A riconosce che D non fa parte della stessa rete.
3. A consulta la sua tabella di routing della rete e reperisce l'indirizzo MAC di C.
4. A questo punto A passa il pacchetto al livello inferiore che creerà una trama con destinazione il MAC di C.

### Inoltro nei router
>[!note]
>Analogamente agli host anche i router seguono tecniche di inoltro diretto e indiretto, tuttavia:
>- Inoltro diretto: hanno più di un interfaccia dove poter effettuare l'inoltro diretto.
>- Inoltro indiretto: si basa su tabelle di routing dove è definita la rotta di instradamento.

L'inoltro ha le seguenti caratteristiche:
- Destination based: L'inoltro IP è basato sul solo indirizzo di destinazione
- Next Hop Routing: Nelle tabelle di routing per ogni rete è indicato solo il prossimo router (next-hop) nel percorso verso la destinazione.

L'inoltro avviene da router a router attraverso le reti IP. Tutti gli host che appartengono alla rete di destinazione sono identificati nelle tabelle di routing da una singola entry.

In ogni tabella di routing deve sempre essere indicata per ogni entry in modo esplicito la lunghezza del NetID. Infatti i protocolli di routing devono supportare l'invio dell'informazione di netmask insieme agli indirizzi di rete in tutti i messaggi di annuncio di rotte. In passato non tutti i protocolli lo consentivano.