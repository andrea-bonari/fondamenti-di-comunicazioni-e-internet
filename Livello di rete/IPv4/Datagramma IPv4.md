>[!note]
>Il protocollo IP è connectionless (paradigma packed-oriented, due pacchetti destinati allo stesso host possono essere trattati in maniera diversa) e non affidabile (consegna best-effort), inoltre offre servizi di indirizzamento e frammentazione/deframmentazione (se il livello inferiore lo richiede).
>
>Le unità informative IP sono dette pacchetti IP e eseguono una segnalazione di errori e una garanzia sul massimo tempo di vita.

### Header IPv4
> [!note]
> ![[Pasted image 20240618140741.png]]
> 
> Definiamo i contenuti dell'header:
> - Ver: indica la versione del protocollo (IPv4 o IPv6). Se non è riconosciuta il pacchetto è scartato
> - HLEN: indica la lunghezza dell'header in multipli di 32 bit
> - Total length: Lunghezza totale del pacchetto in byte
> - TOS (Type Of Service): Si suddivide in <br> ![[Pasted image 20240618141210.png]]
> 	- DSCP (Differentiated Services Code Point):
> 		- `Default`: 0
> 		- `Expedited Forwarding`/`Voice Admit`: basso ritardo
> 		- `Assured Forwarding`: Differenti mix di classe di priorità di servizio e precedenza di dropping in caso di congestione
> 		- `Class Selector`: 8 differenti classi di servizio
> 	- ECN (Explicit Congestion Notification): usato nei router per segnalare un imminente congestione alle destinazioni
> - Protocol: indica il protocollo di livello superiore
> - TTL (Time To Live): impostato ad un valore elevato dal mittente e decrementato da ogni router attraversato. Se raggiunge 0 il pacchetto è scartato
> - Options: originariamente pensati per testing e debugging, ora raramente usati e di norma filtrati dai router perché considerati un rischio

### Frammentazione e ricostruzione
La frammentazione è distribuita sui diversi link in base alle necessità, mentre la ricostruzione avviene centralizzata all'arrivo dei frammenti del pacchetto. I link di rete possono imporre un limite alla dimensione delle trame di 2 livello detto MTU, dove diversi link hanno diversi MTU.

Questo costringe a dividere il datagram IP troppo lungo in più frammenti, che riusciranno a stare in una sola trama.

Ogni frammento può essere ulteriormente frammentato durante il cammino, e ogni frammento ha l'header IP con aggiunti i seguenti campi:
- Identification: identifica tutti i frammenti di uno stesso pacchetto in modo univoco, scelto dall'IP che effettua la frammentazione
- Frag. Offset: I byte del pacchetto originale sono numerati da 0 al valore della lunghezza totale. Questo campo riporta il numero di sequenza del primo byte del frammento.
- Flags: 3 bit, il primo non è utilizzato, il secondo è M e il terzo è D
	- M (More): è pari a zero solo se è l'ultimo frammento
	- D (Do not fragment): è pari a 1 quando non si vuole che lungo il percorso venga applicata la frammentazione. In caso fosse necessaria il pacchetto sarà scartato.

