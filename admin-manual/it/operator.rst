============
|product_op|
============

Modalità Operative
==================

Il |product_op| è raggiungibile all'indirizzo: https://_server_/oppanel

Il |product_op| può lavorare in due modalità: 

#. con softphone integrato WebRTC
#. pilotando un telefono esterno Yealink o Snom entrambi con firmware recenti

Le due modalità sono mutuamente esclusive: non possono essere usate contemporaneamente.


.. warning:: Per utilizzare la modalità WebRTC è necessario accettare, meglio se definitivamente, il certificato all'indirizzo https://ip_server:8089/ws ed eventualemente anche http://nome_server.dominio:8089/ws

Installazione
=============

L'installazione del modulo |product_op| deve essere fatta dal modulo Gestione Pacchetti di |product_nethserver|.



Aggiornamento
=============

L'aggironamento del |product_op| è automatico.


Configurazione
==============

La configurazione del |product_op| è molto semplice e varia a seconda della modalità operativa scelta: con softphone o con telefono esterno

I parametri  base sono:

* Server Host/IP: ip o nome del centralino 
* Minimizza utenti: visualizzazione degli interni modalità compatta
* Visualizza operatore: visualizzazione del proprio interno insieme a tutti gli altri
* Utente: username e password per autenticazione LDAP
* Interno: interno da utilizzare nel |product_op|
* Password: password sip dell'interno da utilizzare
* Selezione Origine: tipologia interno fisico o webRTC
* Coda Esterna: coda per la gestione chiamate in ingresso
* Coda Attesa: coda per la gestione delle attese
* SIP: parametri per la configurazione sip, vengo configurati con un default automatico


.. note ::

  é fondamentale che su |parent_product|, nella gestione utenti del CTI, sia stato creato l'utente con cui viene fatta l'autenticazione LDAP, e che questo sia correttamente associato all'interno specificato in questa configurazione.


Modalità Softphone WebRTC integrato
===================================

Il |product_op| contiene un :dfn:`softphone` che può sostituire il telefono fisico.

Gli interni che si desidera utilizzare come softphone necessitano di una configurazione speciale (su |parent_product|).

Configurare le opzioni dell'interno come segue:

* **transport:** WS Only
* **encryption:** yes
* **directmedia:** no
* **videosupport:** no
* **icesupport:** yes
* **avpf:** yes



Funzionamento
=============

Il |product_op| è fondamentalmente diviso in due parti, la parte superiore che riguarda la gestione della chiamata e la parte inferiore che mostra in tempo reale la situazione degli altri interni.

La parte superiore è a sua volta divisa in tre parti.

La parte in alto a sinistra mostra lo stato della coda per le chiamate in ingresso dando l'elenco delle chiamate in coda, per ogni chiamata viene mostrata la posizione in coda, il numero chiamante, il nome associato al numero chiamante se riconosciuto dal |parent_product|, il tempo di attesa.

Il |product_op| consente tramite drag and drop di trascinare una di queste chiamate in attesa direttamente su un interno mostrato nella parte inferiore del |product_op| per effettuare un trasferimento di chiamata blindato o nella sezione centrale per rispondere.

La parte centrale superiore del |product_op| rappresenta il proprio interno telefonico sia che sia stata selezionata l'opzione telefono fisico sia per interno webRTC.
I tasti nella parte inferiore rappresentano le funzionalità telefoniche più comuni:

* accettare una chiamata o effettuarla
* chiudere una chiamata in corso
* mettere in attesa una chiamata
* trasferimento di una chiamata
* ripresa di una chiamata in attesa
* attivazione del tastierino numerico

Nella colonna di sinistra ci sono:

* pulsante per attivare la registrazione della chiamata in corso
* pulsante per creare una nota per la chiamata in corso
* pulsante per parcheggiare la chiamata in corso


Nella colonna di denstra invece:

* pulsante per la creazione di un postit
* pulsante per consultare la casella vocale
* pulsante per consultare il report delle chiamate

In caso di scelta per il telefono fisico i pulsanti funzionali vengono mappati con le funzionalità del telefono fisico e quindi ad esempio cliccando sul pulsante per chiudere una chiamata sarà fatto sul telefono.

La parte centrale della sezione dedicata al proprio interno funge da vero e proprio display per il proprio telefono.
Viene mostrato il totale delle conversazioni attive e la durata di quella in linea in più cliccandoci sopra è possibile digitare un numero o un nome per effettuare una chiamata.
La digitazione del terzo carattere attivirà la ricerca in rubrica.

La sezione superiore a destra mostra invece lo stato della coda di attesa e dei parcheggi.
La coda di attesa ha lo scopo di mettere in attesa le chiamate se si sta usando la modalità webRTC in quanto non è possibile utilizzare la funzionalità di attesa del telefono fisico.
Per mettere in attesa e riprendere le chiamate è comodamente utilizzabile il drag and drop anche in questo caso, sia verso il proprio interno, sia verso un interno mostrato nella sezione inferiore.
I parcheggi forniscono una modalità alternativa per mettere in attesa una chiamata, controllare nel manuale del |parent_product| le funzionalità specifiche.

La sezione inferiore mostra lo stato degli interni del centralino.
Gli interni sono ordinabili per nome, cognome o interno telefonico ed è possibile effettuare una ricerca nel box in alto a sinistra.
E' possibile vedere gli interni raggruppati secondo i gruppi pannello operatore configurati nel |parent_product|.
Cliccando sui vari interni è possibile interegire con essi e seconda del loro stato vegno abilitate le funzionalità:

* interno non occupato cliccandoci parte la chiamata ad esso
* interno occupato: termina chiamata in corso, ascolto chiamata in corso, intromissione chiamata in corso, registrazione chiamata in corso ed eventuale pausa o fine della registrazione

Cliccando invece sul simbolo della chat si apre una conversazione con l'utente, sul simbolo delle note è possibile creare una nota.
