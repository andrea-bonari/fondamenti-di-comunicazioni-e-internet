>[!example] Esempio di tabella di routing
>
>| Netmask       | Destination   | Next Hop       | Flag          | Metric     | Use        | Interface       |
>| ------------- | ------------- | -------------- | ------------- | ---------- | ---------- | --------------- |
>| $$255.0.0.0$$ | $$124.0.0.0$$ | $$145.6.7.23$$ | $$\text{UG}$$ | $$4$$      | $$20$$     | $$\text{Eth}1$$ |
>| $$\cdots$$    | $$\cdots$$    | $$\cdots$$     | $$\cdots$$    | $$\cdots$$ | $$\cdots$$ | $$\cdots$$      |

### Inoltro diretto con le netmask
Per inoltrare un pacchetto occorre capire se appartiene alla sottorete di una delle interfacce. Per farlo si fa un AND bit a bit tra l'indirizzo dell'interfaccia e netmask, e tra l'indirizzo di destinazione e netmask. Se i due risultati coincidono allora la sottorete è la stessa e si procede all'inoltro diretto.

### Inoltro indiretto con le netmask
Se il precedente confronto è negativo si procede con un inoltro indiretto. Si effettua un confronto riga per riga AND bit a bit usando la netmask relativa a ciascuna riga. Se il confronto da esito positivo per più right della tabella viene selezionata la tabella con il maggior numero di 1, in quanto un prefisso più lungo equivale ad una rotta più specifica.

