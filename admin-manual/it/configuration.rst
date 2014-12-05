==============
Configurazione
==============

Il pacchetto che consente la configurazione del CTI è |rpm_cti_conf_nethservice| in
NethService e |rpm_cti_conf_nethserver| in NethServer e viene installato automaticamente
come dipendenza.

Al termine dell'installazione è necessario collegarsi alla pagina di configurazione
di |parent_product| (http://_server_/nethvoice) e cliccare su :dfn:`"Applica modifiche"`.
Procedere quindi con la configurazione del CTI.

|product| 2 introduce il concetto di :index:`"Presence"` che ruota attorno all'utente:
ogni utente ha associati degli endpoint (interni, e-mail, cellulare, chat, ecc) presso
cui è raggiungibile. Il CTI è in grado di fornire una visione d'insieme dello stato
dell'utente.

L'autenticazione all'interno di |product| avviene con le credenziali dell'utente di
sistema. Quindi, ogni utente che deve accedere al CTI, è un utente di sistema.

Ciascun utente ha associato un profilo. Un profilo è composto da uno o più permessi
che descrivono quello che l'utente può fare all'interno del CTI. Ogni utente deve
inoltre obbligatoriamente essere associato ad un interno telefonico.

Oltre alla lista sopra citata, ciascun utente ha dei permessi aggiuntivi che comprendono:

* lista delle schede clienti personalizzate
* lista dei video in streaming (citofoni, ecc...)
* lista dei gruppi utenti del pannello operatore

Si consiglia di seguire quest'ordine per la configurazione:

* creazione dei profili comprendenti permessi, schede cliente, streaming e gruppi
* associazione dei profili a ciascun utente
* creazione delle sorgenti di streaming (opzionale)
* creazione delle customer card personalizzate (opzionale)
* creazione dei gruppi del pannello operatore (opzionale)
* configurazione della modalità di invio SMS (opzionale)

Schede Clienti
==============

Questa pagina consente la creazione e la modifica delle customer cards (schede clienti). Una :dfn:`customer card` è un modulo che consente di definire una query ad un
database (locale o remoto) al momento dell'arrivo di una chiamata e di
mostrare in |product| il risultato.

-  **Nome**: è il nome della customer card, deve essere univoco. "*Identity*"
   e "*Calls*" sono nomi riservati alle customer card di default.
-  **Tipo di database**: è il tipo di database dove verrà effettuata la
   query. Al momento sono supportati *mysql* e *mssql*.
-  **Porta Database**: è la porta usata per raggiungere il database. Nel
   caso di database locale è possibile utilizzare una
   sock al posto della porta (chiusa di default) => /var/lib/mysql/mysql.sock
-  **Host**: è l'host che ospita il database
-  **Query**: è la query da eseguire

Esempio
-------

L'esempio seguente crea una customer card *"ticket"* che, all'arrivo di
una chiamata, mostra in |product| i risultati di una query effettuata sul
database di otrs:

+-------------------------+-------------------------------------------------------------------------------------------------------+
| **Nome**                | ticket                                                                                                |
+-------------------------+-------------------------------------------------------------------------------------------------------+
| **Nome Visualizzato**   | ticket                                                                                                |
+-------------------------+-------------------------------------------------------------------------------------------------------+
| **Tipo di database**    | mysql                                                                                                 |
+-------------------------+-------------------------------------------------------------------------------------------------------+
| **Porta Database**      | /var/lib/mysql/mysql.sock                                                                             |
+-------------------------+-------------------------------------------------------------------------------------------------------+
| **Nome Database**       | otrs                                                                                                  |
+-------------------------+-------------------------------------------------------------------------------------------------------+
| **Database Username**   | otrs                                                                                                  |
+-------------------------+-------------------------------------------------------------------------------------------------------+
| **Database Password**   | \*\*\*                                                                                                |
+-------------------------+-------------------------------------------------------------------------------------------------------+
| **Query**               | SELECT T.title AS Titolo, date_format(T.create_time,'%d/%m/%Y %H:%i') AS c_time, date_format\         |
|                         | (T.change_time,'%d/%m/%Y %H:%i') AS m_time, CONCAT(U.first_name,' ',U.last_name) AS gestore,\         |
|                         | 'Cliente', TS.name AS stato, lp FROM ticket T INNER JOIN customer_user CU ON T.customer_user_id=\     |
|                         | CU.login INNER JOIN ticket_state TS ON T.ticket_state_id=TS.id INNER JOIN users U ON T.change_by=\    |
|                         | U.id WHERE (CU.phone LIKE '%$EXTEN%' OR CU.phone2 LIKE '%$EXTEN%' OR CU.phone3 LIKE '%$EXTEN%' OR \   |
|                         | CU.phone4 LIKE '%$EXTEN%' OR CU.phone5 LIKE '%$EXTEN%') LIMIT 10                                      |
+-------------------------+-------------------------------------------------------------------------------------------------------+
| **Visibile di default** | True                                                                                                  |
+-------------------------+-------------------------------------------------------------------------------------------------------+


Profili
========

Un profilo è composto da un insieme di permessi che descrivono quello che l’utente può fare all’interno del CTI. Di seguito l'elenco dei permessi e il relativo significato.

L'utente può:

**Spy**
    ascoltare le telefonate di qualsiasi interno telefonico (solo ascolto)

**DND**
    configurare il suo stato di "non disturbare"

**CDR**
    visionare lo storico delle proprie telefonate

**SMS**
    inviare SMS

    visionare lo storico dei propri SMS inviati

**Chat**
    utilizzare il servizio di chat

**Don't spy**
    disabilitare l'azione di spy sul proprio utente

**Post-it**
    creare/modificare/leggere/eliminare i post-it

    visionare lo storico dei propri post-it creati

    creare/modificare/leggere/eliminare le note sui chiamanti

    visionare lo storico delle proprie note create e quelle pubbliche di altri utenti

**Trunks**
    visualizzare tutti i fasci con le relative informazioni di stato

**Queues**
    visualizzare le code con le relative informazioni di stato

    effettuare il logon/logoff su/dalle code in cui i suoi interni sono membri dinamici

    attivare/disattivare lo stato di pausa sulle code in cui i suoi interni sono membri (statici e dinamici)

**Intrude**
    introdursi in una conversazione (ascoltare e parlare)

**Privacy**
    vedere offuscate l'identità dei chiamanti/chiamati di comunicazioni relative ad altri utenti

**Parkings**
    vedere i parcheggi con il relativo stato

    effettuare il pick-up di chiamate parcheggiate

**Admin CDR**
    visionare lo storico delle telefonate di tutti gli utenti

**Admin SMS**
    inviare SMS

    visionare lo storico degli SMS inviati da qualsiasi utente

**Admin Queues**
    visualizzare le code con le relative informazioni di stato

    effettuare il logon/logoff su/dalle code di tutti gli agenti dinamici

    attivare/disattivare lo stato di pausa di tutti gli agenti statici e dinamici delle code

    bypassa la privacy per tutte le chiamate in transito su una coda

**Recording**
    registrare le proprie conversazioni

    visualizzare/ascoltare/eliminare le proprie registrazioni

**Phonebook**
    utilizzare la rubrica e creare nuovi contatti

**Extensions**
    visualizzare gli utenti del pannello operatore e il loro relativo stato

    visualizzare il numero di nuovi messaggi vocali di tutti gli utenti

**Admin Pick-up**
    eseguire il pick-up di qualsiasi chiamate che sta squillando su un interno: non dai parcheggi

**Admin Post-it**
    creare/modificare/leggere/eliminare i post-it

    visionare lo storico dei post-it creati da tutti gli utenti

    creare/modificare/leggere/eliminare le note sui chiamanti

    visionare lo storico delle proprie note create e quelle pubbliche di altri utenti

**Admin Hangup**
    chiudere la conversazione di qualsiasi interno telefonico

**Admin Transfer**
    trasferire le chiamate di qualsiasi interno, tramite il trasferimento di tipo cieco

    trasferire le chiamate in attesa su una qualsiasi coda, tramite il trasferimento di tipo cieco

    trasferire le chiamate parcheggiate, tramite il trasferimento di tipo cieco

**Phone Redirect**
    configurare vari tipi di redirezioni automatiche sul proprio interno telefonico (CF, CFUnconditional, CFBusy, CF_VoiceMail)

**Admin Recording**
    registrare le conversazioni di qualsiasi interno telefonico

    visualizzare/ascoltare/eliminare le registrazioni di qualsiasi utente

**Attended Transfer**
    eseguire il trasferimento di chiamata consultativo delle proprie chiamate

**Streaming Permissions**
    visualizzare diverse sorgenti video scelte tra quelle create in precedenza

**Customer Cards Permissions**
    visualizzare le schede clienti scelte tra quelle create in precedenza. Di default sono abilitate "l'anagrafica" e quella che consente la visualizzazione dello "storico delle ultime chiamate"

**Operator Group Permissions**
    visualizzare gruppi di utenti del pannello operatore scelti tra quelli creati in precedenza

.. _user_configuration_ref_label:

Utenti
======

Ciascun utente ha associati degli endpoint (interni, e-mail, cellulare, chat, ecc) presso cui è raggiungibile. Questa sezione consente di associarne a piacere, come anche il profilo dei permessi da utilizzare.
Un interno telefonico può essere associato al più a un solo utente.

L'opzione "Mostra tutti gli interni nel pannello operatore" consente la visualizzazione degli interni non associati a nessun utente.

SMS
===

Consente la configurazione della modalità d'invio degli SMS.

-  **Tipo**: è possibile inviare SMS tramite web service di operatori
   esterni o utilizzando il *Portech*. La prima opzione è quella
   consigliata. Nel menù sono presenti alcuni operatori, con dei
   template di url predefiniti.
-  **Username**: login richiesto dal tipo d'accesso.
-  **Password**: password richiesta dal tipo d'accesso.
-  **Url**: i parametri necessari all'invio dell'SMS vengono inviati al
   server tramite l'URL (indipendentemente dal metodo GET o POST).
   Quando si configura un server personalizzato è necessario sapere che
   nome devono avere le variabili utente, password, numero e testo.

   Se un ipotetico servizio di hosting chiamasse l’utente "username" e la password "pass", l’URL risultante sarebbe del tipo: ::

     http://www.smshosting.it/smsMaster/invioSmsHttp.do?username=user&pass=password&numero=$NUMBER&testo=$TEXT&test=N

-  **Metodo**: è il metodo usato per l'invio dei parametri tramite web
   service. Se non è specificato diversamente dall'operatore, è consigliato
   l'utilizzo di GET.
-  **Prefisso**: è il prefisso internazionale ed è in generale
   obbligatorio (es. 0039 per l'Italia). Una volta configurato, tutti
   gli SMS saranno inviati con tale prefisso (es. in Italia solamente).
   Tuttavia l'utente |product| ha la possibilità di specificare un
   prefisso diverso anteponendolo al numero stesso nel campo di ricerca
   in rubrica.

-  Alcuni servizi richiedono anche il *mittente* come parametro: è
   sufficiente personalizzare l'URL. Ad esempio se è richiesto il
   parametro *mittente* e voglio che abbia valore *Pippo*, l'URL sarà
   del tipo: ::

     http://servizio.com/pagina.phpusername=$USER&pass=$PASSWORD&numero=$NUMBER&testo=$TEXT&mittente=Pippo

**Modalità d'invio tramite Portech:** gli SMS non verranno inoltrati
immediatamente, ma accodati. Ogni cinque minuti uno script si occupa
d'inviarli a destinazione in maniera sequenziale e di registrare l'esito
dell'operazione nel database. Tale modalità è dovuta alle limitazioni
dell'apparato. Nel campo Url si dovrà inserire *l'indirizzo IP del
Portech*. I messaggi vengono accodati in ``/var/spool/nethcti/sms/`` e lo script
che si occupa dell'inoltro è ``/usr/lib/node/nethcti-server/scripts/sendsms.php``.

.. note:: Se si utilizza il portech modello MV-374 è necessario specificare anche la porta 8023 nel campo Url. Se ad esempio l'IP del dispositivo è 192.168.1.5, l'url deve essere 192.168.1.5:8023



**Modalità d'invio tramite Web:** |product| è stato testato con il
servizio *smshosting*. A causa della diversa granularità nella gestione
degli errori da parte dei vari operatori, si garantisce l'esito
dell'operazione solo con tale servizio. Tuttavia è possibile utilizzare
liberamente altri gestori, tenendo in considerazione che in alcuni casi
l'esito d'invio potrebbe risultare positivo quando in realtà non lo è
(es. prefisso errato). È comunque possibile contattare l'assistenza in
caso di problemi o per la richiesta d'estensione del supporto.

Prefisso per SMS
----------------

*Il prefisso telefonico internazionale per l'invio degli SMS è in
generale obbligatorio.*

È possibile configurarlo in due modi:

#. tramite |product|, anteponendolo al numero inserito nel box di ricerca
#. nella configurazione lato server che vale per tutti gli utenti |product|

.. note:: la configurazione tramite il secondo metodo, non preclude la possibilità per l'utente, di inviare SMS utilizzando un prefisso diverso. Infatti il prefisso anteposto al numero nel box di ricerca, ha priorità rispetto a quello configurato col metodo due. Se tuttavia l'utente inserisce un numero telefonico privo di prefisso, allora verrà utilizzato quello del secondo metodo.

Esempio 1
^^^^^^^^^

L'amministratore configura il prefisso *0039* tramite il secondo metodo. L'utente Pippo, tramite |product| invia un SMS al numero *3331234567*. Il risultato è l'inoltro dell'SMS a *00393331234567*.

Esempio 2
^^^^^^^^^

L'amministratore configura il prefisso *0039* tramite il secondo metodo. L'utente Pippo, tramite |product| invia un SMS al numero *00303331234567*. Il risultato è l'inoltro dell'SMS a *00303331234567*.

Esempio 3
^^^^^^^^^
L'amministratore configura il prefisso *vuoto* tramite il secondo metodo. L'utente Pippo, tramite |product| invia un SMS al numero *3331234567*. Il risultato è l'inoltro dell'SMS a *3331234567*.


Streaming
=========

È possibile definire le sorgenti di streaming video che verranno poi mostrate in |product|. I permessi di ogni sorgente possono essere definiti per ogni utente.

I parametri per configurare una sorgente video sono:

-  **Nome**: è il nome della telecamera. Deve essere univoco.
-  **Descrizione**: è l'etichetta che sarà visibile nel client.
-  **Tipo**: indica il tipo di supporto
-  **Url**: è l'indirizzo della sorgente video.

   Qui vengono definite anche le dimensioni del video: ::

     http://INDIRIZZOIP/enu/cameraLARGHEZZAxALTEZZA.jpg

   LARGHEZZAxALTEZZA può assumere i valori 160x120, 320x240, 352x272, 352x288, 640x480

   Esempio: ::

     http://192.168.1.123/enu/camera640x480.jpg

-  **Username**: nome utente per l'accesso al dispositivo.
-  **Password**: password per l'accesso al dispositivo.
-  **Framerate**: è la frequenza di refresh delle immagini. Questo
   numero rappresenta i frame mostrati ogni 1/1000 (millesimo) di
   secondo. Per esempio, inserendo 1000 si avrà un frame al secondo.
-  **Interno**: è l'interno telefonico assegnato alla videocamera. Questo campo può
   essere omesso.
-  **Comando di apertura**: è il comando per aprire la porta, nel caso
   alla videocamera sia associato un citofono. Questo campo può essere
   omesso.


Code
====

Per ciascuna coda è necessario attivare le seguenti voci:

#. Evento quando si chiama
#. Evento su Stato Membri
