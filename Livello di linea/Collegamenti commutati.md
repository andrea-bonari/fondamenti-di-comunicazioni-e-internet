>[!note]
>Nel caso di Ethernet, è possibile usare delle LAN commutate (o switched), che implementano la commutazione di pacchetto. Per implementarla in Ethernet occorre sostituire l'HUB con un dispositivo detto SWITCH (o bridge).

### Switch
>[!note]
>Lo switch ha una funzione di filtering e di relay: I pacchetti possono venire scartati o inoltrati in base alla rete di provenienza e destinazione. Per stabilire se filtrare/instradare una trama si consulta una tabella locale chiamata Forwarding DataBase (FDB).

A ciascuna porta di uno switch è collegata una rete che può contenere una rete broadcast tradizionale o anche solo una stazione.
Lo switch assicura che le trame provenienti da ciascun dominio siano inoltrate al dominio di destinazione. In modalità broadcast le trame sono inoltrate su tutti i domini eccetto quello di origine.

>[!tip] Segmentazione in domini
>Lo switch segmenta i domini di accesso multiplo mantenendo la rete un unico dominio broadcast, questa segmentazione consente di aumentare l'efficienza.

### Transparent Bridging
Lo switch non dispone di un indirizzo MAC, ed è quindi completamente trasparente alle stazioni che implementano le funzionalità del livello di linea. 

Il FDB viene compilato autonomamente all'arrivo di un pacchetto secondo il metodo Transparent Bridging (valido solo per reti ad albero): all'arrivo di ogni pacchetto l'indirizzo sorgente è inserito nella tabella. Se dobbiamo inoltrare un indirizzo in tabella inoltriamo alla porta corrispondente, altrimenti eseguiamo un inoltro broadcast. La validità delle righe del FDB è limitata a un tempo limite, solitamente 300s.

### Broadcast Storm
In caso la rete non sia ad albero gli switch seguono il seguente metodo:
![[Pasted image 20240620141606.png]]

### Spanning Tree
Nella pratica la topologia è di solito magliata per garantire tolleranza ai guasti, e per evitare il problema del broadcast storm i bridge rendono inattive alcune porte in modo da ridurre la rete ad una rete ad albero nel funzionamento normale. Un protocollo specifico (Spanning Tree Protocol) calcola lo spanning Tree e lo modifica in caso di guasti.

