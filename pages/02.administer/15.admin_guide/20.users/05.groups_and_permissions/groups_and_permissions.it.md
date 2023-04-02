---
title: Gruppi utente e permessi
template: docs
taxonomy:
    category: docs
routes:
  default: '/groups_and_permissions'
---

Potete accedere all'interfaccia di gestione dei gruppi e dei permessi, partendo
dalla pagina webadmin recandovi alla sezione 'Utenti' e cliccate sul pulsante corrispondente

![](image://button_to_go_to_permission_interface.png)

## Gestione dei gruppi

La gestione dei gruppi permette di definire gruppi di utenti in gruppi che possono essere usati per la gestione dei permessi di utilizzo di applicazioni e di altri servizi (come l'email o chat XMPP). *Non* è obbligatorio riunire gli utenti in gruppi per assegnare loro permessi o divieti, che possono essere assegnati individualmente ad ogni utente.

L'uso dei gruppi è comunque raccomandato per semplificare la gestione. Ad esempio se sul server sono presenti account di diversa natura (un club, un'associazione, un'impresa) risulta più semplice impostare permessi e divieti al gruppo come ad esempio `associazione` ed aggiungere i rispettivi partecipanti al gruppo rispettivo.

### Gruppi di default

Ogni installazione crea automaticamente due gruppi:
- `tutti gli utenti`, che raggruppa tutti gli utenti registrati sul server YunoHost,
- `visitatori`, che raggruppa chi sta utilizzando il server senza essere registrato.

Non è possibile cambiare il contenuto di questi due gruppi ma solo i loro permessi.

### Visualizzare i gruppi esistenti.

[ui-tabs position="top-left" active="0" theme="lite"]
[ui-tab title="Dall'interfaccia web"]
I gruppi esistenti sono elencati all'inizio della pagina *gruppi e permessi*.

![](image://groups_default-groups.png)

[/ui-tab]
[ui-tab title="Dalla linea di comando"]

Per elencare i gruppi esistenti dalla riga di comnando digitate:

```shell
$ yunohost user group list
groups:
  all_users:
    members:
      - alice
      - bob
      - charlie
      - delphine
```
[/ui-tab]
[/ui-tabs]

### Creare un nuovo gruppo

[ui-tabs position="top-left" active="0" theme="lite"]
[ui-tab title="Dall'interfaccia web"]
Per creare un nuovo gruppo cliccate sul pulsante "Nuovo gruppo" in alto della pagina. Per il nome del gruppo si possono usare solo lettere (minuscole e maiuscole) e spazi. Il nuovo gruppo sarà creato senza utenti e senza permessi.

![](image://groups_button-new-group.png)

[/ui-tab]
[ui-tab title="Dalla linea di comando"]
Per creare il gruppo `yolo_crew` dobbiamo digitare:

```shell
$ yunohost user group create yuno_crew
```
[/ui-tab]
[/ui-tabs]

### Modificare un gruppo

[ui-tabs position="top-left" active="0" theme="lite"]
[ui-tab title="Dall'interfaccia web"]
Aggiungiamo un utente a questo gruppo: nel pannello relativo al gruppo cliccate sul pulsante "Aggiungi Utente" e scorrete fino all'utente desiderato e cliccateci.

![](image://groups_button-add-user.png)

Per eliminare l'utente dal gruppo, cliccate sulla X che si trova vicino al suo none nel pannello del gruppo.

![](image://groups_button-remove-user.png)

[/ui-tab]
[ui-tab title="Dalla linea di comando"]
Dalla linea di comando digitate il comando seguente per aggiungere `charlie` e `delphine` al gruppo `yolo_crew`:

```shell
$ yunohost user group update yolo_crew --add charlie delphine
```

(Allo stesso modo, `--remove` può essere usato per rimuovere un utente dal gruppo)

La lista dei gruppi mostrerà:

```shell
$ yunohost user group list
groups:
  all_users:
    members:
      - alice
      - bob
      - charlie
      - delphine
  yolo_crew:
    members:
      - charlie
      - delphine
```
[/ui-tab]
[/ui-tabs]

### Cancellare i gruppi

[ui-tabs position="top-left" active="0" theme="lite"]
[ui-tab title="Dall'interfaccia web"]
Per eliminare un gruppo, cliccate sulla X rossa in alto a destra nella scheda  dei gruppi. Vi verrà chiesta conferma dell'operazione

![](image://groups_button-delete-group.png)

[/ui-tab]
[ui-tab title="Dalla linea di comando"]
Per eliminare il gruppo `yolo_crew` dalla linea di comando digitate

```shell
$ yunohost user group delete yolo_crew
```
[/ui-tab]
[/ui-tabs]

### Gestione dei permessi

Il meccanismo dei permessi permette di impedire l'accesso a determinati servizi e/o programmi (ad esempio mail, XMPP...) o a specifiche parti di essi (esempio l'interfaccia di amministrazione di WordPress).

### Elenco dei permessi

[ui-tabs position="top-left" active="0" theme="lite"]
[ui-tab title="Dall'interfaccia web"]
La sezione gruppi elenca i permessi concessi ad ogni gruppo, e comprende e gruppi speciali quali `Tutti gli utenti` e `Visitatori`.

![](image://groups_default-with-permissions.png)

[/ui-tab]
[ui-tab title="Dalla linea di comando"]
Per visualizzare i permessi e i gruppi che ne usufruiscono, dalla linea di comando:

```shell
$ yunohost user permission list
permissions:
  mail.main:
    allowed: all_users
  wordpress.admin:
    allowed:
  wordpress.main:
    allowed: all_users
  xmpp.main:
    allowed: all_users
```

In questo esempio sopra vediamo che tutti gli utenti possono utilizzare il servizio mail, XMPP e accedere al blog Worpress. Nessuno può accedere all'interfaccia di amministrazione di Wordpress.

Maggiori dettagli possono essere visualizzati aggiungendo l'opzione `--full` che mostrerà l'elenco degli utenti corrispondenti ai gruppi autorizzati e gli URL associati al permesso (nel caso di applicazioni web).
[/ui-tab]
[/ui-tabs]

### Aggiungere un permesso a un gruppo o un utente

[ui-tabs position="top-left" active="0" theme="lite"]
[ui-tab title="Dall'interfaccia web"]
Per aggiungere un permesso a un gruppo è sufficiente cliccare sul pulsante "+" nel pannello del gruppo che si vuole modificare, scorrere l'elenco fino al permesso che si vuole aggiungere e cliccare sul permesso scelto.

![](image://groups_add-permission-group.png)

Notate che è possibile dare un permesso anche ad un singolo utente usando il pannello al fondo della pagina.

![](image://groups_add-permission-user.png)

[/ui-tab]
[ui-tab title="Dalla linea di comando"]
Per aggiungere il permesso di accesso all'interfaccia di amministrazione di WordPress:

```shell
$ yunohost user permission update wordpress.admin --add yolo_crew
```

Notate che è possibile aggiungere un permesso di accesso per un singolo utente:

```shell
$ yunohost user permission update wordpress.admin --add alice
```

Come possiamo vedere sia il gruppo YoloCrew che l'utente alice hanno adesso accesso all'interfaccia di amministrazione di WordPress:

```shell
$ yunohost user permission list
  [...]
  wordpress.admin:
    allowed:
      - yolo_crew
      - alice
  [...]
```

Notate che, se ad esempio volessimo eliminare l'accesso all'email di modo che sia ammesso solo l'utente Bob, è necessario rimuovere anche `Tutti gli utenti` dall'accesso all'email dal pannello dei gruppi dalla riga di comando:

```shell
$ yunohost user permission update mail --remove all_users --add bob
```
[/ui-tab]
[/ui-tabs]

Notate che alcuni permessi sono "protetti", non potrete quindi aggiungerli o rimuoverli dal gruppo Visitatori. Questa protezione si trova nei casi dove aggiungere o togliere permessi non risponde a logica (o dove si corra il rischio di creare problemi di sicurezza).

Un avviso di sicurezza verrà emesso nella pagina di amministrazione se definite o modificate un permesso già specificato in un gruppo più ampio.

![](image://groups_alerte-permission.png)

### Nascondere o mostrare le tiles nell'interfaccia utente.

Dalla versione 4.1 si possono nascondere le tiles nella pagina iniziale.
[ui-tabs position="top-left" active="0" theme="lite"]
[ui-tab title="Dall'interfaccia web"]
Dalla pagina di amministrazione potete scegliere quale tile mostrare: scegliete la scheda `Applicazioni`, poi l'applicazione della quale volete visualizzare o nascondere la tile scegliendo l'opzione in `Mostra il tile nel portale dell'utente`.

[/ui-tab]
[ui-tab title="Dalla linea di comando"]

Dalla linea di comando digitate:

```shell
# Enable the tile for the WordPress admin interface
$ yunohost user permission update wordpress.admin --show_tile True
```
[/ui-tab]
[/ui-tabs]

