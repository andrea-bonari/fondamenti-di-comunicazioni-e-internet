>[!note]
>La commutazione di circuito è un protocollo che alloca un canale predefinito per l'informazione e la trasferisce su quello stesso canale.

Originariamente l'operazione di allocazione era svolta manualmente da un centralinista, ma ad oggi è automatizzato.

Come vantaggi ha una connessione stabile e di qualità, sia dal punto di vista della durata che della velocità, tuttavia per attuare la commutazione di circuito si usa un canale dedicato alla connessione che non può essere utilizzato da altre applicazioni, quindi nonostante la premessa di integrità e velocità non è pienamente ottimizzato.

Il processo di commutazione di circuito comprende:
1. Instaurazione del circuito
2. Trasferimento dei pacchetti
3. Comunicazione di end e relativa flag che indica che il canale è nuovamente libero.

In termini di performance i nodi non hanno bisogno di un buffer per memorizzare temporaneamente i dati in quanto vengono presi e trasmessi direttamente, infatti la capacità dei canali di ingresso è pari a quella dei canali di uscita.