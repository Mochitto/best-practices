# Opening a pull-request

## Versione italiana

### Checklist

Quando si completa una feature, ci sono diversi passaggi da fare prima di aprire una pull-request:
- [ ] Il proprio branch e' in linea con `development`.
- [ ] I cambiamenti sono stati riportati sul CHANGELOG.
- [ ] Si sono risolti i `FIXME` che sono stati aggiunti al progetto. 
    - Se il `FIXME` non puo' essere risolto:
    - [ ] E' stata creata una issue per risolvere il `FIXME`   
- [ ] I messaggi di commit sono formattati correttamente (e non sono presenti commits da squashare).
- [ ] Il linter non riporta errori.
- [ ] Non ci sono regressioni (tutti i test si completano senza errori).

### Messaggio di Pull-request

Nel messaggio della PR andrebbe riportato il CHANGELOG, in modo che chi fa la review sa quali sono stati i cambiamenti maggiori.

Un buon formato potrebbe essere:
````
# <Nome riassuntivo della task>

## CHANGELOG
```
## [Unreleased]

### <tipologia>
- <messaggio>
- <messaggio>

### <tipologia>
- <messaggio>
- <messaggio>
```

### Note aggiuntive
- <Nota>
- <Nota>
````

Una volta che la pull-request viene accettata, sara' chi completa la review ad aggiungere la versione e taggare il merge.

## English version
### TODO
