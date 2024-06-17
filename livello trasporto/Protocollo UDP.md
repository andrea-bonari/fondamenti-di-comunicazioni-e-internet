>[!note]
>Il protocollo UDP è il modo più semplice di usare IP, in quanto non aggiunge nulla se non l'indirizzamento delle applicazioni e un blando controllo dell'errore sull'header (senza correzione), di conseguenza non garantisce la consegna e non esercita nessun controllo di flusso o di errore. È un protocollo datagram.

### Differenze tra UDP e TCP
- Minore latenza in quanto non è necessario stabilire una connessione
- Maggiore semplicità in quanto non occorre tenere traccia dello stato della connessione e ci sono poche regole da implementare
- Minore overhead in quando l'header UDP è minore dell'header TCP.

### Checksum
>[!note] Controllo di integrità
>Viene inserita dell'informazione ridondante nell'header per il controllo dell'errore. Il campo checksum viene calcolato dal trasmettitore e inserito nell'header, il ricevitore effettuerà lo stesso calcolo. Se il risultato è corretto accetta il segmento, altrimenti lo scarta.
>![[Pasted image 20240322072341.png]]

**Calcolo del checksum lato trasmettitore**
1. L'insieme di bit è diviso in blocchi da 16 bit
2. Tutti i blocchi sono sommati in aritmetica complemento ad uno.
3. Il risultato è complementato e inserito nel campo checksum

**Calcolo del checksum lato ricevitore**
1. L'insieme di bit è diviso in blocchi da 16 bit
2. Tutti i blocchi vengono sommati in aritmetica complemento ad uno
3. Il risultato è complementato
4. Se i bit sono tutti `0` il pacchetto è accettato, altrimenti è scartato


