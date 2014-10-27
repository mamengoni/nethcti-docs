=========
Nethifier
=========

|product_nethifier| Windows Integration è l'applicazione per Microsoft Windows che consente l'integrazione di |product| con il desktop Windows. In particolare consente la visualizzazione di popup di notifica nel desktop in corrispondenza della ricezione di telefonate dirette a un qualsiasi interno telefonico dell'utente.

Versioni di Windows compatibili: Windows 7 (64 bit) e Windows 8.

Per la personalizzazione dei popup seguire la :ref:`documentazione. <custom-nethifier-label>`

Requisiti
=========

- .NET Framework 4.5.
- Server |product| 2.x.
- Il pc su cui è in esecuzione |product_nethifier| deve risolvere l'indirizzo ``nomenethvoice.dominio``.

.. note:: Per ottenere il nome di dominio si può utilizzare il comando ``hostname -f``

Installazione
=============

È sufficiente effettuare il download dell'installer
ed eseguirlo. Terminata la fase d'installazione il programma verrà
avviato automaticamente.

Descrizione
===========

|product_nethifier| è sempre visibile tramite l'icona presente nella
system tray di Windows cliccando la quale appare un menu con varie opzioni.

All'arrivo di una chiamata a uno degli interni telefonici associati all'utente,
viene visualizzato un popup nel desktop che riporta informazioni sul chiamante.

Esistono due tipi di popup:

#. **generico** con informazioni sul chiamante
#. **di streaming** quando qualcuno suona al videocitofono

Il secondo tipo mostra l'immagine della persona che sta suonando il videocitofono.

È possibile interagire coi popup in diversi modi, ad esempio visualizzando la customer card del chiamante oppure comandando l'apertura della porta associata al videocitofono.

I popup sono estremamente personalizzabili, sia *nell'interfaccia grafica*, sia soprattutto nel *contenuto*. È infatti possibile aggiungere un qualsiasi comando da eseguire direttamente col click sul popup, come ad esempio l'apertura di un gestionale, l'invocazione di un url http, come anche l'esecuzione di un applicativo Java. In generale è possibile eseguire qualsiasi applicativo di Windows.

Il colore dell'icona del programma presente nella system tray indica lo stato di connessione col server cti. Il blu indica che |product_nethifier| è connesso, mentre il colore rosso indica un problema di connessione. In corrispondenza di una disconnessione di rete, che può accadere per vari motivi, |product_nethifier| tenta di riconnettersi in automatico per un certo intervallo temporale (circa 3 minuti). In ogni modo è possibile forzare la riconnessione attraverso il menù contestuale.

Configurazione
==============

È necessaria solamente la prima volta. I dati da inserire sono:

-  indirizzo del server (nome o IP)
-  credenziali dell'utente

È necessario salvare i dati cliccando il pulsante apposito in maniera tale da
non doverli più reinserire agli avvii successivi. È anche possibile scegliere
di eseguire |product_nethifier| all'avvio di windows.

Rimozione
=========

È sufficiente aprire il menu *Programmi* di Windows e cliccare la voce
"|product_nethifier| -> Uninstall".

Download
========

Il download è disponibile nell'area relativa https://docs.nethesis.it/Area_Download.
