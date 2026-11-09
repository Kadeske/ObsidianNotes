# 08 - Il processo di lettura

> Fonte: [[IUM/TEORIA/PDF/08-lettura.pdf]]
> Riferimenti: Gamberini, Chittaro, Paternò — *Human-Computer Interaction*, cap. 3 · Dix et al. — *Interazione uomo-macchina*, cap. 1 (sezione Lettura)

---

## 1. Come leggiamo

Il processo di lettura ha tre stadi:
1. **Percezione** del pattern visivo;
2. **Decodifica**, usando la rappresentazione interna del linguaggio;
3. **Interpretazione**, usando la conoscenza di sintassi, semantica e pragmatica.

### Movimenti saccadici e fissazioni
L'occhio non scorre il testo in modo continuo: alterna **movimenti saccadici** (salti rapidi) e **fissazioni**. La **percezione avviene durante le fissazioni**, che occupano il **94% del tempo**.

![[IUM08-saccadi.jpg|450]]

### Modalità di lettura
- Velocità media di circa **250 parole al minuto**.
- Le parole **non** vengono lette carattere per carattere: quelle **familiari** si riconoscono dalla **forma**.
- Il **maiuscolo** ha una forma meno variabile e spesso costringe a leggere lettera per lettera (**rallenta**).
- Il riconoscimento della forma **dipende dal contesto**.
- La massima velocità (sperimentale) si ottiene con:
  - font da **9 a 12 punti** (circa 12–14 px su schermo);
  - righe lunghe tra **58 e 132 mm**.
- Scritte scure su sfondo chiaro potrebbero migliorare la lettura a schermo, ma favoriscono il *flickering* (sfarfallio).

In aula si svolgono due **esperimenti di riconoscimento** con brevi elenchi di parole (*Pangolino, Albatros, Piccione* e *SEDANO, Peperone, Wasabi*).

### Il ruolo del contesto
Leggiamo in modo diverso a seconda del contesto: lo stesso simbolo viene letto come **B** tra le lettere e come **13** tra i numeri.

![[IUM08-contesto-B13.png|320]]

Ciò che ci aspettiamo può anche **ingannarci**: nella frase *"La veloce volpe marrone salta sopra il il pigro cane"* quasi nessuno nota il **"il" ripetuto**.

### Ricostruzione semantica
Riusciamo a leggere anche parole con le lettere interne rimescolate, purché **prima e ultima lettera** siano al loro posto: il cervello ricostruisce il significato.

![[IUM08-ricostruzione-semantica.jpg|300]]

## 2. I font

| Tipo | Caratteristiche | Esempio |
|---|---|---|
| **Con grazie** (*serif*), spaziatura proporzionale | Le lettere hanno piccole "aggiunte" decorative alle estremità dei tratti | Times, Palatino, New York |
| **Senza grazie** (*sans-serif*), spaziatura proporzionale | Tratti che terminano squadrati | Helvetica |
| **Monospaziato**, spaziatura fissa | Ogni carattere ha la stessa larghezza | Courier |

- **Spaziatura fissa** vs **variabile** (in un font proporzionale la "i" è molto più stretta della "m").
- La dimensione si misura in **punti** (pt): 1 punto digitale = **1/72 di pollice** ≈ **0,35 mm**.
- Ogni font ha varianti: normale, **neretto**, *corsivo*, sottolineato.

> [!note] Le slide chiamano i serif "gothic" e i sans-serif "roman"; nella terminologia tipografica corrente è il contrario (*roman* indica di solito i caratteri con grazie, *gothic* quelli senza).

### Leggibilità
La leggibilità non è scontata: i manoscritti miniati medievali sono splendidi ma difficilissimi da leggere.

### Lettura su carta e a schermo
- Il testo a schermo (**72–300 dpi**) ha qualità molto inferiore a quello stampato (**1200–2400 dpi**).
- Alcuni caratteri sono resi male a causa dello **jagging** (scalettatura dovuta ai pixel).
- Uno studio del 1987 mostrava che leggere a schermo è circa il **25% più lento** che su carta, perché le fissazioni per riga sono il 15% in più.

### Con o senza grazie?
- **Su carta**, i caratteri **con grazie** sono leggermente più leggibili: meglio per i **testi lunghi**; quelli **senza grazie** sono più leggibili in **testi brevi e titoli**.
- **A schermo**, si raccomanda di solito il **sans-serif** (le grazie, piccole, sono rese male dai pixel).

### Maiuscole o minuscole?
Le minuscole danno a ogni parola un **profilo riconoscibile** (ascendenti e discendenti), mentre il tutto maiuscolo produce blocchi rettangolari tutti uguali. La leggibilità dipende soprattutto dalla **parte superiore** delle parole.

![[IUM08-maiuscole-minuscole.png|420]]

- Un testo **TUTTO IN MAIUSCOLO** è **meno leggibile** di uno in maiuscole e minuscole (verificabile sperimentalmente).
- Le maiuscole vanno bene per **lettere singole o sigle** (es. il volo `AZ7815` invece di `az7815`).

## 3. Compendio di tipografia digitale

**Impaginazione**
- **Blocchi** uniformi e regolari favoriscono la leggibilità.
- **Bilanciare pieni e vuoti**: evitare il *wall of text* (muro di testo).
- **Allineamento**: sul web di solito **a sinistra**, su carta **giustificato** (salvo esigenze particolari).
- Evitare:
  - **vedove**: una parola isolata su una riga;
  - **orfani**: una parola isolata all'inizio di una nuova colonna o pagina;
  - **fiumi**: spazi troppo larghi tra le parole nel testo giustificato.

**Font e stili**
- Il font del corpo deve **facilitare la lettura**; il serif è spesso usato per titoli e sottotitoli.
- Un font **distintivo** solo per marchi o titoli che devono attirare l'attenzione.
- **Non** pubblicare parole come immagini.
- **Grassetto**: per sostantivi o aggettivi importanti, su porzioni molto brevi. *Test della pellicola*: togliendo tutto tranne il grassetto, si dovrebbe capire il senso.
- **Corsivo**: per enfasi, citazioni, titoli di opere, termini tecnici; da evitare nei testi lunghi.
- **Sottolineato**: significa "clicca qui" → solo per i link.

## 4. Principi della Gestalt per il raggruppamento

| Principio | Percepiamo come gruppo… |
|---|---|
| **Prossimità** | oggetti **vicini** |
| **Similarità** | oggetti di **forma simile** |
| **Regione comune** (raggruppamento) | oggetti racchiusi da un **bordo** |
| **Chiusura** | strutture **aperte** completate come chiuse, se la loro regolarità lo consente |
| **Continuità** | oggetti **allineati** o che sembrano proseguire uno nell'altro |

![[IUM08-gestalt.png|180]]

## 5. Lettura sul web

### Il modello a F (*F-shape*)
Sul web gli utenti **non leggono tutto** (vanno di fretta e hanno poco interesse). Se l'attenzione non viene guidata, lo sguardo traccia una **F**:
1. un movimento **orizzontale** nella parte alta del contenuto;
2. un secondo movimento orizzontale, più corto, un po' più in basso;
3. una scansione **verticale** del lato sinistro.

Il modello è confermato anche sui dispositivi mobili.

![[IUM08-f-shape.jpg|260]]

Nelle pagine dei risultati di Google, la stessa concentrazione dello sguardo in alto a sinistra è nota come **triangolo d'oro** (*golden triangle*):

![[IUM08-golden-triangle.jpg|320]]

### Come contrastare l'F-shape
1. Mettere il contenuto più importante **nei primi due paragrafi**.
2. Usare **titoli** per sezioni e sottosezioni…
3. …con **parole rilevanti**.
4. **Raggruppare** i contenuti.
5. Usare il **grassetto** per le parti importanti.
6. Evidenziare i **link** con parole che **anticipano il contenuto** (niente "clicca qui", "vai", "altro").
7. Usare **elenchi** puntati e numerati per liste e procedure.
8. **Tagliare** ciò che non serve.

**Esempi**: una pagina istituzionale con un lungo *wall of text* a confronto con condizioni d'uso suddivise in sezioni con indice; la homepage del *New York Times* come esempio positivo di gerarchia tipografica.

## 6. In sintesi
- La leggibilità a schermo dipende molto dal **tipo e dalla dimensione** del font.
- A schermo si raccomandano font **sans-serif**, evitando il più possibile il corsivo.
- La lettura a schermo è faticosa: evitare i **testi lunghi**.
- **Mai** testi lunghi tutti in maiuscolo.
- Usare **pochi font**, in modo **consistente**, per veicolare informazione.
- **Guidare la lettura** sul web con la tipografia.

**Approfondimenti**: Pasquini, Giomi — *Web Usability*, cap. 6 · <http://www.markboulton.co.uk/journal/five-simple-steps-to-better-typography> · <http://webtypography.net/toc/>

---

## Termini e concetti da ricordare

- **Stadi della lettura**: percezione → decodifica → interpretazione.
- **Saccadi** e **fissazioni**: si percepisce durante le fissazioni (94% del tempo).
- ~**250 parole/minuto**; parole familiari riconosciute dalla **forma**.
- Velocità massima con font da **9–12 pt** e righe da **58–132 mm**.
- **Contesto** (B / 13) e **ricostruzione semantica** (prima e ultima lettera al loro posto).
- **Serif / sans-serif / monospaziato**; **1 pt = 1/72 di pollice ≈ 0,35 mm**.
- Schermo (72–300 dpi) vs stampa (1200–2400 dpi); **jagging**; a schermo meglio il **sans-serif**.
- **Maiuscolo** meno leggibile (niente profilo della parola); va bene per sigle.
- **Tipografia**: evitare *wall of text*, **vedove**, **orfani**, **fiumi**; grassetto con parsimonia (test della pellicola); sottolineato = link.
- **Gestalt**: prossimità, similarità, regione comune, chiusura, continuità.
- **F-shape** e **golden triangle**: le cose importanti vanno in alto e a sinistra; titoli, grassetto, elenchi, link descrittivi.
