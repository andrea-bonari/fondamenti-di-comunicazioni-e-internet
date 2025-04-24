>[!note]
>L'algoritmo di Dijkstra si applica al generico nodo sorgente $s$.

### Complessità
A differenza dell'algoritmo di Bellman-Ford, che ha complessità asintotica $\mathcal{O}(N^{3})$, l'algoritmo di Dijkstra ha complessità $\mathcal{O}(N^{2})$, e quindi è più conveniente.

### Caratteristiche
**Vantaggi**:
- Più flessibile in quanto ogni nodo ha una mappa completa della rete
- Non è necessario inviare LSP periodicamente ma solo dopo un cambiamento
- Tutti i nodi vengono subito informati dei cambiamenti
**Svantaggi**:
- È necessario un protocollo dedicato a mantenere l'informazione sui vicini
- È necessario l'uso del flooding
- È necessario un riscontro dei pacchetti di routing inviati
- Difficile da implementare

### Protocolli di routing e Link state
Ogni nodo riconosce i nodi e le destinazioni adiacenti, e le distanze per raggiungerle. A questo punto il nodo invia tutti gli altri nodi della rete (flooding) mediante LSP (Link State Packet). Tutti i nodi costruiscono quindi un database di LSP e una mappa completa della tipologia della rete. Sulla base di questa informazione vengono calcolati i cammini minimi verso tutte le destinazioni.

>[!tip] Flooding
>Ogni pacchetto in arrivo viene ritrasmesso su tutte le uscite eccetto quella da cui è stato ricevuto. Occorre prevenire i loop e di conseguenza si usa un numero di sequenza (e un database che li memorizza) e un TTL.

