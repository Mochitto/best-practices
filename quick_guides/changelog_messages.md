# Changelog messages

## Versione italiana

###### Exempi e dettagli possono essere trovati qui: https://keepachangelog.com/it-IT/1.0.0/

Una versione nel changelog deve essere formattata in questo modo:
```
## [<versione o 'unreleased'>]

### <categoria di messaggi>
- <messaggio>
```
Possono esserci piu' categorie di messaggi in una versione, ognuna con i propri messaggi.

Il CHANGELOG deve riportare i cambiamenti avvenuti fra le versioni, **sia a livello di codice che features**.

E' importante che i messaggi siano facilmente leggibili da chiunque, non solo da chi sta lavorando attivamente al progetto.

I messaggi hanno la prima lettera in maiuscolo e finiscono con un punto.

I messaggi del changelog sono divisi in queste categorie:

- **Features aggiunte**: per nuove features.
- **Features cambiate**: per cambiamenti a features esistenti.
- **Features deprecate**: per features che verranno rimosse nelle prossime versioni.
- **Features rimosse**: per features rimosse.
- **Problemi risolti**: per bug fixes.
- **Sicurezza**: in caso di vulnerabilita'.

### Creare un CHANGELOG per gli utenti
Un modo per creare automaticamente un CHANGELOG per gli utenti e' quello di contrassegnare i messaggi che possono essere mostrati nel CHANGELOG per gli utenti.

Un esempio utilizzato a [Raintonic](https://raintonic.com/):
I messaggi che hanno un "piu'" `+` all'inzio, verranno aggiunti al CHANGELOG che verra' mostrato all'utente.
```
## [1.2.3]

### Features aggiunte
- +Aggiunta la possibilita' di riordinare gli ordini nella tabella "Ordini in corso".
- Creato un nuovo endpoint 'customers/top-customers' per ottenere i clienti piu' attivi. 
```
Raintonic incrementa MINOR solo quando c'e' una nuova release: quando la `v1.3.0` verra' rilasciata, il primo messaggio verra' mostrato agli utenti, mentre il secondo non verra' riportato.  

In questo modo, risulta molto semplice creare uno script che crea il CHANGELOG degli utenti ogni volta che la versione viene aggiornata.

## English version

###### Examples and an in-depth explanation can be found at https://keepachangelog.com/en/1.1.0/

A version in the CHANGELOG should be formatted this way:
```
## [<version or 'unreleased'>]

### <messages group's kind>
- <message>
```
There can be more than one kind of messages in a version, each with their own messages.

The CHANGELOG file reports changes that happened between versions, **both code-wise and features-wise**.

It's crucial that the CHANGELOG file's messages are kept simple and easy to understand by everyone, not just people working on the project.

Changelog messages groups' kinds are:

- **Added**: for new features.
- **Changed**: for changes in existing functionality.
- **Deprecated**: for soon-to-be removed features.
- **Removed**: for now removed features.
- **Fixed**: for any bug fixes.
- **Security**: in case of vulnerabilities.

### Keeping a CHANGELOG for the end-users
A way to automatically build a end-users' CHANGELOG is to have a delimiter that marks which changelog's messages to add to the end-users' CHANGELOG.

This is an example of how [Raintonic](https://raintonic.com/) does it:
CHANGELOG messages that start with a "plus" `+` are added to the end-users' CHANGELOG.
```
## [1.2.3]

### Added
- +Users can now order their orders in the "Current orders" table.
- Created a new endpoint at 'customers/top-customers' that exposes the most lucrative customers.
```
Raintonic's versioning methodology increments MINOR only on new releases to the customer; when the `v1.3.0` will be released the first message will be shown to end-users, while the second one won't.

With this format, creating a parser that builds the end-users CHANGELOG becomes an easy feat.
