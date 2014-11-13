========
Sviluppo
========

.. _authentication-ref-label:

Autenticazione
==============

Lo scopo della fase d'autenticazione è creare un *token* che sarà usato per le operazioni future, quali l'invocazione delle REST API o per creare una connessione permanente (WebSocket o TCP).

Eseguire il login
-----------------

1. Il client invia una richiesta HTTP (POST) alla seguente rest api:

::

 https://<SERVER>/webrest/authentication/login

specificando lo _username_ e _password_ in formato JSON:

.. code-block:: json

 { "username": "my_user", "password": "my_password" }

2. Il client riceve una risposta standard HTTP 401. Se l'autenticazione ha avuto successo la risposta contiene un *nonce* (una stringa) nello header HTTP, altrimenti l'autenticazione è fallita. Un esempio di header è (il nonce è *06d15944d8ece69bdc97742b37c507970e2f6651*):

::

 www-authenticate: Digest 06d15944d8ece69bdc97742b37c507970e2f6651

3. Il client calcola il token:

.. code-block:: bash

 tohash = username + ':' + password + ':' + nonce
 token  = HMAC-SHA1(tohash, password)

4. Il client memorizza il token per usi futuri.

**Se il token d'autenticazione non viene mai usato, scade dopo un certo periodo di tempo (il default è mezz'ora). Una nuova fase d'autenticazione è necessaria.**


Eseguire il logout
------------------

1. Il client invia una richiesta HTTP (POST) alla seguente rest api:

::

    https://<SERVER>/webrest/authentication/logout

2. Il token viene rimosso dal server.

REST API
========

Le REST API sono dei Web Services che consentono l'interazione con il server |product|. È uno strumento molto utile per l'integrazione e lo sviluppo ed è la miglior soluzione da utilizzare con applicazioni mobile.

.. note::

 Questo documento assume che il lettore possegga già una certa familiarità con le più comuni tecniche di programmazione e Web Services.

Come usare una API
------------------

Di seguito vengono elencate le caratteristiche comuni a tutte le REST API:

* una API viene invocata tramite l'invio di una richiesta HTTP[S] al server |product| che risponde fornendo i dati richiesti o tramite un codice di stato o entrambi.
* I codici di stato delle risposte HTTP sono quelli `standard <http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html>`_.
* Il formato utilizzato per lo scambio dati è `JSON <http://www.json.org/>`_.
* Tutte le risorse sono raggiungibili tramite l'urli base:

::

  http[s]://<SERVER>/webrest/

* Per richiedere una API è necessario aggiungere un *patì* al baseurl che specifica la risorsa da richiedere. Per esempio:

::

  http[s]://<SERVER>/webrest/phonebook/search

* Ogni richiesta deve contenere i parametri d'autenticazione per il controllo d'accesso, specificando lo header HTTP ``Authorization``:

::

 Authorization: username:token

* Ogni richiesta viene *autenticata* e *autorizzata* del server |product|.


Autenticazione
--------------

#. L'autenticazione di una richiesta REST viene eseguita dal server controllando la validità del token passato. Quindi, come fase preliminare, Il client deve eseguire il login e deve creare un token d'autenticazione come descritto :ref:`qui <authentication-ref-label>`.
#. Ogni richiesta deve contenere lo header HTTP ``Authorization``:

::

    Authorization: username:token

**La validità del token d'autenticazione viene aggiornato ad ogni richiesta, altrimenti scade dopo un certo intervallo temporale (di default pari a un'ora). Dopo la scadenza è necessaria una nuova fase d'autenticazione.**


Autorizzazione
--------------

Ogni richiesta REST API viene autorizzata dal server che controlla i permessi utente configurati dall'amministratore attraverso l'interfaccia di |parent_product|.


Esempio d'uso con cURL
----------------------

L'esempio seguente mostra come eseguire una richiesta rest tramite `cURL <http://curl.haxx.se/>`_. Lo strumento si può rivelare utile per eseguire dei test e per comprendere il meccanismo delle chiamate con maggior dettaglio.

1. Supponiamo di volere effettuare la ricerca in rubrica del contatto *nethesis* per estrarre il numero telefonico. La prima operazione da eseguire è l'autenticazione (è necessaria solamente la prima volta per la costruzione del token).

::

 curl --insecure -i -X POST -d "username=my_user&password=my_password" https://192.168.5.226/webrest/authentication/login

L'autenticazione ha successo e il server risponde con:

::

 HTTP/1.1 401 Unauthorized
 Date: Thu, 12 Jun 2014 13:01:43 GMT
 www-authenticate: Digest f4700adb35ad29ee16afe6c03c0196dfc74ec3b1
 Content-Length: 0
 Content-Type: text/plain

2. Estraiamo il nonce dall'header *www-authenticate*:

::

 f4700adb35ad29ee16afe6c03c0196dfc74ec3b1

3. Costruiamo il token d'autenticazione:

::

 tohash = "my_user:my_password:f4700adb35ad29ee16afe6c03c0196dfc74ec3b1"
 token  = HMAC-SHA1("my_user:my_password:f4700adb35ad29ee16afe6c03c0196dfc74ec3b1", "my_password") = "1d8062d1c85a8fe6983745a1ee318d1cd9b8bde1"

4. Chiamiamo la rest api *phonebook/search*:

::

 curl --insecure -i -H "Authorization: my_user:1d8062d1c85a8fe6983745a1ee318d1cd9b8bde1" https://192.168.5.226/webrest/phonebook/search/nethesis

5. Il server invia la risposta in format JSON con i dati richiesti:

.. code-block:: json

 {
    "centralized": [
        {
            "id": 1916,
            "owner_id": "",
            "type": "Reseller",
            "homeemail": null,
            "workemail": "info@nethesis.it",
            "homephone": null,
            "workphone": "0721405516",
            "cellphone": "",
            "fax": "",
            "title": null,
            "company": "NETHESIS SRL ",
            "notes": "",
            "name": "",
            "homestreet": null,
            "homepob": null,
            "homecity": null,
            "homeprovince": null,
            "homepostalcode": null,
            "homecountry": null,
            "workstreet": "VIA DEGLI OLMI, 12",
            "workpob": null,
            "workcity": "PESARO",
            "workprovince": null,
            "workpostalcode": null,
            "workcountry": null,
            "url": "http://www.nethesis.it"
        }
    ],
    "nethcti": []
 }

Elenco delle API |product|
--------------------------

.. _/astproxy: http://dev.nethesis.it/nethcti/classes/plugin_rest_astproxy.html
.. _/authentication: http://dev.nethesis.it/nethcti/classes/plugin_rest_authentication.html
.. _/authorization: http://dev.nethesis.it/nethcti/classes/plugin_rest_authorization.html
.. _/callernote: http://dev.nethesis.it/nethcti/classes/plugin_rest_callernote.html
.. _/histcallernote: http://dev.nethesis.it/nethcti/classes/plugin_rest_histcallernote.html
.. _/all_histcallernote: http://dev.nethesis.it/nethcti/classes/plugin_rest_all_histcallernote.html
.. _/configmanager: http://dev.nethesis.it/nethcti/classes/plugin_rest_configmanager.html
.. _/custcard: http://dev.nethesis.it/nethcti/classes/plugin_rest_custcard.html
.. _/historycall: http://dev.nethesis.it/nethcti/classes/plugin_rest_historycall.html
.. _/histcallswitch: http://dev.nethesis.it/nethcti/classes/plugin_rest_histcallswitch.html
.. _/cel: http://dev.nethesis.it/nethcti/classes/plugin_rest_cel.html
.. _/phonebook: http://dev.nethesis.it/nethcti/classes/plugin_rest_phonebook.html
.. _/postit: http://dev.nethesis.it/nethcti/classes/plugin_rest_postit.html
.. _/historypostit: http://dev.nethesis.it/nethcti/classes/plugin_rest_historypostit.html
.. _/all_historypostit: http://dev.nethesis.it/nethcti/classes/plugin_rest_all_historypostit.html
.. _/sms: http://dev.nethesis.it/nethcti/classes/plugin_rest_sms.html
.. _/historysms: http://dev.nethesis.it/nethcti/classes/plugin_rest_historysms.html
.. _/all_historysms: http://dev.nethesis.it/nethcti/classes/plugin_rest_all_historysms.html
.. _/streaming: http://dev.nethesis.it/nethcti/classes/plugin_rest_streaming.html
.. _/voicemail: http://dev.nethesis.it/nethcti/classes/plugin_rest_voicemail.html

====================== ==================================================================================
Path                   Descrizione
====================== ==================================================================================
`/astproxy`_           Interazione con il server Asterisk
`/authentication`_     Funzionalità d'autenticazione
`/authorization`_      Funzionalità per i permessi utente
`/callernote`_         Funzionalità relative alle note sulle chiamate
`/histcallernote`_     Storico delle note sulle chiamate relative al proprio utente
`/all_histcallernote`_ Storico delle note sulle chiamate di tutti gli utenti del sistema
`/configmanager`_      Funzionalità relative alla configurazione degli utenti e di |product|
`/custcard`_           Funzionalità relative alle schede clienti
`/historycall`_        Storico delle chiamate del proprio utente
`/histcallswitch`_     Storico delle chiamate di tutti gli utenti del sistema
`/cel`_                Consente di recuperare informazioni dettagliate sulle chiamate dal CEL di Asterisk
`/phonebook`_          Funzionalità relative alle rubriche
`/postit`_             Funzionalità relative ai POST-IT
`/historypostit`_      Storico dei POST-IT dell'utente
`/all_historypostit`_  Storico dei POST-IT di tutti gli utenti del sistema
`/sms`_                Funzionalità relative agli SMS
`/historysms`_         Storico degli SMS dell'utente
`/all_historysms`_     Storico degli SMS di tutti gli utenti del sistema
`/streaming`_          Funzionalità sulle sorgenti video
`/voicemail`_          Funzionalità relative alle voicemail
====================== ==================================================================================

WebSocket
=========

La connessione WebSocket viene utilizzata dal server per comunicare in tempo reale con tutti i client connessi (ad esempio per notificare gli eventi del server Asterisk).
Per stabilire una connessione WebSocket col server |product| è necessaria una prima fase d'autenticazione.

Eseguire il login
-----------------

1. Il client esegue il login e crea un nuovo token d'autenticazione come descritto :ref:`qui <authentication-ref-label>`.
2. Il client stabilisce una connessione websocket con il server (la porta di default sicura è la 8181).
3. Il client invia il messaggio *login* al server attraverso la connessione websocket specificando *username* e *token* in formato JSON:

::

 socket.emit('login', { accessKeyId: username, token: token.toString() });

4. Se il login ha avuto successo il client riceve il messaggio *authe_ok*, altrimenti il messaggio *401* e il client viene disconnesso.

**Una volta completato il login con successo, il token ha validità infinita fino al riavvio del server.**


Integrazione di applicazioni legacy
===================================

Per mantenere compatibilità con le :index:`applicazioni legacy` che usano le API della versione 1.x, |product| offre la possibilità di fare telefonate invocando una particolare API senza autenticazione. **Questa funzionalità è disabilitata di default per motivi di sicurezza.**

**Per l'attivazione eseguire:**

::

 config setprop nethcti-server UnAutheCall enabled
 signal-event nethcti-server-update


Una volta attivata è possibile fare una telefonata eseguendo la richiesta HTTP GET:

::

 https://<SERVER>/webrest/astproxy/unauthe_call/:endpoint/:number


dove :dfn:`:endpoint` deve essere sostituito con l'interno telefonico che si vuole utilizzare e *:number* deve essere sostituito con il numero da chiamare.

Esempio per chiamare il numero *0721405516* tramite l'interno *214* tramite il server *nethvoice.server.it*:

::

 https://nethvoice.server.it/webrest/astproxy/unauthe_call/214/0721405516

Può essere utilizzato anche il protocollo HTTP.


.. warning::

   Se la funzionalità viene abilitata, chiunque può eseguire telefonate da qualsiasi interno verso qualsiasi destinazione tramite una richiesta HTTP GET.


**Per la disabilitazione eseguire:**

::

  config setprop nethcti-server UnAutheCall disabled
  signal-event nethcti-server-update
