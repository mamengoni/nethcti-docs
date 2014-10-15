========
Supporto
========

Prima analisi
=============

Prima di richiedere supporto può rivelarsi utile analizzare il file di log: spesso aiuta ad individuare
immediatamente il problema. Il file di log di |product| é: ::

 /var/log/asterisk/nethcti.log

Richiesta di supporto tecnico
=============================

Prima di richiedere supporto, assicurarsi che tutti i client si colleghino al server utilizzando il Fully Qualified Domain Name (FQDN) e quindi che risolvano correttamente il nome del server.

Tutte le richieste possono essere inoltrate attraverso il `portale di supporto <helpdesk.nethesis.it>`_. Ciascuna richiesta dovrà:

* contenere i dati per l'accesso SSH alla macchina (indispensabile per fornire assistenza)
* indicare le seguenti informazioni sul client utilizzato:

 * `versione del browser`
 * `versione del sistema operativo`
 * `se possibile, le credenziali di accesso dell'utente`
 * `versione dei pacchetti installati:`

 ::

  rpm -q nethcti nethcti-server nethcti-nethvoice-module neth-oppanel neth-queueman


Comandi utili
=============

========================================= =======================================================
Comando                                   Descrizione
========================================= =======================================================
``tail -f /var/log/asterisk/nethcti.log`` Mettersi in tail sul log
``service nethcti-server restart``        Riavvio del server |product| su |product_nethservice|
``restart nethcti-server``                Riavvio del server |product| su |product_nethserver|
``signal-event nethcti-server-update``    Espansione dei templates e riavvio del server |product|
========================================= =======================================================
