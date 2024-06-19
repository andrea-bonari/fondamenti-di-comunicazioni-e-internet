>[!note]
>Non è possibile applicare DV o LS direttamente a tutta Internet per problemi di scalabilità e sicurezza.

### Architettura di routing in internet
Internet è suddiviso in Autonomous Systems (AS), cioè in porzioni di reti gestite dalle stesse autorità. Si definisce un router interno ad un AS Interior Gateway, mentre un router al bordo come Exterior Gateway. Ad ogni AS è attribuito da IANA un identificativo detto ASN.

Il routing di un AS è quindi indipendente da quello di altri AS. Gli Interior Gateway si scambiano informazioni di routing usando un Interior Gateway Protocol (IGP), mentre il routing tra AS è gestito usando un approccio diverso da DV/LS usando un protocollo chiamato Exterior Gateway Protocol (EGP).

### Tipologie di inoltro
- **Diretto**: Il NetID degli host e destinazione coincidono, quindi il forwarding è effettuato mediante la rete di livello 2.
- **Indiretto**: Il NetID degli host sorgente e destinazione sono diversi ma appartengono allo stesso AS, quindi il forwarding avviene mediante IGP.
- **Indiretto Gerarchico**: Host e sorgente sono in AS diversi, quindi il pacchetto è instradato usando IGP fino all'Exterior Gateway, usando EGP raggiunge l'AS destinazione, che instraderà poi il pacchetto con IGP fino all'host destinazione.

Nella modalità indiretto gerarchico l'EGP annuncia le destinazioni esterne raggiungibili. Infatti gli Exterior Gateway si scambiano informazioni sintetiche di raggiungibilità.

### Interior Gateway Protocols (IGPs)
>[!note]
>In un AS possono essere configurati più IGP, a patto che siano gestiti in modo da garantire la consistenza del routing. Il caso più frequente tuttavia è l'uso di un solo IGP.

Per supportare molteplici IGP viene definito un Routing Domain (RD), cioè una porzione di AS in cui è implementato un unico protocollo IGP. Siccome alcuni router appartengono a più RD implementano più protocolli su diverse interfacce.

>[!tip] Ridistribuzione
>I router su più domini possono tradurre informazioni per scambiarle tra domini. La traduzione dipende dall'implementazione e dalle caratteristiche dei domini. È valido anche lo scambio di informazioni tra IGP e EGP.

I protocolli IGP più usati sono:
- Distance Vector:
	- RIP (Routing Information Protocol)
	- IGRP (Interior Gateway Routing Protocol)
- Link State:
	- IS-IS (Intermediate System Intermediate System)
	- OSPF (Open Shortest Path First)

### Exterior Gateway Protocols (EGPs)
Il protocollo EGP più usato è BGP (Border Gateway Protocol) e implementa un sistema chiamato Path Vector.