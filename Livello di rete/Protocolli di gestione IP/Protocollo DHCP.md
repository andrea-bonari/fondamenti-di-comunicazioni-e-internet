Le procedure statiche di assegnamento degli indirizzi sono poco flessibili. In molti casi ci sono più host degli indirizzi disponibili ma alcuni host sono spesso in attivi o scambiano informazioni raramente. Per ottenere degli indirizzi dinamicamente quindi supponiamo di avere un server in grado di fornire l'indirizzo IP ad un host su richiesta, potremo fare associazioni di tipo diverso:
- associazioni statiche: il server ha una tabella fissa di corrispondenza tra indirizzi fisici e IP.
- associazioni automatiche: la procedura di corrispondenza nella tabella è automatizzata dal server.
- associazioni dinamiche: l'insieme di IP disponibili è più piccolo degli host.

>[!note] Dynamic Host Configuration Protocol (DHCP)
>DHCP è un servizio per associare dinamicamente gli indirizzi IP. Ogni associazione è rilasciata dopo un timeout. È anche possibile che all'arrivo di una richiesta non ci siano indirizzi disponibili.

### Scambio di messaggi
1. Un client che deve configurare il proprio stack IP invia un messaggio broadcast di `DHCPDISCOVER` con il proprio indirizzo fisico.
2. Il server risponde con un messaggio di `DHCPOFFER` contenente il proprio identificativo e un indirizzo IP proposto.
3. Il client può accettare l'offerta inviando una `DHCPREQUEST` in broadcast con l'identificativo del server.
4. Il server crea l'associazione e manda un messaggio `DHCPACK` contenente tutte le informazioni necessarie.
5. Il client può rilasciare l'indirizzo con l'invio di un messaggio `DHCPRELEASE`.

I parametri inviati dal server sono l'indirizzo IP, la NetMask, il Gateway e il server DNS.

DHCP usa le porte 68 (nel client) e 67 (nel server)
