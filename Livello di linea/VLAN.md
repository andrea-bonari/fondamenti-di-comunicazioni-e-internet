>[!note]
>La modalità Ethernet Full Duplex consente la trasmissione su doppino o fibra senza CSMA-CD. Ciò consente la creazione di LAN completamente commutate che costituiscono la soluzione usata universalmente oggigiorno. Non ci sono collisioni perché ad ogni porta di uno switch è collegato al massimo un trasmettitore e un ricevitore.

### Virtual LANs (VLANs)
>[!note]
>Le VLAN consento di creare LAN logicamente separate su un'unica LAN fisica, la comunicazione tra le diverse VLAN può avvenire a livello 3 attraverso un router. La separazione in VLAN è spesso dovuta a motivi di sicurezza e separazione di traffico.

Le VLAN sono separate staticamente in base alla porta dello switch, ed è indipendente dalla configurazione fisica delle VLAN. Una VLAN può essere assegnata a più di una porta.

Le trunk port (link di collegamento tra switch) trasportano il traffico di tutte le VLAN, tuttavia è necessario un metodo per distinguere le trame di VLAN diverse sui trunk.
![[Pasted image 20240620142508.png]]

### VLAN tagging
>[!note]
>Il protocollo 802.1q (LAN tagging) serve a etichettare le trame di differenti VLAN per differenziarle. Dal punto di vista del livello 3, le VLAN equivalgono a LAN fisiche separate, tuttavia le interfacce dei router connessi agli switch che gestiscono le VLAN devono supportare il protocollo LAN tagging.

