>[!note]
>HTTP/2 è una versione di HTTP che mira a ridurre la latenza dei trasferimenti con più oggetti e a risolvere alcuni problemi di HTTP/1.1
>
>A differenza delle versioni precedenti HTTP/2:
>- È in formato binario e non testuale
>- Utilizza una singola connessione TCP per stream multipli
>- Comprime gli header
>- Aggiunge un servizio di server push e un servizio di controllo di flusso a livello applicativo
>- Si appoggia di default su TLS

### Header HTTP2
![[Pasted image 20240305174636.png]]

Il campo type può essere:

| Campo                                     | Descrizione                           |
| ----------------------------------------- | ------------------------------------- |
| `DATA`                                    | Trasporta dati di uno stream          |
| `HEADERS`                                 | Apre uno stream                       |
| `PRIORITY`                                | Specifica priorità di uno stream      |
| `RST_STREAM`                              | Termina uno stream                    |
| `SETTINGS`                                | Trasporta parametri di configurazione |
| `PUSH_PROMISE`                            | Gestisce il servizio di push          |
| `PING,GOAWAY,WINDOWS_UPDATE,CONTINUATION` | Chiude una connessione                |
##### Compressione degli header

Siccome l'header di una richiesta può avere dimensioni non trascurabili utilizziamo le seguenti tecniche per comprimerlo:
- **Codifica di Huffman**: Assegno stringhe binarie ai simboli più comuni
- **Indexing**: Assegno un indice alle header line più comuni e invio solo l'indice
- **Codifica differenziale**: L'header riporta solo le differenze rispetto all'header precedente

### Multiplazione e server push
>[!note]
>In HTTP2 lo scambio di frame tra client e server è organizzato in stream. Il server può inviare al client delle informazioni utili senza che ne faccia esplicita richiesta (server push). Questo è usato generalmente per i file HTML che richiedono oggetti aggiuntivi (CSS, JS, immagini, ecc).

### HTTPS
>[!note]
>Per encrittare la connessione HTTP possiamo utilizzare i protocolli SSL (Secure Socket Layer) e TLS (Transport Layer Security), che aggiungono confidenzialità, integrità e autenticazione alla connessioni TCP.

La procedura di connessione SSL/TLS si divide nei seguenti passaggi:
1. **Handshake**: Il server e il client si accordano sul tipo di cifratura da applicare ai dati. Generalmente il client e il server si scambiano i certificati di identità, il server invia una chiave pubblica assieme a delle info aggiuntive, genera delle chiavi simmetriche per le cifrature del trasferimento dati e le chiavi simmetriche sono scambiate su una connessione cifrata con altrettante chiavi asimmetriche.
2. **Trasferimento dei dati**: I dati sono suddivisi in PDU, che saranno cifrate con l'algoritmo scelto in precedenza.
3. **Chiusura della connessione**: Si usa un messaggio speciale per chiudere la connessione in modo sicuro.
