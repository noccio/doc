---
title: Migrazione dell'email da un provider di email ed un sever YunoHost
template: docs
taxonomy:
    category: docs
routes:
  default: '/email_migration'
---

*[Documentazione relativa alla configurazione del server mail in Yunohost](/email)*.

La migrazione delle email da un server ad un altro si può eseguire con due strumenti raccomandati : ImapSync o Larch.

Questi programmi devono essere installati sul pc personale. Le procedure di trasferimento seguono lo schema seguente:

**`Vecchio server email -> computer personale con installati ImapSync o Larch -> nuovo server email`**

### ImapSync

[Sito web di ImapSync](http://imapsync.lamiral.info/)

Installate ImapSync sul computer personale seguendo questa [guida](http://imapsync.lamiral.info/INSTALL):
```bash
sudo dnf install imapsync # con Fedora
```
Migrazione delle mail tra i due server:
```bash
imapsync --host1 <dominio/IP> --port1 993 --ssl1 --user1 <utente> --password1 <password> \
--host2 <dominio/IP> --port2 993 --ssl2 --user2 <utente> --password2 <password>
```

Attenzione! i parametri di migrazione `--port 993` e `--ssl` sono specifici del proprio server mail YunoHost.

### Larch

[Sito web di Larch](https://github.com/rgrove/larch/)

Per prima cosa installate `gem`, in seguito installate `larch` sul vostro pc personale:
```bash
sudo gem install larch
```
Migrazione delle mail tra i due server:
```bash
larch -a -f imaps://server_di_origine.org -t imaps://server_di_destinazione.org
```
Consultate la [documentazione di Larch](https://github.com/rgrove/larch#label-Usage) per altre tipologie di migrazione.
