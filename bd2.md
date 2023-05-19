# Misc

## ER

- quando c'è un istante di inizio e un istante di fine di un'entità, è sempre necessario mettere un'Entità, e un'EntitàTerminata is-a Entità, altrimenti non è possibile registrare istanze di Entità, quando queste non sono ancora terminate
- gli attributi di relationship non possono essere chiave, e non possono essere contenuti in una chiave

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

## Vincoli

- vincolo di disgiunzione tra periodi, da imparare a memoria (preso da Officine)
    - le riparazioni relative allo stesso veicolo hanno periodi disgiunti; il vincolo implica che ogni veicolo può essere associato al più ad una riparazione in corso, e che questa sarà l'ultima in ordine cronologico
    - $$\forall v, r_1, r_2, a_1, a_2 \quad \\ \mathrm{Veicolo}(v) \land \mathrm{Riparazione}(r_1) \land \mathrm{Riparazione}(r_2) \land \mathrm{veicoloRip}(v, r_1) \land \\ \mathrm{veicoloRip}(v, r_2) \land r_1 \neq r_2 \land \mathrm{accettazione}(r_1, a_1) \land \mathrm{accettazione}(r_2, a_2) \rightarrow \\ \nexists t \quad \mathrm{dataora}(t) \land (t \ge a_1 \land (\forall \hat r_1 \quad \mathrm{riconsegna}(r_1, \hat r_1) \rightarrow t \le \hat r_1)) \land \\ (t \ge a_2 \land (\forall \hat r_2 \quad \mathrm{riconsegna}(r_2, \hat r_2) \rightarrow t \le \hat r_2))$$
- si possono usare sovraentità, anche di generalizzazioni

## Use-Case

- quando ho uno use-case che deve creare una is-a dell'input, non va restituita l'istanza in output
- si possono usare sovraentità, anche di generalizzazioni
- quando chiede il login di un utente, la signature è login(s: stringa): Utente, la precondizione è che deve esistere un utente avente nome 's', e viene restituito tale utente nella postcondizione

## Ristrutturazione ER

- rimozione attributi multivalore (con molteplicità (0,N) o (1,N))
    - attributo multivalore di entità
        - entità Abitazione, con attributo indisp (X,N)
        - diventa Abitazione - (X,N) - indisp - (1,N) ValoreIndisp, dove ValoreIndisp è un'entità avente attributo valore/Periodo (chiave)
    - attributo multivalore di relationship
        - Compagnia - (1,N) - sede - (0,N) - Città, dove sede ha tel (1,N)
        - diventa Compagnia - (1,N) - sCmp - (1,1) - Sede - (1,1) - sCtt - (0,N) - Città, dove Sede è un entità che presenta un vincolo di integrità che comprende sCmp e sCtt, e inoltre Sede - (1,N) - tel - (1,N) - ValoreTel, dove ValoreTel è un'entità che presenta un attributo valore (chiave)
- creare ogni dominio in SQL
    - creazione tipi stringa StringS, StringM e StringL (tutti varchar)
    - creazione tipi IntegerGZ, IntegerGEZ, RealGZ, RealGEZ (non è possibile usare check in type)
    - controllare che sia possibile implementare i vincoli dei domini
        - alcuni non sono realizzabili con clausole check, ed è necessario creare un trigger
    - TODO ci sono altre cose che dovrei scrivere?
- ristrutturare l'ER
    - rimuovere is-a e generalizzazioni tra entità
        - fusione
            - diventa una sola entità, avente un attributo 'tipo/{'E1', 'E2',...}' con i vari nomi delle varie ex-sottoentità, e comprende anche tutti gli attributi che avevano le sue sottoentità, con molteplicità (0,1)
            - vanno inseriti vincoli esterni per le relazioni che avevano solamente le sottoentità
                - se c'è un'istanza di una relazione che aveva solamente E1, allora il tipo è E1
            - vanno inseriti vincoli per le eventuali disgiunzioni se vi era una generalizzazione
            - vanno controllate le molteplicità delle relazioni che erano collegate alle is-a, e vanno aggiunti vincoli esterni
                - se la molteplicità era (1,X), deve diventare (0,X)
            - vanno inseriti vincoli esterni per gli attributi delle ex-sottoentità
        - duplicazione
            - TODO credo si possa fare solamente quando c'è una generalizzazione completa, o una roba del genere
        - aggiunta relationship
            - UtenteAttivo is-a Utente
            - diventa UtenteAttivo - (1,1) - isUtente - (0,1) Utente, e si pone un vincolo di integrità sulla relazione tra UtenteAttivio e Utente, dalla parte di UtenteAttivo
            - vanno inseriti vincoli esterni nel caso in cui vi era una generalizzazione, per disgiungere
    - rimuovere is-a tra relazioni
        - vanno inseriti vincoli esterni
        - attenzione se le relazioni avevano attributi multivalore (vanno riportate le is-a a cascata)
    - TODO RELAZIONI TRA PIÙ DI 2 ENTITÀ??
- scelta identificatori di ogni entità, e scelta di identificatori primari
    - ogni entità deve avere un identificatore primario, se un'entità non ha attributi chiave, si aggiunge un 'id', ed è anche primario
    - è possibile (e va fatto) usare i vincoli di integrità come chiavi
        - controllare che non siano presenti cicli di dipendenze negli identificatori primari
- riscrivere tutti i vincoli esterni (che hanno subito modifiche) con il nuovo alfabeto
- TODO VANNO RISCRITTI ANCHE GLI USE CASE?

## Schema relazionale

- entità
    - una tabella per ogni entità
    - un'attributo per attributo di entità
        - gli id sono di tipo serial
            - quando occorrono in altre tabelle, sono integer, perche serial lo decide il DBMS
    - definizione della chiave primaria (primary key)
    - vincoli di foreign key, ogni volta che un insieme di attributi definiscono una chiave di un'altra tabella
- relazioni
    - accorpamento
        - si può procedere, in teoria _sempre_, se il vincolo di molteplicità è (X,1)
        - vanno accorpate se sono comode per realizzare gli use-case, quando è frequente accedere a certi dati ottenibili attraverso dale relazione
        - vanno accorpate in un'entità, se sono parte di un identificatore di tale entità
        - vincolo di identificazione
            - primario
                - per poter esprimere i vincoli di identificazione primari, tutti i dati devono essere nella stessa relazione
            - non primario
                - i vincoli di identificazione esterni vanno gestiti come vincoli esterni (in FOL)
        - generalmente si accorpano le relazioni is-a, a fronte di una ristrutturazione senza fusione o duplicazione
    - tabella
        - oltre agli attributi di relationship, vanno inseriti anche gli attributi degli identificatori delle entità che collega (ed eventuali vincoli di foreign key, e di inclusione nelle tabelle delle entità che collega)
    - TODO double checcka tutti questi che stanno qua, just to make sure
    - molteplicità
        - (0,N) - rel - (0,N)
            - Studente - (0,N) - esame - (0,N) - Corso
            - Studente(_cf_: char(16), ...)
            - Corso(_id_: PosInt, ...)
            - esame(_studente_: char(16), _corso_: PosInt, ...)
                - primary key (studente, corso)
                - foreign key studente references Studente(cf)
                - foreign key corso references Corso(id)
        - (0,1) - rel - (0,N)
            - Studente - (0,1) - haTutor - (0,N) - Tutor
            - Studente(_cf_: char(16), ...)
            - Tutor(_id_: PosInt, ...)
            - haTutor(_studente_: char(16), tutor: PosInt, ...)
                - primary key (studente)
                - foreign key studente references Studente(cf)
                - foreign key tutor references Tutor(id)
        - (0,1) - rel - (0,1)
            - Docente - (0,1) - presiede - (0,1) - Facolta
            - tabella
                - Docente(_cf_: char(16), ...)
                - Facolta(_id_: integer, ...)
                - presiede(_docente_: char(16), facolta: integer, ...)
                    - primary key (docente)
                    - foreign key docente references Docente(cf)
                    - foreign key facolta references Facolta(id)
                    - unique facolta
            - accorpamento
                - versione 1
                    - Docente(_cf_: char(16))
                        - TODO primary key?
                    - Facolta(_id_: integer, ..., preside\*: char(16))
                        - TODO primary key?
                        - foreign key preside references Docente(cf)
                        - unique preside
                - versione 2
                    - Docente(_cf_: char(16), facoltaPresieduta\*: integer)
                        - TODO primary key?
                        - foreign key facoltaPresieduta references Facolta(id)
                        - unique facoltaPresieduta
                    - Facolta(_id_: integer, ...)
                        - TODO primary key?
        - (1,1) - rel - (0,N)
            - Studente - (1,1) - iscritto - (0,N) - Università
            - tabella
                - Studente(_cf_: char(16), ...)
                    - primary key (cf)
                    - foreign key cf references iscritto(studente)
                - Università(_nome_: varchar(100), ...)
                - iscritto(_studente_: università: varchar(100))
                    - TODO la chiave primaria?
                    - foreign key studente references Studente(id)
                    - foreign key università references Università(nome)
            - accorpamento
                - Studente(_cf_: char(16), ..., iscritto: varchar(100))
                    - TODO primary key?
                    - foreign key iscritto references Università(nome)
                - Università(_nome_: varchar(100), ...)
        - (1,N) - rel - (0,N)
            - Docente - (1,N) - insegna - (0,N) - Corso
            - Docente(_cf_: char(16), ...)
                - primary key (cf)
                - inclusione cf $\subseteq$ insegna(docente)
            - Corso(_id_: PosInt, ...)
            - insegna(_docente_: char(16), _corso_: PosInt)
                - TODO la chiave primaria?
                - foreign key docente references Docente(cf)
                - foreign key corso references Corso(id)
- relazioni n-arie
    - TODO mo non me va PAG 111R B.3.1.2.3.5

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

****

# iCare

****

