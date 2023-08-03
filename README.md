# RT Best practices

Questo documento riporta alcune best practices che possono aiutare a scrivere codice semplice da leggere e modificare in futuro.

---
Indice:

- 

---

## Git, github e versioning

Utilizzare tags, changelog files e semantic versioning puo' aiutarci nel sapere in che momento aggiungiamo quale feature, risolviamo bugs o facciamo altre modifiche ad un progetto.

### Semantic versioning - Semver

Il semantic versioning ha lo scopo di permettere di capire a colpo d'occhio il tipo di cambiamento che e' avvenuto fra versioni diverse (o commits) di una applicazione.
Si basa su uno standard curato dalla community (vesione [italiana](https://semver.org/lang/it/) e [inglese](https://semver.org/).
Esso prevede questo tipo di nomenclatura:

`[Versione maggiore].[versione minore].[patch/hotfix]`


```
ex. versione 1.2.4
```

**Versione maggiore**: cambiamenti che cambiano l'API in modo non retrocompatibile (per progetti interni, generalmente riscritture del progetto).
**Versione minore**: quando viene aggiunta una funzionalita' (in modo retrocompatibile).
**Patch**: correzioni di bugs.

Ogni volta che la versione maggiore viene incrementata, versione minore e patch tornano a 0; quando versione minore viene aggiornata, patch torna a 0.
```
Ex.
0.0.1 => Commit: BUG FIX - fixed button bug  =>  0.0.2
1.0.2 => Commit: Rewrite in Angular 17 => 2.0.0
2.3.4 => Commit: Feature - added table view => 2.4.0
```

Queste versioni possono essere "collegate" ai commit in questo modo:
```bash
git commit -m "<messaggio di commit>" # crea commit
git tag v0.0.1 # crea tag (aggiunta automaticamente all'ultimo commit)
git push origin main --tags # push tags verso github
```

I tags si possono vedere in questo modo
```git
git tag 
```

I tags hanno senso se usati assieme ad un changelog =>

### Changelogs - keepachangelog

I changelogs sono dei files che permettono di documentare in modo leggibile cio' che e' stato aggiunto, modificato, rimosso etc. in ogni versione del programma.
Si basa su uno standard tenuto dalla community (versione [italiana](https://keepachangelog.com/it-IT/1.0.0/) e [inglese](https://keepachangelog.com/en/1.1.0/)).

Esempio:
```md
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
### Fixed

- Improve French translation (#377).

## [1.1.1] - 2023-03-05

### Added

- Arabic translation (#444).
```

Ogni volta che si va a merge-are o rebase-are e si crea una nuova versione del progetto, bisognerebbe aggiungere le varie modifiche che sono state fatte alla versione in questo documento.

Ci sono vari tipi di cambiamenti:
- **Added**: nuove features
- **Changed**: modifiche di features presenti
- **Deprecated**: deprecazioni nel codice
- **Removed**: features rimosse
- **Fixed**: bug fixes
- **Security**: vulnerabilita' presenti/sistemate

E' buon uso tenere un header con `## [Unreleased]` durante lo sviluppo della versione, in modo da poter cambiare "unreleased" in "X.Y.Z" quando la versione viene rilasciata.

### Messaggi di commit

Anche i commit sono un tipo di documentazione.

In genere, bisognerebbe creare un commit per ogni numbero di modifiche che possono essere raggruppate assieme sotto uno di queste categorie:
- **Feat**: nuova feature
- **Fix**: bug fix
- **Chore**: rimozione di commenti, pulizia dei files, formattazione con prettier, aggiornare dependencies etc. - cio' che non riguarda fix, features e non cambia il codice della src o dei tests
- **Docs**: aggiornamenti sui files di documentazione (ad esempio, aggiungere righe al changelog)
- **Test**: nuovi tests o correzione di tests presenti
(ce ne sono altri, ma questi sono i casi principali)

La struttura di base e'
```
<tipologia di commit>: <descrizione>
```

Esempi:
```
Feat: added buttons to download pdf templates
Fix: program doesn't run out of memory anymore when exporting.
Chore: cleaned up imports
Test: added tests for the stores data service
```


### Git history e branches

Quando si sviluppa una nuova feature o si risolve un bug, e' buona cosa creare un nuovo branch su cui lavorare.
Esistono anche dei branches usati spesso in ogni progetto:
- **Main (o master)**: branch che ha il codice pronto per essere mandato in production; con codice rifinito e stabile
- **Development (o develop)**: branch che si usa durante lo sviluppo del programma, in cui si merge-ano i branch di features e fixes
- **feat/<branch name>**: branch usato per lo sviluppo di una feature
- **fix/<branch name>**: branch usato per lo sviluppo di un/piu' bug fix

Il flow tipico potrebbe essere:
1. Devo creare una nuova feature per gestire lo scroll su una mappa -> creo `feat/map-scroll`
2. Ho finito di sviluppare la feature e puo' unirsi alle altre features aggiunte dalle altre persone -> aggiorno CHANGELOG, merge-o su `development` e taggo il commit con la nuova versione (minor)
3. Rimuovo il branch `feat/map-scroll`
4. Mi richiedono di cambiare parte della feature -> creo `fix/map-scroll`
5. Ho finito di sviluppare il fix e puo' unirsi alle altre features aggiunte dalle altre persone -> aggiorno CHANGELOG, merge-o su `development` e taggo il commit con la nuova versione (patch)
6. Abbiamo raggiunto abbstanza features da rilasciare una versione stabile -> apro una PR per merge-are su `main`, sistemo cio' che potrebbe essere da sistemare e taggo il commit (major o minor)

!! Usare **rebase, amend, force-push etc.** solo quando si lavora su branches che si possiedono e che non sono condivisi con altre persone.
Rebase e' una azione distruttiva che cambia l'ordine dei commits e a cui non si puo' rimediare in caso di errori.
Rebase-are su branch condivisi porta anche il rischio di rimuovere il lavoro fatto da altre persone.

In caso di errori, si puo' create un nuovo commit con ex. `Fix: fixing problems regarding previous commits`.

---

## Commenti e documentazione nel codice

Un tipo di documentazione che e' presente nel codice sono i commenti, le type signatures e i nomi che si danno alle variabili.

### Nomi
Spesso un buon nome toglie il bisogno di usare commenti per dare informazioni aggiuntive.

**Nomi da non usare**:
- lettere singole
- soprannomi
- abbreviazioni
- acronimi

```js
// Avoid Single Letter Names
let n = 'use name instead';

// Avoid Acronyms
let cra = 'no clue what this is';

// Avoid Abbreviations
let cat = 'cat or category??';

// Avoid Meaningless Names
let foo = 'what is foo??';
```

**Linee guida utili**:
In genere, usare verbi per funzioni e nomi per variabili puo' aiutare a dare nomi migliori.
E' anche utile essere specifici quando un nome puo' essere generico (data, values, user...).
```js
function getValues() // invece che values()

const ApiStoresData // invece che data

const adminUser // invece che user
```


### Commenti

Si possono dividere in:
- **Docstrings**: commenti che descrivono la funzione di una funzione, classe o variabile, qualora il nome non sia abbastanza per intuirlo.
- **TODO**: commenti che descrivono cio' che andra' fatto in futuro, ma che non e' collegato al codice presente (simile ad una task da risolvere)
- **FIXME**: commenti che descrivono cio' che va fatto il prima possibile (codice scritto male che ha bisogno di refactor, valori hard-coded che devono essere scritti in modo dinamico, parti di codice che vanno estratte in funzioni specifiche, cambiamenti che non sono attuabili nella fase del progetto attuale ma in futuro lo saranno etc.)

Strutture dei commenti:
```js
/** QUESTA E' UNA DOCSTRING IN JAVASCRIPT, inizia con /** e si mette sopra alla variabile, funzione o classe che va a definire.
*   <messaggio, anche a piu' righe>
*/
const qualcosa = qualcosa

// TODO: Questo e' un todo, inizia con "TODO:"
// FIXME: Questo e' un fixme, inzia con "FIXME:"

```

Esempi:
```js
/**
* A test function written to test docstrings.
* FIXME: remove me later, just for testing
*/
function greetWorld() {
    console.log("Hello world")
}

// TODO: Add a new function that takes a name as a parameter and prints "hello ${name}"
```

Avere la documentazione nel codice permette che essa sia sempre aggiornata.  
Quando si modifica una funzione, una classe o la funzione di una variabile, e' importante controllare se anche la docstring ha bisogno di essere aggiornata.  

Quando invece si crea del codice particolarmente complesso, puo' essere utile commentare i vari passaggi e il **perche'** il codice e' scritto in questo modo.

Esempio:
```js
// Javascript has scoping problems in MyClass since higher order functions are used in some of its methods.
// Using this reference, we can avoid using "this" and avoid the scoping problem - https://stackoverflow.com/questions/40298991/understanding-javascripts-this-different-types-of-function-calls-and-how-to-c
let myClassThis

class Myclass {
    constructor() {
        myClassThis = this
        this.name = "john"
        this.speak(this.hello())
        this.speak(this.bye())
    }

    speak(message) {
        console.log(message())
    }

    hello() {
        return `Hello, ${myClassThis.name}`
    }

    bye() {
        return `Hello, ${myClassThis.name}`
    }
}
```

!! Nei commenti e' importante scrivere il **perche'** si e' fatta una scelta, non il **come** qualcosa funzioni: se si sente il bisogno di scrivedere come qualcosa sta funzionando, allora e' probabilmente troppo complesso.

Il codice dovrebbe essere leggibile e comprensibile, almeno per quanto riguarda la logica. I commenti possono dare un contesto, in cui il codice viene applicato.

### Quando scrivere le docstrings
Prima di fare un commit, e' utile aggiungere docstrings a parti del codice che sono "complete".

Se sto creando un component, aggiungero' una docstring che descrive per cosa viene usato il componente solo una volta completato.
Se invece sto scrivendo una piccola funzione, e' utile aggiungere subito una docstring, magari ancora prima di scrivere la funzione (descrivere cio' che stai per fare puo' aiutarti a capire meglio come farlo: [rubber duck debugging](https://en.wikipedia.org/wiki/Rubber_duck_debugging))

---

## Refactoring e tenere il codice pulito

Durante il tempo, si crea del debito tecnico dovuto alla mancanza di revisioni del codice.
E' buon uso avere alcuni accorgimenti sia durante la scrittura che durante la revisione del codice.

### Linee guida
Queste linee guida possono aiutare a riconoscere quando bisogna revisionare il codice (refactor). Non sono sempre applicabili, ma generalmente e' buona norma seguirle.

- **Le funzioni non dovrebbero essere piu' lunghe di 50 linee**; se lo sono e' probabile che serva *estrarre funzioni* (di piu' su questo in seguito)
- **Si fa fatica a dare nomi alle variabili e funzioni**; probabilmente la architettura/logica che si sta applicando e' troppo complessa
- **Si sente il bisogno di aggiungere commenti che spieghino cio' che succede nel codice**; probabilmente la logica e' troppo complessa e va semplificata
- **Il file e' piu' lungo di 300 righe**; probabilmente puo' essere diviso in diversi file con moduli specifici
- **Si fatica a navigare il file** probabilmente il file e' troppo lungo o la logica non lineare, ad esempio nel caso di una gestione dello stato fatta male
- **Una funzione richiede piu' di tre parametri**: probabilmente puoi ridurre il numero di parametri creando piu' funzioni invece che una sola
- **Stai provando a vedere cosa fa una funzione mettendoci cose a caso** (durante lo sviluppo, non durante reverse-engineering); probabilmente devi scrivere dei test che descrivono cio' che ti aspetti e cio' che non vuoi che succeda
- **Stai usando type signatures generiche o literals complessi**; probabilmente devi creare delle interfacce specifiche o semplificare la logica di cio' che stai scrivendo per usare tipi piu' semplici
- **Hai piu' di tre/quattro livelli di indentazione**; probabilmente devi *estrarre funzioni*.
- **Hai lo stesso codice (o molto simile) in piu' di tre punti**; probabilmente e' il momento di astrarre la logica comune e creare una funzione che gestisca i vari casi (creare astrazioni quando la duplicazione e' bassa puo' invece aumentare la complessita' della applicazione)

### Estrarre funzioni
Spesso le funzioni possono essere divise in sotto-funzioni piu' specifiche. Cio' permette di ridurre la complessita' generale delle funzioni, in quanto fanno una sola cosa (e quindi sono molto leggibili e prevedibili).

Esempio 
```js

// PRIMA DEL REFACTOR

// operation: "3 + 4"
function calculate(operation) {
    let operator
    if ("+" in operation){ operator = "+"}
    else if ("-" in operation){ operator = "-"}
    else if ("*" in operation){ operator = "*"}
    else if ("/" in operation){ operator = "/"}

    const operationsMap = {
    "+": (num1, num2) => num1 + num2,
    "-": (num1, num2) => num1 - num2,
    "*": (num1, num2) => num1 * num2,
    "/": (num1, num2) => num1 / num2,
    }

    const firstNumber = Number.parseInt(operation.split(+)[0].trim())
    const secondNumber = Number.parseInt(operation.split(+)[1].trim())

    return operationsMap[operator](firstNumber, secondNumber)
}

// DOPO IL REFACTOR
function calculate(operation) {
    const operator = extractOperator(operation)
    const operationFunction = getOperationFunction(operator)
    const [firstNumber, secondNumber] = extractNumbers(operation)
    
    return operationFunction(firstNumber, secondNumber)
}
```

Gli steps da fare durante l'estrazione di funzioni sono:
1. Prendi il codice e lo muovi in una funzione
```js
function calculate(operation) {
    const operator = extractOperator(operation)

    const operationsMap = {
    "+": (num1, num2) => num1 + num2,
    "-": (num1, num2) => num1 - num2,
    "*": (num1, num2) => num1 * num2,
    "/": (num1, num2) => num1 / num2,
    }

    const firstNumber = Number.parseInt(operation.split(+)[0].trim())
    const secondNumber = Number.parseInt(operation.split(+)[1].trim())

    return operationsMap[operator](firstNumber, secondNumber)
}

function extractOperator(operation) {
    let operator
    if ("+" in operation){ operator = "+"}
    else if ("-" in operation){ operator = "-"}
    else if ("*" in operation){ operator = "*"}
    else if ("/" in operation){ operator = "/"}

    return operator
}


```
2. Migliori il codice: aggiungi docstrings, lo ripulisci e se necessario rimuovi duplicazione del codice
```js
/**
* Get the algebraic operators from a string representing an operation.
*/
function extractOperator(operation: string) {
    return operation.match(/[-+*\/]/)[0]
}
```
3. Muovi le nuove funzioni in altri moduli, creandoli se necessario
```js
// component.js
function calculate(operation) {
    const operator = extractOperator(operation)
    const operationFunction = getOperationFunction(operator)
    const [firstNumber, secondNumber] = extractNumbers(operation)
    
    return operationFunction(firstNumber, secondNumber)
}

// handle-operations.js
function getOperationFunction() {...}

// data-extractors.js
function extractOperator() {...}
function extractNumbers() {...}
```

### Funzioni pure e side-effects

