# Travel to the Moon

## ER

- "modello delle tappe", poiché dalla tappa di inizio si parte solamente, e dalla tapa di arrivo si arriva solamente
    - Itinerario - (1,1) - partItin - (0,N) - Destinazione
    - Itinerario - (1,1) - arrivoItin - (0,N) - Destinazione
    - Itinerario - (0,N) - tappaItin - (1,1) - Tappa
    - Tappa - (1,1) - tappaDest - (0,N) - Destinazione
    - Destinazione - (0,N) - postoDest - (1,N) - Posto

## Use-case

- parametri (0,1), per controllare se esistono bisogna creare una formula che è vera se il valore in input non è definito, oppure vale il valore restituito dal predicato stesso
    - "siano definite le seguenti formule (aperte in c)"

****

# TuTubi

## Vincoli

- valori distinti e progressivi per gli interi dei video delle playlist
    - per fare la progressione, per ogni valore $p$, ci deve essere un $p - 1$

## Use-Case

- per le medie è importante salvare tuple negli insiemi, e non solo i valori, perché altrimenti viene falsata la media

****

# RainAir

## ER

- "modello dei voli" (vedi progettino Voli, che contiene anche le tappe)
- biglietto è una funzionalità, non un'entità
- entità tipologia del velivolo

## UML

- il cliente non è attore del sistema, c'è un Gestore del sistema delle prenotazioni

## Use-Case

- result può essere inserito dentro una formula FOL
- divisione per 2 va arrotondata
- la percentuale si può fare facendo l'elevamento a potenza
    - _esempio_

se a $x = 100$ si vuole applicare il $2\%$ di sconto per 3 volte, ovvero

$$\left. \begin{array}{l}
    x := 100 \\
    x = x - 0.02 \cdot x \\
    x = x - 0.02 \cdot x \\
    x = x - 0.02 \cdot x \end{array} \right.$$

il calcolo è equivalente a

$$\left. \begin{array}{l}
    x := 100 \\
    x = x - (1 - 0.02 \cdot x)^3 \end{array} \right.$$

## Use-case SQL

- poiché la prenotazione può non avere hotel, il fattore moltiplicativo può essere inteso con "h.categoria is null", facendo "Prenotazione p left outer join Hotel h on p.hotel = h.id"

****

# QuickHospital

## ER

- una prestazione contiene ricoveri e prestazioni esterne
- le persone sono solamente medici o pazienti (vincolo esterno), ma i medici possono essere pazienti (relazione is-a, e non generalizzazione, perché disgiunge)
- il medico non è paziente di sé stesso
- le stanze hanno un numero di stanza, anche se non c'è scritto
- specializzazione primaria is-a specializzazione, come relationship

## Use-Case

- ordinare un insieme di elementi
    - sorted(S: Entità (0,N), criterio): (e: Entità, n: intero > 0) (0,N)
        - S: insieme da ordinare
        - criterio: una funzione della forma criterio(e1: Entità, e2: Entità): boolean, che definisce il criterio di ordinamento
            - criterio restituisce true se e solo se e1 <= e2

****

# xFit

## ER

- unione tra istruttore e dipendenti in un'unica entità, per avere un'unica relationship di contratto, e permettere che un istruttore possa essere un cliente
    - un dipendente può attraversare i varchi
- le persone hanno sempre un codice fiscale

## Vincoli esterni

- vincoli esterni di completezza sulle is-a
- vincolo periodo di vendita legale sul dominio, non sugli abbonamenti (il dominio è gestito come se fosse un'entità)

## Use-Case

- le statistiche vanno unite con un solo use-case
- alle somme/sottrazioni tra date/orari va specificata l'unità di tempo
- attenzione alle attività che possono sforare in date successive

****

# CoLab

## Use-case

- vanno considerati gli accessi/utilizzi che sono a cavallo tra 2 giorni diversi (e gestire correttamente le intersezioni delle fasce)

## Use-case SQL

- use-case in sql della prima funzionalita

```
useCase1(F: Insieme(<da: time, a: time>)): Insieme(<da: time, a: time, m: real>)
    if |F| == 0:
        gerare l'errore ...
    else:
        tabella temporanea

        TmpFasce(_da_: time, _a_: time, _giorno_: date, numUtenti: integer)
        
        foreach <da, a> in F:
            foreach giorno negli ultimi 30 giorni:
                Q <- (
                    select count(u.email) as numUtenti
                    from Utente u, Utilizzo ut
                    where ut.utente = u.id
                    and extract(hour from u.istIn) >= :da
                    and u.istFin is not null
                    and extract(hour from u.istFin) <= :a
                    and extract(day from u.istIn) = :giorno
                    and extract(day from u.istFin) = :giorno
                )

                insert into TmpFasce(da, a, giorno, numUtenti) values (:da, :a, :giorno, Q.numUtenti)

        if exists (
            select * from TmpFasce f
            where f.da < f.a
        ) or exists (
            select * from TmpFasce f1, TmpFasce f2
            where (f1.inizio, f1.fine)
            overlaps (f2.inizio, f2.fine)
        ):
            generare l'errore ...
        else:
            Q <- (
                select f.da, f.a, avg(f.numUtenti)
                from TmpFasce f
                group by f.da, f.a
            )

            if Q == NULL:
                generare l'errore ...
            else:
                return Q
```

****

# EDC

## ER

- relazione a 3 tra carta di credito, ricarica e cliente, in cui la molteplicità deve per forza essere CartaDiCredito - (0,N), poiché altrimenti la carta può fare una sola ricarica

## Vincoli esterni

- controllare che le prenotazioni di un cliente siano pagate con la _sua_ carta, o col _suo_ conto (e altri vincoli simili)

## Use-case

- use-case per trovare il prezzo della prenotazione, prendendo i parametri della prenotazione, perché la prenotazione ancora non esiste

****

# iHealth

## Vincoli esterni

- un medico non è paziente di sé stesso

****

# myTunes

## ER

- modello della CPB
    - la CPB non è un'entità, ma si costruisce con gli slot
- se ci sono 2 entità E1, E2 collegate da una relazione E1 - (1,1) - rel - (1,1) - E2, al 99% sono la stessa entità

****

# ecoParks.it

## ER

- i vincoli di integrità devono tutti comprendere cose con molteplicità (1,1)

## Vincoli esterni

- animali(r, false), nel caso in cui 'animali' sia l'attributo booleano di un istanza 'r' di un'entità

## UML

- l'utente può effettuare prenotazioni

****

# Mall

## ER

- se due entità E1, E2 sono in relazione tramite E1 - (0,1) - rel - (1,1) - E2, al 99% E2 is-a E1

## Use-Case

- per fare le operazioni sulle wishlist, serve anche l'utente in input

****

# iCare

- è facile

****

# Workouts

## Use-case

- calcolo della lunghezza del percorso
    - per prendere due tappe consecutive, basta controllare che non esistano tappe in mezzo alle due prese

****

# Alimentary

- le regole devono essere entità, altrimenti non possono essere restituite

****

# TravelPlan

## ER

- invece di fare una relazione ricorsiva, creare un'entità AttivitaComposta
    - Viaggio - (0,N) - ... - (1,1) - AttivitaComposta - (1,N) - ... - (0,N) - AttivitaSingola
    - Utente - (0,N) - ... - (0,N) - AttivitaComposta - (1,N)

****

# SafeOnTheBeach

## Ristrutturazione ER

- rimozione entità Utente

****

# My Precious

## ER

- i biglietti full access e esposizioni permanenti only non dovrebbero essere collegati alle esposizioni, perché le posso visitare tutte quante

## Use-case

- i restauri possono essere iniziati nel periodo dato, o terminati nel periodo dato

## Use-case SQL

- utilizzare le tabelle temporanee per fare gli argmin/argmax

****

# EasyToll

## ER

- veicolo è un'entità da fare, per le disgiunzioni
- cliente è un'entità da fare
- gli attraversamenti vanno fatti con le is-a, sia dei caselli che dei tutor
- relazione tra ClasseVeicolo e Autostrada è una sola

****

# House.com

## ER

- la tipologia è generalizzata completamente in 5, con Altro che ha un attributo chiave "nome"
- la proprietà cancellata non è un'entità

