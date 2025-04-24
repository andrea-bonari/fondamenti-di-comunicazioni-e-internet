>[!note]
>L'instradamento è alla base delle funzionalità di rete implementata dalle entità di livello 3 dei nodi. Consente a due nodi A e B, non collegati direttamente, di comunicare tra loro mediante la collaborazione di altri nodi.

L'algoritmo di routing definisce i criteri di scelta del cammino nella rete per i pacchetti che viaggiano tra un nodo d'ingresso ed uno di uscita. Costruisce quindi le tabelle di routing che vengono usate dai nodi per effettuare il forwarding. Il tipo di rete determina il tipo di tabelle da utilizzare e i gradi di liberà della politica di routing nella scelta dei cammini.

### Protocolli di routing
>[!note]
>Si indicano come protocolli di routing due funzionalità:
>- scambio fra router di informazioni di raggiungibilità
>- costruzione delle tabelle di routing
>
>Formalmente il protocollo è solo la parte che descrive lo scambio di messaggi tra i router, ma in realtà questo scambio è strettamente legato al modo in cui sono calcolate le tabelle di routing.

>[!example] Routing a capacità
>Se il traffico viene fatto passare da pochi cammini nella rete il traffico massimo sarà basso, viceversa se si usano molti cammini il massimo traffico sarà elevato.

### Politiche di routing per internet
Il tipo di inoltro usato dalle reti IP condiziona la scelta delle politiche di routing. Siccome il forwarding IP è basato sull'indirizzo di destinazione e con inoltro al nodo successivo, I pacchetti diretti ad una stessa destinazione $D$ che giungono in router $R$ seguono lo stesso percorso da $R$ verso $D$ indipendentemente dal link di ingresso in $R$.

Quindi il vincolo che ogni politica di routing deve soddisfare è che l'insieme dei cammini da ogni sorgente verso una destinazione $D$ sia un albero, per ogni destinazione $D$.

### Routing IP a cammini minimi
>[!note]
>Il principio su cui si basa il routing IP è molto semplice: Inviare i pacchetti sul cammino minimo verso la destinazione. Il calcolo per i cammini minimi è distribuito dai router mediante uno scambio di informazioni con gli altri router. Nella tabella viene indicato solo il primo router sul cammino grazie alla proprietà secondo la quale anche i sotto-cammini di un cammino minimo sono minimi.

Il calcolo è effettuato sul grafo che rappresenta la rete e ad ogni arco è associato un "peso" opportunamente scelto. I cammini minimi verso una destinazione formano il Minimum Spanning Tree (MST). Esistono degli algoritmi di calcolo dei MST che possono essere eseguiti in modo distribuito dai router.

### Richiamo sui grafi
>[!note]
>![[Pasted image 20240619150803.png]]
>
>1. Digrafo:
>	- $N$ nodi
>	- $A=\set{(i,j)\space|\space i,j\in N}$ archi (coppia ordinata di nodi)
>2. Percorso: $(n_{1},n_{2},\cdots,n_{I})$ insieme di nodi $(n_{i},n_{i+1})\in A$
>3. Cammino: percorso senza nodi ripetuti
>4. Ciclo: percorso con $n_{1}=n_{I}$
>5. Digrafo connesso: per ogni coppia $i,j$ esiste almeno un cammino $i\to j$
>6. Digrafo pesato: $d_{ij}$ peso associato all'arco $(i,j)\in A$
>7. Lunghezza di un cammino: considerato un cammino $(n_{1},n_{2},\cdots, n_{I})$ la sua lunghezza è $d_{n_{1},n_{2}}+d_{n_{2},n_{3}}+\cdots+d_{n_{I-1},n_{I}}$

