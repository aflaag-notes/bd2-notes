# Misc

- domini, esempio del simbolo di predicato
    - sia $\mathrm{DataOra} \{ \mathrm{data}, \mathrm{ora} \}$ un dominio
    - sia $\mathrm E$ un'entità
    - sia $\mathrm{attr}$ un attributo dell'entità $\mathrm E$, di tipo $\mathrm{DataOra}$
    - sia $e$ una variabile tale che $\mathrm{E}(e)$
    - sia v una variabile tale che $\mathrm{attr}(e, v)$
    - allora, la variabile $d$ tale che $\mathrm{data}(v, d)$ è la data di $v$, e la variabile $o$ tale che $\mathrm{ora}(v, o)$ è l'ora di $v$ 
- TODO SCRIVI DOMINI RICORRENTI!!!!
- TODO CONTROLLA VINCOLI DI INTEGRITÀ PER LE RELATIONSHIP E LORO ATTRIBUTI

# TuTubi

## Vincoli

- valori distinti e progressivi per gli interi dei video delle playlist
    - per fare la progressione, per ogni valore $p$, ci deve essere un $p - 1$

# Rainair

## ER

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

# QuickHospital

## ER

- una prestazione contiene ricoveri e prestazioni esterne
- le persone sono solamente medici o pazienti (vincolo esterno), ma i medici possono essere pazienti (relazione is-a, e non generalizzazione, perché disgiunge)
- il medico non è paziente di sé stesso
- le stanze hanno un numero di stanza, anche se non c'è scritto

## Vincoli

- si può usare sovra-entità di una generalizzazione nei vincoli
- intervalli
    - data termine ricovero maggiore della data inizio ricovero
    - lo stesso posto letto non può avere due ricoveri contemporaneamente (impara a memoria il vincolo) TODO SCRIVILO
    - un paziente non può prenotare una prestazione esterna mentre è ricoverato (e viceversa)
    - un paziente non può essere ricoverato mentre è già ricoverato

## Use-Case

- ordinare un insieme di elementi
    - sorted(S: Entità (0,N), criterio): (e: Entità * n: intero > 0) (0,N)
        - S: insieme da ordinare
        - criterio: una funzione della forma criterio(e1: Entità, e2: Entità): bool, che definisce il criterio di ordinamento
            - criterio restituisce true se e solo se e1 <= e2
- posso chiamare più use-case in contemporanea, quindi possono non badare ai vincoli dell'ER

