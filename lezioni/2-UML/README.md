# Esercitazione su requisiti e Casi d’Uso

<!-- New section -->

## Sommario

I **casi d'uso** (_use case_) rappresentano uno strumento di supporto nella
fase di identificazione dei **requisiti funzionali** di un sistema

- Si basano sulla descrizione delle interazioni tipiche tra il sistema stesso, e utenti o sistemi esterni (**attori**)

- Un caso d’uso rappresenta il **valore** che deve essere dato all’utente

- aiuta a comprendere il funzionamento del sistema e ne mette in evidenza eventuali carenze

<!-- New subsection -->

Un **caso d'uso** ha

- _nome_ 

- _descrizione_ (spesso informale) della sequenza di eventi che avvengono tra gli oggetti durante l'elaborazione (anche degli attori esterni al sistema).

Possono contenere diagrammi esplicativi, ed altre informazioni che iutano l’interazione con il cliente

Includono uno o più **scenari**

<!-- New subsection -->

## Scenario

_sequenza dei passi_ che caratterizzano una particolare interazione tra uno o più attori e il sistema

- Viene usato nella raccolta dei requisti per l'elicitazione (identificazione, analisi, valutazione), la validazione e la documentazione dei requisiti

- Scritto in linguaggio naturale (terminologia del cliente)

Un insieme completo di scenari descrive tutto ciò che il sistema deve fare

<!-- New subsection -->

## Passi da seguire per la descrizione di un **caso d'uso**:

- Individuare lo **scenario principale**, esso caratterizza un andamento previsto/desiderato
- Descriverlo utilizzando una **sequenza di passi numerati**
- Individuare gli **scenari alternativi** che comprendono gli andamenti imprevisti e talvolta errati rispetto allo scenario principale
- Individuare gli **attori primari e secondari** ricordando che: ogni caso d'uso ha un **attore principale**

<!-- New section -->

## Diagramma dei casi d'uso

Fornisce un'istantanea dei casi d'uso e degli attori che caratterizzano il sistema in oggetto

- Individua **chi** o che cosa ha a che fare con il sistema (**attore**) e **che cosa** viene fatto (**caso d'uso**)
- Rappresenta la soluzione grafica UML per rappresentare i casi d'uso
- Il caso d'uso è rappresentato da un **ovale** con all'interno un "nome" significativo che ricorda la funzionalità rappresentata, per esempio `login`, `logout`...

<img src="img/casoduso.png" alt="drawing" height="140"/>

<!-- New subsection -->

### Attore

Chiunque o qualsiasi _sistema esterno_ che interagisce con il sistema è un **attore** del sistema

- In caso di utenti, è un ruolo interpretato da un utente in relazione al sistema

- Graficamente è rappresentato da uno **stickman**

<img src="img/stickman.png" alt="drawing" height="200"/>

<!-- New subsection -->

- **Attori primari**: fanno partire il caso d'uso e ne sono i "beneficiari"

- **Attori secondari**: sono coinvolti nel caso d'uso, fornendo informazioni e/o ricevendone dal sistema
Ogni caso d'uso deve avere **uno** e **un solo** attore primario

Un **attore** può essere **primario** in alcuni casi d'uso e **secondario** in altri

<img src="img/attori.png" alt="drawing" height="250"/>


<!-- New subsection -->

## Confini del sistema

- Identificare il confine del sistema significa comprendere cosa è **parte del sistema** (sta dentro i confini) e cosa **no** (sta fuori dai confini)

- Il confine del sistema viene rappresentato con un **rettangolo** che racchiude i Casi d'uso

<img src="img/confine.png" alt="drawing" height="250"/>

<!-- New subsection -->

Le domande che è necessario porsi prima di disegnare il diagramma sono:

- Chi **usa** il sistema?
- Chi **fa partire** il sistema?
- Chi **amministra** il sistema?
- Chi fa lo **shut-down** del sistema?
- Quali altri sistemi **usano** il sistema?
- Chi **raccoglie** informazioni dal sistema?
- Chi **fornisce** informazioni al sistema?

<!-- New section -->

## Relazioni tra casi d'uso

### `<<include>>`

Si usa quando casi d'uso diversi hanno in comune una sequenza di passi da svolgere

- Si evidenziano **parti comuni**

- Si tratta di una **dipendenza** tra casi d'uso: il caso incluso fa parte del comportamento di quello che lo include

- Si evitano le **ripetizioni** nelle descrizioni dei casi d'uso

<!-- New subsection -->

Notazione: relazione di dipendenza (freccia tratteggiata) con stereotipo `<<include>>`

<img src="img/include.png" alt="drawing" height="250"/>

> Nell'**include** la freccia va dal caso d'uso che include **verso** quello incluso.


<!-- New subsection -->

### `<<extend>>`

Si usa quando un caso d'uso riassume scenari diversi con uno scenario principale e alcune possibili varianti che avvengono solo sotto particolari condizioni

- Si tratta di una **dipendenza** tra casi d'uso: il caso d'uso che estende specifica un incremento di comportamento a quello esteso

- Si tratta di comportamento **supplementare ed opzionale** che gestisce una variazione rispetto al comportamento normale

<!-- New subsection -->

Notazione: relazione di dipendenza (freccia tratteggiata) con stereotipo `<<extend>>`

<img src="img/extend.png" alt="drawing" height="250"/>

> Nell'**extend** la freccia va dal caso d'uso che estende **verso** quello esteso.

<!-- New section -->

## Esercizio 1

Si supponga di dover sviluppare un'applicazione web per la gestione della bacheca scolastica.

- Al sistema accedono due tipi di utenti tramite login. Le credenziali sono fornite dalla scuola, quindi gli utenti non hanno bisogno di registrarsi.

<!-- .element: class="fragment" -->

- Un professore può pubblicare il materiale delle lezioni, i compiti assegnati e gli avvisi relativi agli esami. Può inoltre inviare messaggi agli studenti. 

<!-- .element: class="fragment" -->

- Uno studente può visualizzare tutto il contenuto pubblicato sulla bacheca e inviare messaggi al professore.

<!-- .element: class="fragment" -->

<!-- New subsection -->

- Disegnare il diagramma dei casi d'uso.
- Documentare il caso d'uso "Pubblica compiti".

<!-- New subsection -->

<img src="img/Esercizio1.png" alt="drawing" height="700"/>


<!-- New subsection -->

## CdU "Pubblica compiti"

- **Descrizione**: Il professore pubblica sulla bacheca i compiti assegnati agli studenti, indicando la descrizione e la data di consegna.

- **Attori primari**: Professore

- **Attori secondari**: nessuno

- **Precondizioni**: il professore deve aver effettuato il login.

<!-- New subsection -->

**Sequenza principale**:

1. Il professore seleziona "Pubblica compiti" dalla bacheca.
2. Il sistema mostra il form di inserimento con i campi: descrizione del compito e data di consegna.
3. Il professore compila i campi e conferma la pubblicazione.
4. Il sistema valida i dati inseriti.
5. Il sistema pubblica il compito sulla bacheca e lo rende visibile a tutti gli studenti.
6. Il sistema mostra un messaggio di conferma al professore.

<!-- New subsection -->

**Postcondizioni** : il compito è visibile sulla bacheca di tutti gli studenti del corso.

<!-- New section -->

## Esercizio 2

Si supponga di dover sviluppare un sistema software per la gestione di una biblioteca pubblica. 

- Il sistema permette a un **utente** di _registrarsi_ fornendo nome, cognome, codice fiscale ed email, ed effettuare il _login_. 

<!-- .element: class="fragment" -->

- Una volta autenticato, l'utente può _cercare un libro_ e _prenotare un libro_ disponibile.

<!-- .element: class="fragment" -->

- Il **bibliotecario** gestisce il catalogo (aggiunta, modifica e rimozione di libri).

<!-- .element: class="fragment" -->

- Nel caso di libri restituiti in ritardo, il sistema invia automaticamente una notifica via email all'utente moroso tramite un sistema di posta elettronica esterno (**Mail Server**).

<!-- .element: class="fragment" -->

<!-- New subsection -->

- Disegnare il diagramma dei casi d'uso.
- Documentare il caso d'uso "Prenota libro".

<!-- New subsection -->

<img src="img/Esercizio2.png" alt="drawing" height="700"/>


<!-- New subsection -->

## CdU "Prenota libro"

- **Descrizione**: L'utente cerca un libro disponibile e lo prenota per il ritiro in biblioteca.

- **Attori primari**: Utente

- **Attori secondari**: nessuno

- **Precondizioni**: l'utente deve aver effettuato il login; il libro cercato deve essere disponibile

<!-- New subsection -->

**Sequenza principale**:

1. L'utente seleziona l'opzione "Cerca libro" e inserisce i criteri (titolo, autore o ISBN)
2. Il sistema mostra i risultati corrispondenti
3. L'utente seleziona il libro desiderato
4. Il sistema verifica la disponibilità e mostra i dettagli del libro
5. L'utente seleziona "Prenota"
6. Il sistema registra la prenotazione associandola all'utente e mostra una conferma con la data entro cui ritirare il libro

**Postcondizioni** : la prenotazione è registrata nel sistema; la copia è riservata all'utente fino alla data di scadenza

<!-- New subsection -->

**Sequenza alternativa**:

- 4.A Il libro risulta non disponibile (tutte le copie sono in prestito)
  - 4.1.A Il sistema informa l'utente dell'indisponibilità
  - 4.2.A Il sistema annulla la prenotazione

**Postcondizioni** : nessuna

<!-- New section -->

## Esercizio 3

Si vuole realizzare un sistema software per il tracciamento di un caddozzone itinerante. 

- L' utente può consultare il menù del food truck, visualizzare la sua posizione attuale sulla mappa, effettuare un ordine e pagarlo online. Il pagamento online può essere effettuato anche tramite PayPal.

<!-- .element: class="fragment" -->

- Un rilevatore GPS aggiorna automaticamente la posizione del camion ogni minuto.

<!-- .element: class="fragment" -->

- Un amministratore, tramite un pannello di controllo dedicato, è in grado di: aggiornare il menù; gestire gli ordini; verificare che la posizione mostrata agli utenti sia corretta e modificarla manualmente in caso di malfunzionamento del GPS.

<!-- .element: class="fragment" -->

<!-- New subsection -->

- Disegnare il diagramma dei casi d'uso.
- Documentare il caso d'uso "Ordina e paga".

<!-- New subsection -->

<img src="img/Esercizio3.png" alt="drawing" height="700"/>

<!-- New subsection -->

## CdU "Ordina e paga"

- **Descrizione**: L'utente consulta il menù, effettua un ordine, paga online e segue la posizione del camion per sapere dove ritirare il proprio ordine.

- **Attori primari**: Utente

- **Attori secondari**: PayPal (solo nella sequenza alternativa)

- **Precondizioni**: il caddozzone deve trovarsi nella posizione indicata

<!-- New subsection -->

**Sequenza principale**:

1. L'utente seleziona "Consulta menù" e visualizza i prodotti disponibili
2. L'utente seleziona i prodotti desiderati e conferma l'ordine
3. Il sistema mostra il riepilogo dell'ordine e il totale da pagare
4. L'utente seleziona "Pagamento online" e inserisce i dati della carta di credito
5. Il sistema verifica i dati e addebita l'importo
6. Il sistema conferma l'avvenuto pagamento e registra l'ordine
7. L'utente seleziona "Visualizza posizione" e visualizza sulla mappa la posizione attuale del camion per poter ritirare l'ordine

<!-- New subsection -->


**Sequenza alternativa**:
- 4.A L'utente seleziona "Pagamento con PayPal" invece del pagamento con carta
  - 4.1.A Il sistema reindirizza l'utente alla piattaforma PayPal
  - 4.2.A L'utente si autentica su PayPal e autorizza il pagamento
  - 4.3.A PayPal comunica al sistema l'avvenuto pagamento

<!-- New subsection -->


**Postcondizioni**: l'ordine è registrato nel sistema e il pagamento è stato completato; 




