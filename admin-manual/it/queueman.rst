==================
|product_queueman|
==================

Modalità Operative
==================

Il modulo |product_queueman| nasce per mostrare il funzionamento di tutte le code configurate in |parent_product|, sia in tempo reale e sia per quello che riguarda la giornata corrente; per dati più storicizzati vedere il modulo |product_queuereport| del |parent_product|.

Il |product_queueman| è raggiungibile all'indirizzo: ::

 https://_server_/queueman

L'accesso è consentito agli utenti configurati nel modulo Utenti del |parent_product| per la gestione degli utenti per il |product|.

Installazione
=============

Per installare eseguire: ::

 yum --enablerepo=nethupgrade install neth-queueman


Aggiornamento
=============

Per aggiornare eseguire: ::

 yum --enablerepo=nethupgrade update neth-queueman


Configurazione
==============

La configurazione del |product_queueman| avviene al momento dell'autenticazione, dove oltre ai parametri di accesso, bisogna indicare le modalità di utilizzo.

I parametri richiesti sono:

* Minimizza agenti: visione degli agenti in maniera miniaturizzata, questa modalità comporta una perdita di informazioni accessorie (stato chat, stato post-it in alto e in basso stato casella vocale, stato deviazioni di chiamata, stato non disturbare, stato agente generico, stato login |product|)
* Scheda Predefinita: scheda da aprire di default dopo il login
* Utente: dati di accesso al |product_queueman|, username e password
* Code: elenco delle code configurate in |parent_product| tra le quali scegliere quelle da visualizzare nel |product_queueman|.

Funzionamento
=============

Il |product_queueman| si articola in diverse schede.

Come prima cosa vengono mostrate le code selezionate al momento dell'autenticazione, una in ogni scheda, poi ci sono le schede di default Realtime, Riepilogo e Monitor.


Coda
----

Le schede delle code sono divise verticalmente a metà circa, la parte sinistra è dedicata alle chiamate, la parte destra agli agenti.

La parte delle chiamate è a sua volta divisa in due parti, la parte superiore per le chiamate in attesa, la parte inferiore per le chiamate connesse.

Per le chiamate in attesa vengono mostrati la posizione in coda, il numero e/o il nome del chiamante e il tempo dell'attesa.

E' possibile interagire con le chiamate in attesa assegnadole direttamente ad un agente disponibile con drag and drop.

.. warning:: Forzare l'assegnazione di una chiamata con questa procedura fa uscire la chiamata dalle statistiche della coda

Per le chiamate connesse vengono invece mostrati numero e/o il nome del chiamante, tempo di conversazione e operatore connesso.

In alto in questa sezione vengono mostrati i totali in tempo reale, il totale delle chiamate in attesa e il totale delle chiamate connesse.

Nella parte di destra dedicata agli agenti vengono mostrati tutti gli agenti membri della coda, sia statici che dinamici.

La parte centrale del box di ogni agente mostra lo stato di login (verde), pausa (giallo) logout (grigio) ed è l'unica sezione mostrata se viene scelto al momento del login minimizza agenti.

Questa sezione quando l'agente risponde ad una chiamata proveniente dalla coda visualizzata si colorerà di azzurro, mentre se l'agente si trova al telefono per una chiamata estranea alla coda sarà colorata di rosso.

La sezione in alto a sinistra del box agente indica lo stato telefonico dell'interno del |parent_product| utilizzo dall'agente per il login, libero (verde), occupato (rosso), scollegato (grigio).

Successivamente ci sono i box per lo stato chat e i post-it.

Nella parte inferiore c'è la sezione di notifica per stato casella vocale, stato deviazioni di chiamata, stato non disturbare, stato agente generico, stato login |product|.

A seconda dello stato dell'agente il click sul suo box abilita funzionalità diverse:

* interno non occupato: cliccandoci è possibile gestire la sua connessione alla coda facendo login, pausa, ripresa dalla pausa, logout a seconda dello stato
* interno occupato: cliccandoci è possibile interagire con la chiamata in corso ascoltandola o intromettendosi, in più rimangono attive a seconda dello stato le funzionalità per gestire la connessione alla coda elencate prima

In alto viene mostrato in tempo reale il totale degli agenti liberi, degli agenti non loggati, degli agenti in conversazione e degli agenti in pausa.


Realtime
--------

La scheda Realtime ha la funziona di mostrare in tempo reale lo stato di tutte le code del |parent_product| e di tutti gli agenti, tutti i dati vengono aggiornati in tempo reale.

Questa sezione è divisa orizzontalmente in due parti, la parte superiore dedicata alle code, quella inferiore agli agenti.

Nella parte delle code, si trova l'elenco delle code configurate in |parent_product| e in tempo reale viene mostrato per ogni coda il totale delle chiamate in attesa, in conversazione e il totale degli agenti connessi, in pausa, disconnessi, in conversazione nella coda, liberi e occupati in generale (anche per chiamate non inerenti alla coda).

La sezione dedicata agli agenti elenca gli agenti configurati su tutte le code e per ogni coda da lo stato del login, il tempo trascorso dall'ultima chiamata, il tempo della pausa se è in corso, il numero di chiamate prese e lo stato telefonico.

Riepilogo
---------

La scheda Riepilogo mostra i dati delle code del |parent_product| e gli agenti con dati inerenti alla giornata corrente, quindi i totali sono da riferire agli avvenimenti intercorsi dalle 00:00 della giornata corrente al momento della visualizzazione. Questa scheda viene aggiornata periodicamente tranne per lo stato degli agenti che è in tempo reale.

La prima parte dedicata alle code mostra le statistiche giornaliere delle code, elencando:

* chiamate entrate
* totale chiamate evase
* percentuale sul totale delle chiamate evase
* chiamate evase entro il tempo di livello di servizio indicato nella configurazione della coda nel |parent_product|
* percentuale sul totale delle chiamate evase entro il tempo di livello di servizio
* totale chiamate fallite, cioè chiamate entrate in coda, rimaste in attesa per più di 50 secondi ma non gestite per abbandono
* percentuale sul totale delle chiamate fallite
* chiamate nulle, cioè chiamate entrate in coda, rimaste in attesa per meno di 50 secondi ma non gestite per abbandono
* percentuale sul totale delle chiamate nulle
* tempo di attesa minimo di una chiamata in coda
* tempo di attesa massimo di una chiamata in coda
* tempo di attesa medio delle chiamate in coda
* durata minima di una chiamata risposta
* durata massima di una chiamata risposta
* durata media di una chiamata risposta

La seconda parte è dedicata agli agenti delle code, viene elencata ogni coppia agente-coda attiva nella giornata odierna mostrando i seguenti dati:

* nome agente
* stato del collegamento dell'agente alla coda: login (verde), pausa (giallo) e logout(grigio)
* stato telefonico dell'interno associato all'agente: libero (verde), occupato in una chiamata attinente alla coda (azzurro), occupato per una chiamata non attinente alla coda (rosso), disconnesso (grigio)
* nome coda
* totale chiamate prese
* totale chiamate non risposte
* durata minima delle chiamate risposte
* durata massima delle chiamate risposte
* durata media delle chiamate risposte
* totale del tempo trascorso al telefono
* tempo dell'ultimo login
* tempo dell'ultimo logout
* tempo dall'ultima chiamata risposta
* tempo totale di logon
* tempo totale di pausa
* percentuale del totale del tempo di pausa sul tempo di logon
* percentuale del tempo trascorso al telefono sul tempo di logon


Monitor
-------

Nella scheda Monitor vengono mostrate tutte le code attivate nelle loro schede riservate (opzione Visualizza in Monitor).
Per ogni coda viene mostrato l'elenco delle chiamate in attesa in base alla loro posizione, mostrando in nome/numero chiamante e il tempo di attesa.
La scheda Monitor nasce per essere utilizzata in un maxi schermo per mostrare agli agenti in tempo reale lo stato della coda.
