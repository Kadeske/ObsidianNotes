# 04 - Requisiti e Personas

> Fonte: [[IUM/TEORIA/PDF/04-personas.pdf]]

---

## 1. Cosa sono i requisiti

Nello **user-centered design (UCD)** i requisiti rappresentano le **aspettative, necessità e vincoli** degli utenti finali durante lo sviluppo di un prodotto o servizio.

Sono importanti perché:
- **orientano all'utente**: il design risponde davvero ai suoi bisogni;
- **riducono i rischi**: evitano malintesi e sviluppi sbagliati;
- sono **la base della progettazione**: guidano le decisioni di design e sviluppo.

### Requisiti tradizionali vs requisiti UCD

| | Tradizionali | User-centered |
|---|---|---|
| **Definizione** | Specifiche tecniche e funzionali | Aspettative e bisogni degli utenti finali |
| **Focus** | Caratteristiche del sistema, performance, architettura | Esperienza utente, usabilità, soddisfazione |
| **Esempi** | Capacità di carico, tempi di risposta, compatibilità | Facilità d'uso, accessibilità, modalità di interazione |

I due tipi si **integrano**: i requisiti UCD arricchiscono quelli tradizionali, mettono l'utente al centro, possono **evolvere** durante lo sviluppo (serve flessibilità) e puntano a un prodotto che non solo funzioni, ma sia **facile e piacevole** da usare.

### Perché definirli bene
La definizione dei requisiti è **la fase in cui i fallimenti sono più comuni**: ognuno capisce una cosa diversa (la celebre vignetta dell'altalena: come l'ha spiegata il cliente, come l'ha capita il project leader, come l'ha progettata l'analista… e cosa voleva davvero il cliente).

![[IUM04-altalena-requisiti.jpg|550]]

## 2. Tipi di requisiti

| Tipo | Riguarda | Esempi |
|---|---|---|
| **Funzionali** | Cosa il sistema deve **fare** (storicamente i più considerati) | Ricerca prodotti con filtri |
| **Non funzionali** | Vincoli di qualità e risorse | Memoria, tempi di risposta |
| **Dati** | Quali dati salvare e **come** (es. database) | Tipi di dato, persistenza |
| **Ambiente / contesto d'uso** | Dove e in che condizioni si usa il sistema | vedi sotto |

Il contesto d'uso ha tre dimensioni:
- **fisico**: polvere, rumore, vibrazioni, luce, caldo, umidità…;
- **sociale**: condivisione di file, display o carta, collaborazione a distanza, lavoro individuale, privacy dei clienti;
- **organizzativo**: gerarchia, supporto agli utenti, struttura e infrastruttura dell'organizzazione, disponibilità all'apprendimento.

### Come scrivere un requisito
Un requisito deve essere **chiaro, non ambiguo e specifico** (verificabile).

| ✅ Buono | ❌ Cattivo |
|---|---|
| «Il download di qualsiasi pagina del sito deve richiedere meno di 5 secondi» | «Le bambine sotto i 10 anni devono ritenere il sito attraente» |

Di solito si aggiungono campi descrittivi: **identificatore**, **tipo**, **chi lo ha proposto**, **motivazione**.

### Esempi (e-commerce)

| Categoria | Requisito |
|---|---|
| Funzionale | **Ricerca** per categoria, prezzo e valutazione · **Registrazione** via email o social in meno di 2 minuti |
| Usabilità | **Navigazione**: al massimo 3 clic per raggiungere qualsiasi pagina · **Feedback** immediato dopo ogni azione significativa |
| Accessibilità | Completamente navigabile con **screen reader** · **contrasto** sufficiente tra testo e sfondo |
| Performance | Caricamento in meno di **3 s** su connessione media · risposta all'input in meno di **200 ms** |
| Contesto | **Layout responsive** su mobile · **modalità offline** per i contenuti salvati |

## 3. Conoscere gli utenti

User-centered design significa **conoscere gli utenti**, ma è difficile decidere sulla base di un vago concetto di "utente": bisogna renderlo **concreto**.

### Caratteristiche da considerare
- **Chi sono**: abilità, conoscenze, dimestichezza con il computer.
- **Come usano il sistema**:

| Tipo di utente | Di cosa ha bisogno |
|---|---|
| **Inesperto** | Istruzioni passo-passo, interazione vincolata, informazioni chiare |
| **Esperto** | Flessibilità, accesso diretto alle funzioni, "potere" |
| **Frequente** | Scorciatoie (*shortcut*) |
| **Occasionale** | Istruzioni chiare (es. menu) |

- **Caratteristiche fisiche**: la dimensione delle mani influenza posizione e dimensione dei pulsanti; le abilità motorie l'uso delle periferiche; l'altezza il design di un chiosco; la forza (un giocattolo deve richiedere poca forza per l'uso ma molta per cambiare le batterie); le disabilità (vista, udito, destrezza).

### Persona, scenario, obiettivo

![[IUM04-persona-scenario-goal.png|450]]

1. **Persona** — *chi* è il protagonista della storia (atteggiamenti, motivazioni, obiettivi, problemi).
2. **Scenario** — *quando, dove e come* si svolge la storia: la narrazione delle azioni della persona (vedi [[05 - Scenari e modelli dei task]]).
3. **Obiettivo** (*goal*) — *cosa* la persona vuole o deve ottenere; quando lo raggiunge, lo scenario finisce.

## 4. Personas

> [!quote] Nielsen Norman Group
> *"A persona is a **fictional, yet realistic**, description of a typical or target user of the product. A persona is an **archetype** instead of an actual living human, but personas should be described as if they were real people."*

Caratteristiche:
- catturano le caratteristiche degli utenti, **sintetizzate da utenti reali** (non sono persone reali);
- **non** devono essere idealizzate;
- devono essere **"vive"**: un nome, caratteristiche, scopi, conoscenze, vita sociale;
- hanno un **insieme unico di obiettivi**, non solo un ruolo lavorativo (persone con lo stesso ruolo possono avere obiettivi diversi);
- di solito se ne sviluppano **diverse**.

Il dettaglio serve a uscire dal concetto generico di utente (**si ragiona meglio su istanze concrete che in astratto**); le caratteristiche scelte possono sintetizzare risultati statistici.

![[IUM04-esempio-persona.jpg|550]]

### Quando crearle
- **Il prima possibile** nel processo di design.
- Sono il risultato della **user research**: basate su **dati disponibili**, non su come vorremmo che gli utenti fossero.

### Cosa inserire
- **Nome, età, genere e una foto**.
- Un'**etichetta** che descriva cosa fa nella vita reale.
- **Livello di esperienza** nel dominio del prodotto.
- **Contesto d'uso**: lo usa per scelta o per lavoro? Quanto spesso? Da desktop, mobile o altro?
- **Obiettivi e problemi** nei compiti rilevanti (velocità, accuratezza, completezza…).
- Una **frase** che riassuma il suo atteggiamento.

> [!warning] Evitare troppe battute: la persona diventa divertente ma poco utile.

## 5. Come raccogliere le informazioni sugli utenti

| Tecnica | Punti di forza | Limiti |
|---|---|---|
| **Interviste** (anche con prototipi o scenari) | Ottime per indagare i problemi | Richiedono molto tempo; impossibile raggiungere molti utenti |
| **Focus group** (interviste di gruppo) | Visione consensuale, individuare aree di conflitto | Possono essere dominati da pochi individui |
| **Questionari** | Domande specifiche a molte persone; dati qualitativi o quantitativi | Spesso da affiancare ad altre tecniche |
| **Ricerca di prodotti simili** | Utile per individuare requisiti | — |
| **Osservazione diretta** | Capire natura e contesto dei task e il ruolo delle diverse parti | Richiede tempo e un membro del team; produce enormi quantità di dati |
| **Osservazione indiretta** (es. log) | Tenere traccia dei task correnti | Poco usata per i requisiti |
| **Studio della documentazione** (manuali, procedure, norme) | Passi di un'attività, leggi e regole, contesto; non richiede tempo agli utenti | Non si può usare da sola |

## 6. Esercizi

> [!question] Esercizio 1 — Analizzare una persona
> 1. Leggere il template <https://public-media.interaction-design.org/pdf/Persona-Template.pdf> e identificare le varie informazioni.
> 2. Discutere le possibili implicazioni sull'ideazione dell'interfaccia.

> [!question] Esercizio 2 — Scrivere una persona
> Per un'applicazione di **gestione dell'orario delle lezioni universitarie**, scrivere una persona che caratterizzi una categoria di utenti ragionevole.

> [!question] Esercizio 3 — Personas con ChatGPT
> Ripetere l'esercizio 2 con un prompt per ChatGPT che riassuma le informazioni necessarie. Confrontare il risultato con il proprio: quanto è plausibile la persona generata?

## Riferimenti
- Dix, Finlay, Abowd, Beale — *Interazione Uomo Macchina*, cap. 5
- Rogers, Sharp, Preece — *Interaction Design*, cap. 9–11
- Nielsen Norman Group — <https://www.nngroup.com/articles/persona/>
- Approfondimento: B. Buxton — *Sketching user experiences*

---

## Termini e concetti da ricordare

- **Requisiti UCD**: aspettative, necessità e vincoli degli utenti finali; integrano i requisiti tradizionali (tecnici).
- La definizione dei requisiti è **la fase con più fallimenti** (vignetta dell'altalena).
- **Tipi di requisiti**: funzionali · non funzionali · dati · ambiente (fisico, sociale, organizzativo).
- **Buon requisito**: chiaro, non ambiguo, specifico, verificabile (+ id, tipo, autore, motivazione).
- **Utenti** inesperti / esperti / frequenti / occasionali → bisogni diversi.
- **Persona**: archetipo fittizio ma realistico, basato su dati di user research, con nome, foto, obiettivi e problemi.
- **Persona → scenario → obiettivo** (*goal*).
- Le personas si creano **il prima possibile** e non vanno idealizzate.
- **Tecniche di raccolta**: interviste, focus group, questionari, prodotti simili, osservazione diretta e indiretta, documentazione.
