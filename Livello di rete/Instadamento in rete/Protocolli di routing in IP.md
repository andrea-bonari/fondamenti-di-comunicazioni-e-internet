### Routing Information Protocol (RIP)
>[!note]
>RIP si affida ad UDP come protocollo di trasporto usando la porta 520. Si basa su Distance Vector e come metrica di costo usa il numero di salti. La distanza massima è di 15 salti e le info di aggiornamento sono inviate tipicamente ogni 30 secondi.

**Vantaggi**:
- Implementazione facile
**Contro**:
- Complessità elevata ($\mathcal{O}(N^{3})$)
- Possibilità di loop
- Possibilità di count to infinity

Le richieste sono inviate dal router quando si è appena connesso oppure ha alcune entry in scadenza. Con le request è possibile richiedere ai propri vicini entry specifiche. Le response possono essere Solicited (risposte ad una richiesta) o Unsolicited (inviate periodicamente).

RIP ha alcuni problemi:
- Il numero di salti è una misura troppo semplice, si potrebbe estendere la metrica lasciando l'algoritmo invariato
- RIP converge estremamente lentamente: I router a distanza $N$ hanno bisogno di $N$ passi dell'algoritmo per arrivare a conoscere l'effettiva distanza che li separa.
- Limitato a reti di piccole dimensioni

### Open Shortest Path First (OSPF)
> [!note]
> OSPF si basa sull'algoritmo link state. A differenza di RIP invia LSP con tecnica flooding con poche informazioni. La metrica di costo è generica e può essere configurata dall'amministratore di rete. Inoltre dispone di un servizio di load balancing dinamico ottenuto specificando rotte multiple con lo stesso costo.

### Border Gateway Protocol (BGP)
>[!note]
>BGP consente a router di diversi AS di scambiarsi informazioni di routing. Usa un algoritmo di tipo path vector, cioè i gestori di AS decidono il routing in base a delle proprie politiche. È indipendente dai protocolli IGP adottati dentro gli AS. Esistono due tipologie di BGP:
>- external BGP (eBGP): usato da diversi AS border routers per scambiare tra loro informazioni di routing.
>- internal BGP (iBGP): usato da un singolo AS border router per propagare l'informazione di routing dentro l'AS.

>[!tip] Path Vector (PV)
>PV è simile a DV, ma non usa la distanza dalla destinazione, bensì l'intero percorso verso la destinazione.

In realtà un messaggio di BGP non contiene solo il percorso ma una sequenza di attributi.
Tra gli attributi obbligatori:
- ORIGIN: protocollo IGP da cui proviene l'informazione
- AS_PATH: sequenza di AS attraversati (usando ASN)
- NEXT_HOP: prossimo router


Ogni router BGP invia il proprio path vector ai router BGP vicini, l'informazione del path vector è trasmessa su connessioni TCP sulla porta 179. I vari tipi di messaggi sono:
- `OPEN`: apre la connessione TCP e gestisce l'autenticazione reciproca dei router.
- `UPDATE`: annuncia una nuova rotta
- `KEEPALIVE`: mantiene attiva la connessione in caso di assenza di `UPDATE` (usato anche come `ACK` in risposta a `OPEN`)
- `NOTIFICATION`: notifica errori in messaggi precedenti
