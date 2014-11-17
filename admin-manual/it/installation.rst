=============
Installazione
=============

Per installare il server eseguire ::

  yum --enablerepo=nethvoice-11-upgrades,nethupgrade install nethcti-server

Dall'interfaccia grafica di configurazione di |parent_product|, cliccare il pulsante "Applica cambiamenti", se presente.

Per installare il client eseguire ::

  yum install nethcti

Installazione chat su |product_nethserver|
=================================================

Il server chat non viene installato automaticamente con |product|, perciò per utilizzare la chat è necessario installare ejabberd: ::

 yum install ejabberd

Aggiornamento dalla versione 2.x
================================

Per l'aggiornamento del server eseguire: ::

  yum --enablerepo=nethvoice-11-upgrades,nethupgrade update nethcti-server

L'aggiornamento del client avviene in automatico.

Aggiornamento dalla versione 1.x su |parent_product| 11
=======================================================

Per l'aggiornamento del server eseguire: ::

  service proxycti stop
  mkdir -p /home/e-smith/nethcti/backup
  mysqldump nethcti > /home/e-smith/nethcti/backup/nethcti.sql 2>/dev/null
  mysqldump asterisk > /home/e-smith/nethcti/backup/asterisk.sql 2>/dev/null
  rm -f /etc/nethcti/user_prefs.json /etc/nethcti/asterisk.json 2>/dev/null
  yum remove neth-cti
  yum --enablerepo=nethvoice-11-upgrades update nethcti-server

- Dall'interfaccia grafica di configurazione di |parent_product|, cliccare il pulsante "Applica cambiamenti", se presente.
- Nella sezione *CTI -> Profili* creare i profili desiderati.
- Nella sezione *CTI -> Utenti* associare gli utenti a profili e interni telefonici.
- Confermare con "Applica cambiamenti".

Migrare il database dei dati con il seguente comando: ::

  /usr/lib/node/nethcti-server/docs/migratedb.js `config getprop nethcti-server DbPasswd`

Durante la migrazione i dati del CTI (rubriche, postit, ecc) vengono convertiti automaticamente al nuovo formato.

Le configurazioni relative a:

- Gruppi Pannello Operatore
- SMS
- Streaming

vanno ricreate ex-novo attraverso l'interfaccia grafica di |parent_product|.

Per l'aggiornamento del client eseguire: ::

  yum install nethcti

**Migrare i template delle customer cards**

L'aggiornamento migra automaticamente le query delle customer cards, ma non i template che devono invece essere copiati manualmente nel path corretto tramite il comando: ::

 cp /usr/lib/node/proxycti/template/decorator_cc_* /home/e-smith/nethcti/templates/customer_card/

Se il template contiene delle immagini è anche necessario copiarle nel path: ::

 /home/e-smith/nethcti/static/img

e quindi adattare i relativi path nel template usando: ::

 /webrest/static/img/<FILENAME>

Aggiornamento dalla versione 1.x su |parent_product| 8
======================================================

Eseguire `l'aggiornamento <https://docs.nethesis.it/Aggiornamento_NethVoice_11>`_ a |parent_product| 11.

Per l'aggiornamento del server eseguire: ::

  service proxycti stop
  mkdir -p /home/e-smith/nethcti/backup
  mysqldump nethcti > /home/e-smith/nethcti/backup/nethcti.sql 2>/dev/null
  mysqldump asterisk > /home/e-smith/nethcti/backup/asterisk.sql 2>/dev/null
  rm -f /etc/nethcti/user_prefs.json /etc/nethcti/asterisk.json 2>/dev/null
  yum remove neth-cti
  yum --enablerepo=nethvoice-11-upgrades install nethcti-server

- Dall'interfaccia grafica di configurazione di |parent_product|, cliccare il pulsante "Applica cambiamenti", se presente.
- Nella sezione *CTI -> Profili* creare i profili desiderati.
- Nella sezione *CTI -> Utenti* associare gli utenti a profili e interni telefonici.
- Confermare con "Applica cambiamenti".

Migrare il database dei dati con il seguente comando: ::

  /usr/lib/node/nethcti-server/docs/migratedb.js `config getprop nethcti-server DbPasswd`

Durante la migrazione i dati del CTI (rubriche, postit, ecc) vengono convertiti automaticamente al nuovo formato.

Le configurazioni relative a:

- Gruppi Pannello Operatore
- SMS
- Streaming

vanno ricreate ex-novo attraverso l'interfaccia grafica di |parent_product|.

Per l'aggiornamento del client eseguire: ::

  yum install nethcti

**Migrare i template delle customer cards**

L'aggiornamento migra automaticamente le query delle customer cards, ma non i template che devono invece essere copiati manualmente nel path corretto tramite il comando: ::

 cp /usr/lib/node/proxycti/template/decorator_cc_* /home/e-smith/nethcti/templates/customer_card/

Se il template contiene delle immagini è anche necessario copiarle nel path: ::

 /home/e-smith/nethcti/static/img

e quindi adattare i relativi path nel template usando: ::

 /webrest/static/img/<FILENAME>
