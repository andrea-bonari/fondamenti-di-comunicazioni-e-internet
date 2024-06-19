>[!note]
>ICMP è un protocollo di messaggi di servizio fra host e router per informazioni su errori e fasi di attraversamento della rete. I messaggi ICMP sono incapsulati e trasportati da IP, infatti può essere considerato un utente di IP.
>![[Pasted image 20240619100617.png]]

### Funzionalità di error reporting
I messaggi di errore contengono l'header del pacchetto IP che li ha generati e i suoi primi 8 byte di dati.

>[!tip] Destination Unreachable (type 3, code 0-12)
>È inviato quando un router scarta un pacchetto per qualche motivo. Viene inviato al mittente del pacchetto. Nel campo code è codificato il motivo dell'errore. La generazione avviene soltanto quando il router si accorge del problema.

>[!tip] Time Exceeded (type 11, code 0-1)
>Il code 0 viene inviato alla sorgente del pacchetto quando un router decrementa il suo TTL a 0, mentre il code 1 è inviato dalla destinazione quando tutti i frammenti di un pacchetto non arrivano entro un tempo massimo.

>[!tip] Parameter Problem (type 12, code 0-1)
>Il code 0 viene inviato se l'header del pacchetto IP ha un incongruenza con uno dei suoi campi. In questo caso viene aggiunto nella sezione "resto dell'header" un campo pointer di 8 bit che punta al byte del pacchetto che ha causato il problema.
>Il code 1 viene usato quando un opzione non è implementata.

>[!tip] Redirect (type 5, code 0-3)
>Questo messaggio è usato quando si vuole che la sorgente usi per raggiungere la destinazione un router diverso da quello prescelto.

### Funzionalità di diagnostica
A differenza della precedente funzionalità, la funzionalità di diagnostica funziona secondo il paradigma domanda/risposta.

>[!tip] Echo Request (type 8) & Echo Reply (type 0)
>![[Pasted image 20240619102648.png]]
>Questi messaggi sono usati per verificare la raggiungibilità e lo stato di un host o un router. Quando un nodo IP riceve un messaggio Echo Request risponde immediatamente con un messaggio di Echo Reply.
>
>Nel campo identifier viene scelto il mittente della richiesta, e verrà ripetuto nella risposta.
>In caso di più richieste consecutive si può inserire un different sequence number per identificare la richiesta.
>Infine nel campo optional data si può inserire una sequenza arbitraria, che sarà ripetuta per controllare eventuali errori

>[!example] Il comando traceroute
>Il comando traceroute normalmente usa i messaggi di echo request verso la destinazione, con un TTL impostato a 1, che verrà incrementato ad ogni risposta di errore fino al ricevimento dell'Echo Reply.
>
>Esempio del comando traceroute:
>
>```
➜ ~: traceroute www.ietf.org
traceroute to www.ietf.org (104.16.44.99), 30 hops max, 60 byte packets
 1  fritz.box (192.168.178.1)  0.524 ms  0.487 ms  0.463 ms
 2  81.174.0.21 (81.174.0.21)  12.177 ms  14.117 ms  14.098 ms
 3  * * *
 4  10.40.83.121 (10.40.83.121)  18.615 ms  19.288 ms  18.583 ms
 5  10.40.85.72 (10.40.85.72)  19.256 ms  19.228 ms  19.191 ms
 6  188.114.100.24 (188.114.100.24)  32.698 ms  27.112 ms  27.032 ms
 7  188.114.100.3 (188.114.100.3)  13.439 ms  20.467 ms 188.114.100.19 (188.114.100.19)  23.224 ms
 8  104.16.44.99 (104.16.44.99)  23.159 ms  23.141 ms  23.110 ms
>```

>[!tip] Address Mask Request (type 17) & Address Mask Reply (type 18)
>![[Pasted image 20240619103946.png]]
>Un host invia un messaggio AMR ai router della rete locale per conoscere la propria netmask. Un router risponde con un messaggio Address Mask Reply che contiene la subnet mask per la rete locale. Attualmente poco usato perché sostituito dal DHCP.

>[!tip] Router Solicitation (type 10) & Router Advertisement (type 9)
>![[Pasted image 20240619104152.png]]
>I router inviano messaggi di Router Advertisement per annunciare la loro presenza in rete e fornire informazioni utili, come l'indirizzo IP delle interfacce. L'invio e periodico. Possono essere sollecitati con messaggi di Router Solicitation. Attualmente poco usato perché sostituito dal DHCP

