=========================
Personalizzazione servizi
=========================

È possibile personalizzare la configurazione di alcuni servizi solo manualmente attraverso la linea di comando.

Attivazione del debug
=====================

Di default il file di log riporta solamente messaggi di warning ed errori. È possibile innalzare il livello di debug per avere maggiori informazioni: ::

 config setprop nethcti-server LogLevel info
 signal-event nethcti-server-update

Per ripristinare il livello di debug: ::

 config setprop nethcti-server LogLevel warn
 signal-event nethcti-server-update

.. warning:: Innalzando il livello la dimensione del file di log aumenta rapidamente.

Configurazione Chat
===================

È possibile personalizzare il protocollo utilizzato dal client per connettersi al server ejabberd. ::

    config setprop nethcti-server Protocol "https"
    signal-event nethcti-server-update

I valori possibili sono *http* o *https*.

Configurare l'autenticazione su un server LDAP esterno
======================================================

Configurare il tipo d'autenticazione come "ldap": ::

    config setprop nethcti-server AuthType "ldap"

Configurare i parametri di connessione al server ldap esterno tramite i seguenti comandi: ::

    config setprop nethcti-server LdapBaseDN <BASE_DN>
    config setprop nethcti-server LdapOu <OU>
    config setprop nethcti-server LdapPort <LDAP_PORT>
    config setprop nethcti-server LdapServer <LDAP_SERVER>

Esempio: ::

    config setprop nethcti-server AuthType "ldap"
    config setprop nethcti-server LdapBaseDN "dc=mycompany,dc=local"
    config setprop nethcti-server LdapOu "Users"
    config setprop nethcti-server LdapPort "389"
    config setprop nethcti-server LdapServer "192.168.5.111"

.. note:: Per configurare ldap sicuro con SSL usare la porta 636.

Come ultima operazione eseguire: ::

    signal-event nethcti-server-update

Configurare l'autenticazione su Active Directory
================================================

Configurare il tipo d'autenticazione come "activeDirectory": ::

    config setprop nethcti-server AuthType "activeDirectory"

Configurare i parametri di connessione al server Active Directory tramite i seguenti comandi: ::

    config setprop nethcti-server LdapBaseDN <BASE_DN>
    config setprop nethcti-server LdapOu <OU>
    config setprop nethcti-server LdapPort <LDAP_PORT>
    config setprop nethcti-server LdapServer <LDAP_SERVER>

Esempio: ::

    config setprop nethcti-server AuthType "activeDirectory"
    config setprop nethcti-server LdapBaseDN "dc=mycompany,dc=local"
    config setprop nethcti-server LdapOu "Users"
    config setprop nethcti-server LdapPort "389"
    config setprop nethcti-server LdapServer "192.168.5.111"

Come ultima operazione eseguire: ::

    signal-event nethcti-server-update

Configurare un prefisso telefonico
==================================

Per configurare un prefisso per tutte le chiamate: ::

 config setprop nethcti-server Prefix 0039
 signal-event nethcti-server-update

Per rimuoverlo: ::

 config setprop nethcti-server Prefix ""
 signal-event nethcti-server-update

Personalizzare il mittente degli SMS
====================================

Se si utilizza *smshosting* come servizio web per l'invio degli SMS, è possibile personalizzare
il nome del mittente dei messaggi. È sufficiente aggiungere il seguente parametro all'url da inserire
nel pannello di configurazione di |parent_product|: ::

  mittente=nome_da_usare



.. _custom-nethifier-label:

Personalizzare |product_nethifier|
==================================

I :doc:`popup <nethifier>` possono essere personalizzati sia nella *grafica*, sia nel *contenuto*
attraverso i seguenti template HTML: ::

 /home/e-smith/nethcti/static/templates/notification_popup/call.html
 /home/e-smith/nethcti/static/templates/notification_popup/streaming.html

Il primo serve a mostrare i dati di una chiamata generica, mentre il secondo quelli di una
:ref:`sorgente video <streaming-video-label>`, quale ad esempio un videocitofono.

.. note:: *Conoscenze richieste: linguaggi di programmazione HTML, CSS e Javascript.*

Personalizzazione della grafica
-------------------------------

È sufficiente personalizzare il codice HTML e/o CSS dei template indicati. Supponiamo ad esempio di
voler modificare il colore del **nome e numero chiamante** in verde nel template ``call.html``: sarà sufficiente
aggiungere la regola css ``color: green`` nel selettore ``.contact-info``, che diventerà:

.. code-block:: css

    .contact-info {
        font-family: verdana;
        margin-top: 5px;
        margin-left: 15px;
        float: left;
        color: green;
    }

Personalizzazione del contenuto
-------------------------------

È possibile estendere le funzionalità presenti all'interno dei popup con nuovi comandi da eseguire.

1. Creare il template custom `win_popup.json`:

::

 mkdir -p /etc/e-smith/templates-custom/etc/nethcti/win_popup.json
 cp /etc/e-smith/templates/etc/nethcti/win_popup.json/10base /etc/e-smith/templates-custom/etc/nethcti/win_popup.json

2. Aprire il template appena creato con un editor di testi:

::

 vim /etc/e-smith/templates-custom/etc/nethcti/win_popup.json/10base

3. Aggiungere il nuovo comando all'interno dell'oggetto JSON `"commands"`, specificando
il percorso del programma eseguibile di Windows che si intenderà eseguire: ::

    ,"<NOME_NUOVO_COMANDO>": {
        "command": "<NOME_NUOVO_COMANDO>",
        "runwith": "<PATH_EXE>"
    }

Se ad esempio il nuovo comando si chiama **"gestionale"** e il programma da eseguire è
**"c:\\windows\\notepad.exe"**, la sezione da inserire sarà: ::

    ,"gestionale": {
        "command": "gestionale",
        "runwith": "c:\\\windows\\\notepad.exe"
    }


e quindi il template custom diventerà: ::

    {
        my $popupCtiProto = ${'nethcti-server'}{'PopupCtiProto'} || "https";

        $OUT = '{
        "call": {
            "width": "400",
            "height": "175"
        },
        "stream": {
            "width": "400",
            "height": "400"
        },
        "close_timeout": "10",
        "commands": {
            "url": {
                "command": "url",
                "runwith": ""
            }
            ,"gestionale": {
                "command": "gestionale",
                "runwith": "c:\\\windows\\\notepad.exe"
            }
        },
        "cti_proto": "' . $popupCtiProto .'"
    }';
    }

.. warning:: Il percorso dell'eseguibile di Windows deve utilizzare la stringa "\\\\\\" come separatore.

4. Adattare l'altezza del popup che si intende modificare, in base all'elemento grafico da aggiungere. Se ad esempio
si vuole inserire un nuovo pulsante nel template `"call.html"`, un'altezza pari a 175px può essere sufficiente:

::

    {
        "call": {
            "width": "400",
            "height": "175"
        },
        ...

5. Salvare la configurazione e uscire dall'editor di testi.

6. Eseguire il comando: ::

    signal-event nethcti-server-update

7. Personalizzare uno o entrambi i template HTML in base alle proprie necessità:
è necessario inserire un **elemento grafico** e **un'azione da eseguire** in
corrispondenza del click sullo stesso. Supponiamo ad esempio di voler inserire
un nuovo pulsante nel template *"call.html"* cliccando il quale eseguire il nuovo
comando "gestionale".

Il codice HTML del nuovo pulsante grafico da inserire in *call.html* sarà:

.. code-block:: html

    <div class="contact-action">
        <div id="open-gestionale-but" cmd="gestionale" arg="" close="1" class="button" title="">Gestionale</div>
    </div>

8. **Opzionale:**
se si desidera passare l'identificativo del chiamante come parametro al programma di Windows,
è necessario aggiungere il seguente codice javascript in coda alla funzione `window.onload`:

.. code-block:: javascript

 $('#open-gestionale-but').attr('arg', params.callerNum);

9. Eseguire |product_nethifier| in Windows e connettersi al server cti.

Da questo momento alla ricezione di una chiamata generica nel popup sarà presente
un nuovo pulsante di nome "Gestionale", cliccando il quale si aprirà il notepad di Windows.

Ogni client |product_nethifier| può inoltre personalizzare i path dei programmi da eseguire:
aprire l'interfaccia grafica |product_nethifier| attraverso la voce "Visualizza" del
menù contestuale dell'icona nella system tray di Windows, selezionare il tab "Esegui", personalizzare
i path e salvare la configurazione.

Personalizzazione del protocollo
--------------------------------

È possibili modificare il protocollo con cui aprire |product| tramite il click sul popup.
Eseguire: ::

 config setprop nethcti-server PopupCtiProto "<PROTO>"
 signal-event nethcti-server-update

dove <PROTO> può assumere i valori *http* o *https*.
