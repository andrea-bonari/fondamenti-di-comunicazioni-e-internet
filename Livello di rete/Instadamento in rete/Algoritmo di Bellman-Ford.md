>[!note]
>Ipotizzando che i pesi siano di qualsiasi segno e che non esista alcun ciclo di lunghezza negativa, l'algoritmo di Bellman-Ford ha come scopo:
>- trovare i cammini minimi tra un nodo sorgente $s$ e tutti gli altri nodi
>- trovate i cammini minimi da tutti i nodi ad un nodo destinazione $d$
>
>Definendo $D^{(h)}_{i}$ come lunghezza del cammino minimo tra il nodo $s$ e il nodo $i$ composto da un cammino di archi $\leq h$:
>$$\begin{align*}
&D^{h}_{s}= 0&&\forall h\\
&D^{(0)}_{i}=\infty&&\forall i\neq0\\
&D^{(h+1)}_{i}=\min\left\{ D_{i}^{(h)},\min_{j}(D^{(h)}_{j}+d_{ij})\right\}
\end{align*}$$
> L'algoritmo termina in $N-1$ passi.

### Esempio codice dell'algoritmo di Bellman-Ford
```ts
interface Edge {
    src: number;
    dest: number;
    weight: number;
}

function bellmanFord(graph: Edge[], V: number, E: number, src: number): void {
    // Passaggio 1: Inizializza le distanze da fonte a tutti gli altri vertici come INFINITO
    let distance: number[] = Array(V).fill(Infinity);
    
    // Distanza da fonte a se stessa è sempre 0
    distance[src] = 0;
    
    // Rilassa gli archi |V| - 1 volte
    for (let i = 1; i < V; i++) {
        for (let j = 0; j < E; j++) {
            const u = graph[j].src;
            const v = graph[j].dest;
            const weight = graph[j].weight;
            
            if (distance[u]!== Infinity && distance[u] + weight < distance[v]) {
                distance[v] = distance[u] + weight;
            }
        }
    }
    
    // Controlla la presenza di cicli negativi
    for (let j = 0; j < E; j++) {
        const u = graph[j].src;
        const v = graph[j].dest;
        const weight = graph[j].weight;
        
        if (distance[u]!== Infinity && distance[u] + weight < distance[v]) {
            console.log("Il grafo contiene un ciclo di peso negativo");
            return;
        }
    }
    
    // Stampa l'array delle distanze costruito
    console.log(distance);
}

// Utilizzo dell'esempio
const graph: Edge[] = [
    { src: 0, dest: 1, weight: -1 },
    { src: 0, dest: 2, weight: 4 },
    { src: 1, dest: 2, weight: 3 },
    { src: 1, dest: 3, weight: 2 },
    { src: 1, dest: 4, weight: 2 },
    { src: 3, dest: 2, weight: 5 },
    { src: 3, dest: 1, weight: 1 },
    { src: 4, dest: 3, weight: -3 }
];

bellmanFord(graph, 5, 8, 0);

```

### Forma distribuita
>[!note]
>L'informazione sulla connettività scambiata dai nodi è costituita dal Distance Vector (DV): $$[\text{indirizzo destinatario}, \text{ distanza}]$$
>Il DV è inviato periodicamente (o a seguito di un cambiamento nella topologia di rete) ai soli nodi adiacenti. A questo punto ogni nodo esegue un ricalcolo delle distanze se:
>- Riceve un DV diverso da quello memorizzato in precedenza
>- cade/nasce una linea attiva a cui è connesso.
>
>Il ricalcolo avviene con la formula: $$D'_{j}=\min\left[ D_{j}, \min_{k}[D_{k}+d_kj]\right]$$

Processo di ricezione DV da un vicino:
1. Incrementiamo la distanza dalle destinazioni specificate del costo del link di ingresso.
2. Per ogni destinazione specificata nel DV:
	- Se la destinazione non è nella tabella di routing l'aggiungiamo.
	- Se il next-hop nella tabella di routing corrisponde al mittente del DV sostituiamo l'informazione della tabella di routing con quella nuova.
	- Se la distanza nel DV è minore di quella scritta nella tabella di routing sostituiamo l'informazione della tabella di routing con quella nuova.

### Caratteristiche

**Vantaggi**:
- Facile da implementare
**Svantaggi**
- Problema della velocità di convergenza (crescita del tempo proporzionale al numero di nodi)
- Limitato dal nodo più lento
- Dopo un cambiamento possono sussistere dei cicli per un tempo anche lungo
- Comportamento stabile difficile da mantenere su reti grandi
- problema della stabilità (counting to infinity)

### DV e counting to infinity

>[!error] Counting to infinity
>Se si rompe un arco che è fondamentale per collegare due nodi (bridge) si può creare un loop infinito tra due nodi perché le tabelle di routing si aggiornano con distanza infinita tra quei due nodi (non convergono).

Per rimediare al problema esistono 4 modi:
- Hop Count limit: il counting to infinity termina se si utilizza la convenzione di rappresentare l'infinito mediante un valore finito, più grande del percorso più lungo della rete. Tuttavia la convergenza verso uno stabile è lenta.
- Split-Horizon: Il nodo $A$ non annuncia a $D$ con quale costo raggiunge $X$, quindi il nodo $A$ manda messaggi di routing diversi sui link locali. Esiste una forma semplice (omissione dell'informazione) e una forma "Poisonous Reverse" (sostituzione dell'informazione con distanza infinita). Tuttavia non funziona in certe topologie.
- Hold-down: Quando un router rileva una rotta non attiva, la contrassegna come in stato di "hold down". Questo accade quando il router riceve un Distance Vector (DV) con distanza infinita per quella rotta o quando non riceve il DV che segnala la rotta dal nodo del primo hop per un determinato tempo $T_{\text{invalid}}$. Durante lo stato di hold down, la rotta non viene annunciata nei DV, o viene annunciata con un costo infinito. Inoltre, i DV ricevuti da altri nodi che riportano metriche peggiori rispetto a quella corrente non sono considerati validi per quella rotta. Tuttavia, i DV migliorativi da altri router sono considerati validi, consentendo alla rotta di uscire dallo stato di hold down. Dopo un periodo di tempo $T_{\text{flush}}$​, la rotta in hold down viene cancellata. Il tempo tra $T_{\text{invalid}}$​ e $T_{\text{flush}}$ deve essere impostato in modo tale che le informazioni relative a un cambiamento (come un guasto) possano propagarsi correttamente nella rete.
- Triggered Update: I cambiamenti di topologia sono annunciati immediatamente e distinti dagli altri. Aumenta la velocità di convergenza e fa scoprire prima i guasti.
