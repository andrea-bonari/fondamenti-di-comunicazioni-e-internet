>[!note]
>DNS è un protocollo applicativo basato su UDP tra name server e host per risolvere nomi simbolici.
>
>Inoltre fornisce servizi di host aliasing, mail server aliasing e load distribution.

Il sistema di DNS distribuito e gerarchico, e ogni livello della gerarchia ha una profondità diversa dell'informazione.
![[Pasted image 20240313134814.png]]
Ci sono 13 root name servers in tutto il mondo.

Inoltre esistono diversi tipi di nameservers:
- Local Name Servers:
Risolve gli indirizzi simbolici in una rete locale, e eventualmente contatta i Root Name Servers

- Authoritative Name Servers
Nameserver responsabile di un hostname particolare

### Modalità di risoluzione hostname
Il metodo di risoluzione di un hostname è rappresentato in figura.
![[Pasted image 20240313135341.png]]

Tuttavia esistono due modalità di risoluzione di un hostname:

>[!tip] Modalità iterativa
>![[Pasted image 20240313135620.png]]

>[!tip] Modalità ricorsiva
>![[Pasted image 20240313135705.png]]

Un server, dopo aver reperito un informazione da un authoritative server può memorizzarla temporaneamente per un tempo chiamato `TTL` stabilito dall'authoritative server. I TLD server sono generalmente memorizzati nei LNS

### Tipologia di informazioni

Ogni risorsa è organizzata in una $4$-upla di informazioni: $$(\text{Name},\text{Value},\text{Type},\text{TTL})$$
In base al tipo le possiamo descrivere come 

| Tipo    | Descrizione                                                                           |
| ------- | ------------------------------------------------------------------------------------- |
| `A`     | Associa l'host `Name` all'indirizzo IP `Value`                                        |
| `NS`    | Associa un dominio `Name` a un server `Value` che può ottenere ulteriori informazioni |
| `CNAME` | `Name` è un alias per l'host il cui nome canonico è in `Value`                        |
| `MX`    | `Name` è il dominio/alias di una mail e `Value` è il nome del mail server             |
### Formato dei messaggi DNS
![[Pasted image 20240313140853.png]]

### Content Distribution Networks (CDNs)
>[!note]
>Per distribuire in maniera efficiente molteplici contenuti contemporaneamente a molteplici utenti geograficamente distanti si può adottare una rete di CDN.
>
>Una rete di server geograficamente distribuita che ospita copie dei contenuti richiesti è definita come una CDN, è può essere di proprietaria o di terze parti.

Per scegliere quale CDN utilizzare si possono usare i seguenti criteri:
- Più vicino
- Percorso più corto
- Lasciare la scelta all'utente

