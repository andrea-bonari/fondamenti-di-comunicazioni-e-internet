### cURL
>[!note]
>cURL è un tool da linea di comando la manipolazione ed il trasferimento di dati basato su url. Esempio:
>```curl www.polimi.it```

### Cookies
HTTP è stateless, tuttavia in certi casi è necessario mantenere uno stato. Per permettere questo sono necessari dei cookie.

Per creare un cookie il server risponde con un header `set-cookie: <cookie>` nella risposta, ogni successiva richiesta del client avrà un header `cookie: <cookie precedente>` (memorizzato nell'host) e il server risponderà con una risorsa diversa in base al cookie (tramite un db di supporto).

### Proxy
>[!note]
>Un proxy è un intermediario tra un client e un server HTTP. Tutte le richieste del client sono quindi soddisfatte senza coinvolgere il server. Generalmente un proxy dispone di cache utilizzate per migliorare la performance.

Ci sono due tipi di proxy:
- Default
- Proattivo: in base e modelli e statistiche passate comincia a caricare pagine con alta probabilità di essere richieste

I proxy moderni possono anche avere funzioni di indirizzamento o di sicurezza.
![[Pasted image 20240305070841.png]]

