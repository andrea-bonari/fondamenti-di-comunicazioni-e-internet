>[!note]
>Esistono delle tabelle denominate ARPCache che creano una corrispondenza tra indirizzi IP e indirizzi fisici.

### Address Resolution Protocol (ARP)
Quando nella ARPCache non è presente l'indirizzo MAC cercato viene generato un messaggio di ARP-request, che è inviato in broadcast e contiene l'indirizzo IP di cui si chiede l'indirizzo MAC. L'host che riconosce l'indirizzo IP come proprio invia una ARP-reply direttamente a chi aveva inviato la richiesta con l'indicazione del proprio indirizzo MAC.

### Reverse ARP (RARP)
Il protocollo RARP effettua l'operazione inversa: conoscendo un indirizzo MAC e non l'indirizzo IP va a scoprire tale IP. È molto utile per le macchine diskless che effettuano bootstrap in rete, tuttavia non è più usato perché sostituito da DHCP.