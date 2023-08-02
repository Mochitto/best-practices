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

### Quando scrivere le docstrings
Prima di fare un commit, e' utile aggiungere docstrings a parti del codice che sono "complete".

Se sto creando un component, aggiungero' una docstring che descrive per cosa viene usato il componente solo una volta completato.
Se invece sto scrivendo una piccola funzione, e' utile aggiungere subito una docstring, magari ancora prima di scrivere la funzione (descrivere cio' che stai per fare puo' aiutarti a capire meglio come farlo: [rubber duck debugging](https://en.wikipedia.org/wiki/Rubber_duck_debugging))

---


