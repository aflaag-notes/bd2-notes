# Misc

## ER

- quando c'è un istante di inizio e un istante di fine di un'entità, è sempre necessario mettere un'Entità, e un'EntitàTerminata is-a Entità, altrimenti non è possibile registrare istanze di Entità, quando queste non sono ancora terminate
- gli attributi di relationship non possono essere chiave, e non possono essere contenuti in una chiave TODO QUESTA COSA MESA CHE È UNA STRONZATA

## Domini

- esempio del simbolo di predicato
    - sia $\mathrm{DataOra} \{ \mathrm{data}, \mathrm{ora} \}$ un dominio
    - sia $\mathrm E$ un'entità
    - sia $\mathrm{attr}$ un attributo dell'entità $\mathrm E$, di tipo $\mathrm{DataOra}$
    - sia $e$ una variabile tale che $\mathrm{E}(e)$
    - sia v una variabile tale che $\mathrm{attr}(e, v)$
    - allora, la variabile $d$ tale che $\mathrm{data}(v, d)$ è la data di $v$, e la variabile $o$ tale che $\mathrm{ora}(v, o)$ è l'ora di $v$ 
- domini ricorrenti
    - TODO DA FINIRE
    - IntervalloOre
    - Periodo
    - Denaro
    - Indirizzo
        - via: stringa
        - civico : intero > 0 (0,1)
        - CAP: stringa di 5 cifre numeriche
        - città: stringa
        - nazione: stringa
    - Telefono
        - codicePaese: stringa di massimo 5 cifre numeriche secondo standard
        - numero: stringa di massimo 15 cifre numeriche
    - CodiceFiscale
        - composto da stringhe alfanumeriche di 16 caratteri che rispettano i vincoli dei codici fiscali italiani
    - data:
        - giorno: ?
        - mese: ?
        - anno: ?
    - dataora
        - giorno: data
        - ora: ora

## Vincoli

- vincolo di disgiunzione tra periodi, da imparare a memoria (preso da Officine)
    - le riparazioni relative allo stesso veicolo hanno periodi disgiunti; il vincolo implica che ogni veicolo può essere associato al più ad una riparazione in corso, e che questa sarà l'ultima in ordine cronologico
    - $$\forall v, r_1, r_2, a_1, a_2 \quad \\ \mathrm{Veicolo}(v) \land \mathrm{Riparazione}(r_1) \land \mathrm{Riparazione}(r_2) \land \mathrm{veicoloRip}(v, r_1) \land \\ \mathrm{veicoloRip}(v, r_2) \land r_1 \neq r_2 \land \mathrm{accettazione}(r_1, a_1) \land \mathrm{accettazione}(r_2, a_2) \rightarrow \\ \nexists t \quad \mathrm{dataora}(t) \land (t \ge a_1 \land (\forall \hat r_1 \quad \mathrm{riconsegna}(r_1, \hat r_1) \rightarrow t \le \hat r_1)) \land \\ (t \ge a_2 \land (\forall \hat r_2 \quad \mathrm{riconsegna}(r_2, \hat r_2) \rightarrow t \le \hat r_2))$$
- si possono usare sovraentità, anche di generalizzazioni

## UML

- "sistema" NON è un attore

## Use-Case

- quando ho uno use-case che deve creare una is-a dell'input, non va restituita l'istanza in output
- si possono usare sovraentità, anche di generalizzazioni
- quando chiede il login di un utente, la signature è login(s: stringa): Utente, la precondizione è che deve esistere un utente avente nome 's', e viene restituito tale utente nella postcondizione
- TODO K MINIMI E K MASSIMI
- nelle precondizioni vanno aggiunte le condizioni dei vincoli esterni
- nelle precondizioni vanno aggiunte le condizioni delle chiavi dell'ER

****

# Travel to the Moon

## ER

- "modello dei voli", necessario poiché altrimenti si potrebbero creare delle catene con cicli strani
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

- "modello dei voli" (vedi Travel to the Moon)
- biglietto è una funzionalità, non un'entità
- entità tipologia del velivolo

## UML

- il cliente non è attore del sistema, c'è un Sistema Prenotazioni

## Use-Case

- result può essere inserito dentro una formula FOL
- divisione per 2 va arrotondata
- le costanti va esplicitato che sono simboli di costante
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

****

# QuickHospital

## ER

- una prestazione contiene ricoveri e prestazioni esterne
- le persone sono solamente medici o pazienti (vincolo esterno), ma i medici possono essere pazienti (relazione is-a, e non generalizzazione, perché disgiunge)
- il medico non è paziente di sé stesso
- le stanze hanno un numero di stanza, anche se non c'è scritto
- specializzazione primaria is-a specializzazione, come relationship

## Vincoli

- si può usare sovra-entità di una generalizzazione nei vincoli
- intervalli
    - data termine ricovero maggiore della data inizio ricovero
    - lo stesso posto letto non può avere due ricoveri contemporaneamente (vincolo scritto nella sezione iniziale, con Officine)
    - un paziente non può prenotare una prestazione esterna mentre è ricoverato (e viceversa)
    - un paziente non può essere ricoverato mentre è già ricoverato

## Use-Case

- ordinare un insieme di elementi
    - sorted(S: Entità (0,N), criterio): (e: Entità, n: intero > 0) (0,N)
        - S: insieme da ordinare
        - criterio: una funzione della forma criterio(e1: Entità, e2: Entità): bool, che definisce il criterio di ordinamento
            - criterio restituisce true se e solo se e1 <= e2
- posso chiamare più use-case in contemporanea, quindi possono non badare ai vincoli dell'ER

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
- alle sottrazioni tra date/orari va specificato che non sono interi
- attenzione alle attività che possono sforare in date successive

****

# CoLab

- fa sboccare sto progetto

****

# EDC

## ER

- relazione a 3 tra carta di credito, ricarica e cliente, in cui la molteplicità deve per forza essere CartaDiCredito - (0,N), poiché altrimenti la carta può fare una sola ricarica, ma serve un vincolo esterno per cui la ricarica può essere fatta da una sola carta

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
    - l'ascolto non può essere una relationship
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

- per fare le operazioni sulle playlist, serve anche l'utente in input

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
- LE RELAZIONI POSSONO AVERE ATTRIBUTI CHIAVE/PARTE DI CHIAVE??

****

# SafeOnTheBeach

## Ristrutturazione ER

- rimozione entità Utente

## Use-case SQL

- CAPIRE GROUP BY

