>[!note]
>Un indirizzo IPv4 è un numero binario di 32 bit, spesso rappresentato con un numero decimale ogni 8 byte: $$\underbrace{10000000}_{128}.\underbrace{00001011}_{11}.\underbrace{00000011}_{3}.\underbrace{00011111}_{31}$$
>È associato in modo univoco ad un'interfaccia di rete di host o di un router (non direttamente all'host o router). Un indirizzo IP deve avere valenza e univocità universali, perché il routing in IP è basato sull'indirizzo dell'host di destinazione. Ogni gestore di rete ha a disposizione un blocco di indirizzi che distribuisce alle interfacce dei singoli apparati.

### IP network e NetID
>[!note]
>Un blocco di IP è assegnato alle interfacce di una rete IP, ogni indirizzo deve avere identici i primi $n$ bit. Questi $n$ bit si chiamano identificativo di rete (NetID).

Ogni indirizzo IP è quindi diviso in due campi:
![[Pasted image 20240618102022.png]]

### Classless Inter-Domain Routing (CIDR)
>[!note]
>CIDR è stato sviluppato negli anni '90 ed è stato introdotto in via definitiva a settembre 1993.
>Secondo questo standard $x.y.z.w/w$ significa che viene allocato ad una rete un insieme di indirizzi contigui con primo indirizzo $x.y.z.w$ e con $2^{32-n}$ indirizzi in totale. Questo sistema ha razionalizzato l'assegnazione di indirizzi poco flessibile del sistema classful.

>[!tip] Netmask
>La netmask è un numero binario di 32 bit associato ad una rete IP. Inizia con $n$ bit impostati a $1$ con $n$ pari alla lunghezza del NetID. I restanti bit sono impostati a $0$.
>Indica quali bit di un indirizzo IP sono assegnati al NetID.

>[!example] NetID: notazioni alternative e equivalenti
>Rappresentiamo il NetID $130.86.0.0$ ($10000010\space01010110$):
>- $130.86.0.0$ + Netmask $255.255.0.0$
>- $130.86.0.0/16$
>- $130.86.$ * $.$ *
>- Intervallo: $[130.86.0.0,130.86.255.255]$

### Rete IP e rete fisica
Una rete IP è un insieme di interfacce fisicamente interconnesse, tipicamente con switch e hub. È necessario che vi sia almeno un router con un'interfaccia collegata alla rete IP per comunicare con altre reti IP.

### Indirizzi privati
>[!note]
>Esistono dei blocchi di indirizzi usabili da chiunque in ambito privato, ciò significa che possono essere riutilizzati più volte:
>- $[10.0.0.0,10.255.255.255]$
>- $[172.16.0.0,172.31.255.255]$
>- $[192.168.0.0,192.168.255.255]$

Un router non deve mai inoltrare un pacchetto con destinazione un indirizzo IP privato verso una propria interfaccia di uscita che abbia un indirizzo IP pubblico.

### Indirizzi speciali

>[!tip] Indirizzo di rete
>L'indirizzo con il campo HostID posto a 0 serve ad indicare la rete il cui indirizzo è contenuto nel campo NetID.

>[!tip] Indirizzo broadcast diretto
>L'indirizzo con il campo HostID di soli 1 assume il significato di indirizzo di broadcast della rete indicata nel campo NetID. I router di transito lo trattano come un normale pacchetto, il router della rete di destinazione esegue il broadcast solo se è abilitato a farlo.

>[!tip] Indirizzo di broadcast limitato
>Un indirizzo di soli 1 ($255.255.255.255$) assume il significato di indirizzo di broadcast nella stessa rete di chi invia il pacchetto. Questo pacchetto non può oltrepassare il router.

>[!tip] Stessa rete
>Quando il campo NetID è posto a zero, l'indirizzo indica l'host il cui indirizzo è contenuto nel campo host sulla stessa rete del mittente.

>[!tip] Mittente
>Un indirizzo di soli 0 ($0.0.0.0$) assume il significato del mittente stesso del pacchetto. È usato come campo sorgente quando l'host non conosce il proprio indirizzo.

>[!tip] Indirizzo di loopback
>L'indirizzo con il primo ottetto pari a 127 e gli altri campi qualsivoglia indica il loopback sullo stesso host.

### Assegnamento degli indirizzi
L'ente IANA (Internet Assigned Numbers Authority) coordina e pianifica l'assegnazione degli indirizzi IP su base mondiale (sia per IPv4 che IPv6). È anche amministratore dei DNS root.

La IANA assegna blocchi di indirizzi a 5 RIR (Regional Internet Registries). Ciascun RIR assegna blocchi ai LIR (Local Internet Registries), che sono ISP o istituzioni che a loro volta possono assegnare prefissi di rete ai propri clienti.

>[!error] Esaurimento degli indirizzi IPv4
>La IANA ha assegnato tutti gli indirizzi disponibili ai RIR nel 2011, e ogni RIR ha terminato gli indirizzi disponibili. Si è quindi rivelato che la scelta di usare soli 32 bit per gli indirizzi è stata una scelta sbagliata.
>
>Di conseguenza l'unica possibilità è di assegnare nuovi indirizzi IPv6 (128 bit, quindi $2^{128}$ indirizzi)

### Indirizzamento classful
>[!note]
>Gli indirizzi IPv4 erano originariamente divisi in 5 classi:
>
>| Classe | Descrizione         | NetID  | HostID | Indirizzo inizio | Indirizzo fine    |
>| ------ | ------------------- | ------ | ------ | ---------------- | ----------------- |
>| A      | Poche reti grandi   | 7 bit  | 24 bit | $$0.0.0.0$$        | $$127.255.255.255$$ |
>| B      | Reti medio-grandi   | 14 bit | 16 bit | $$128.0.0.0$$      | $$191.255.255.255$$ |
>| C      | Tante reti piccole  | 24 bit | 8 bit  | $$192.0.0.0$$      | $$223.255.255.255$$ |
>| D      | Indirizzi multicast | /      | /      | $$224.0.0.0$$      | $$239.255.255.255$$ |
>| E      | Indirizzi multicast | /      | /      | $$240.0.0.0$$      | $$255.255.255.255$$ |
>
>In questo sistema i router deducono la lunghezza del prefisso dei primi bit del primo byte, infatti non occorre la netmask e non occorre che i protocolli di routing supportino la netmask nei messaggi.

Per quanto possa sembrare comodo il problema è la tipologia di classi:
- Le reti di classe A e B sono troppo grandi
- Le reti di classe B sono troppo poche
- Le reti di classe C sono troppo piccole

Eliminando la distinzione tra classi CIDR risolve il problema.

