# Misc

## ER

- quando c'è un istante di inizio e un istante di fine di un'entità, è quasi sempre necessario mettere un'Entità, e un'EntitàTerminata is-a Entità, altrimenti non è possibile registrare istanze di Entità, quando queste non sono ancora terminate
- gli attributi di relationship non possono essere chiave, e non possono essere contenuti in una chiave
- solo gli attributi con molteplicità (1,1) possono essere chiave e/o parte di chiave

## Domini

- esempio del simbolo di predicato
    - sia $\mathrm{DataOra} \{ \mathrm{data}, \mathrm{ora} \}$ un dominio
    - sia $\mathrm E$ un'entità
    - sia $\mathrm{attr}$ un attributo dell'entità $\mathrm E$, di tipo $\mathrm{DataOra}$
    - sia $e$ una variabile tale che $\mathrm{E}(e)$
    - sia v una variabile tale che $\mathrm{attr}(e, v)$
    - allora, la variabile $d$ tale che $\mathrm{data}(v, d)$ è la data di $v$, e la variabile $o$ tale che $\mathrm{ora}(v, o)$ è l'ora di $v$ 
- domini ricorrenti
    - Denaro
        - importo: intero >= 0
        - valuta: stringa di 3 caratteri secondo standard
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
        - giorno: intero in [1, 31]
        - mese: intero in [1, 12]
        - anno: intero >= 0
    - ora:
        - ora: intero in [0, 23]
        - minuti: intero in [0, 59]
        - secondi: intero in [0, 59]
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

## Use-case

- quando ho uno use-case che deve creare una is-a dell'input, non va restituita l'istanza in output
- si possono usare sovraentità, anche di generalizzazioni
- quando chiede il login di un utente, la signature è login(s: stringa): Utente, la precondizione è che deve esistere un utente avente nome 's', e viene restituito tale utente nella postcondizione
- k massimi
    - $S = \{(p, s) \mid \textrm{Persona}(p) \land \textrm{stipendio}(p, s)\}$
    - $\textrm{result} = \{(p, s) \mid (p, s) \in S \land |\{(p', s') \mid (p', s') \in S \land p' \neq p \land s \le s'\}| < k\}$
- k minimi
    - $S = \{(p, s) \mid \textrm{Persona}(p) \land \textrm{stipendio}(p, s)\}$
    - $\textrm{result} = \{(p, s) \mid (p, s) \in S \land |\{(p', s') \mid (p', s') \in S \land p' \neq p \land s \ge s'\}| < k\}$
- nelle precondizioni vanno aggiunte le condizioni dei vincoli esterni
- nelle precondizioni vanno aggiunte le condizioni delle chiavi dell'ER

## Trigger

- per controllare la disgiunzione di due cose che possono non essere terminate, va controllato solamente se esiste un'altra cosa che inizia prima della new, e o non è finita, o è finita dopo dell'inizio della new, e l'altro caso non va controllato poiché il trigger verrà eseguito nuovamente sull'inserimento dell'altro

## Use-case SQL

- extract("field" from "source")
- order by + limit per fare k max/min

