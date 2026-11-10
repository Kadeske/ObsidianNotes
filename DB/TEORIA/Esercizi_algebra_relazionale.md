# Esercizi di algebra relazionale

> Esercizi basati sulle tabelle di [[Dispense_algebra_relazionale#0. Base di dati di esempio|Dispense di algebra relazionale]]. Prima ci sono i testi di tutti gli esercizi, poi le [[#Soluzioni|soluzioni]]. Per ogni esercizio scrivi l'**espressione algebrica** e calcola il **risultato** sull'istanza data.

> [!abstract]- Promemoria delle tabelle (clicca per aprire)
> - STUDENTI(<u>Matricola</u>, Nome, Città, Anno)
> - DOCENTI(<u>IdDoc</u>, Nome, Città)
> - CORSI(<u>Codice</u>, Titolo, CFU, Docente) — FK `Docente → DOCENTI.IdDoc`
> - ESAMI(<u>Studente, Corso</u>, Voto) — FK `Studente → STUDENTI.Matricola`, `Corso → CORSI.Codice`
> - TUTOR(<u>Matricola</u>, Nome, Città, Anno)
> - PROPEDEUTICITÀ(<u>Corso, Richiede</u>)
>
> | Matricola | Nome | Città | Anno |
> |---|---|---|---|
> | 101 | Anna | Cagliari | 1 |
> | 102 | Bruno | Sassari | 2 |
> | 103 | Carla | Cagliari | 2 |
> | 104 | Dario | Oristano | NULL |
> | 105 | Elena | Nuoro | 1 |
>
> | IdDoc | Nome | Città |
> |---|---|---|
> | D1 | Rossi | Cagliari |
> | D2 | Bianchi | Sassari |
> | D3 | Verdi | Iglesias |
>
> | Codice | Titolo | CFU | Docente |
> |---|---|---|---|
> | BD | Basi di Dati | 9 | D1 |
> | IUM | Interazione Uomo Macchina | 6 | D2 |
> | ALG | Algoritmi | 9 | D1 |
> | RET | Reti | 6 | D2 |
>
> | Studente | Corso | Voto |
> |---|---|---|
> | 101 | BD | 28 |
> | 101 | IUM | 30 |
> | 102 | BD | 24 |
> | 103 | ALG | 30 |
> | 103 | BD | 27 |
> | 104 | IUM | 22 |
>
> | Matricola (TUTOR) | Nome | Città | Anno |
> |---|---|---|---|
> | 102 | Bruno | Sassari | 2 |
> | 103 | Carla | Cagliari | 2 |
> | 201 | Fabio | Cagliari | 4 |

---

## Testi

Gli esercizi sono in ordine di difficoltà crescente.

### Esercizio 1 — Selezione e valori nulli ★
Elencare i **nomi** degli studenti che **non sono al primo anno**, includendo anche quelli il cui anno **non è noto**.

### Esercizio 2 — Join semplice ★
Elencare i **nomi** degli studenti che hanno preso **30** in almeno un esame.

### Esercizio 3 — Join con selezione sull'altra tabella ★
Elencare i **titoli** dei corsi tenuti dal docente di nome **Rossi**.

### Esercizio 4 — Operatori insiemistici ★★
a) Elencare le **città** in cui vive almeno uno studente ma **nessun tutor**.
b) Elencare le **matricole** dei tutor che hanno sostenuto **almeno un esame**.

### Esercizio 5 — Differenza e ricostruzione degli attributi ★★
Elencare i **nomi** dei docenti che **non tengono alcun corso**.

### Esercizio 6 — Pattern "nessuno" ★★
Elencare **matricola e nome** degli studenti che **non hanno sostenuto nessun esame** di un corso tenuto dal docente **Bianchi**.

### Esercizio 7 — Join esterno su tre tabelle ★★
Per **ogni corso**, elencare il **titolo**, il **nome del docente** e le **matricole** degli studenti che lo hanno superato, una per riga. Devono comparire **anche i corsi che nessuno ha superato**.

### Esercizio 8 — Self-join ★★★
Elencare le **coppie di matricole** di studenti che hanno sostenuto **almeno un esame dello stesso corso**. Ogni coppia deve comparire **una sola volta** (non sia (101, 102) sia (102, 101)) e uno studente non deve essere abbinato a sé stesso.

### Esercizio 9 — Minimo senza aggregati ★★★
Elencare il **nome** dello studente che ha preso il **voto più basso** nell'esame di **Basi di Dati** (codice `BD`).

### Esercizio 10 — "Per ogni" (divisione) ★★★
Elencare i **nomi** degli studenti che hanno sostenuto **tutti** i corsi sostenuti da **Dario** (matricola 104), **escluso Dario** stesso.

---

## Soluzioni

### Soluzione 1
$$\pi_{\text{Nome}}\big(\sigma_{\text{Anno} \ge 2 \;\vee\; \text{Anno IS NULL}}(\text{STUDENTI})\big)$$

| Nome |
|---|
| Bruno |
| Carla |
| Dario |

> [!warning] Trappola
> $\sigma_{\text{Anno} \neq 1}(\text{STUDENTI})$ oppure $\sigma_{\text{Anno} \ge 2}(\text{STUDENTI})$ **escludono Dario**, perché un confronto con `NULL` non è mai vero. Il caso nullo va nominato esplicitamente con `IS NULL`.

### Soluzione 2
$$\pi_{\text{Nome}}\big(\text{STUDENTI} \bowtie_{\text{Matricola}=\text{Studente}} \sigma_{\text{Voto}=30}(\text{ESAMI})\big)$$

Gli esami con 30 sono 101–IUM e 103–ALG.

| Nome |
|---|
| Anna |
| Carla |

> La selezione si applica **prima** del join (*push selections down*): il risultato è lo stesso, ma il join lavora su 2 tuple invece che su 6.

### Soluzione 3
$$\pi_{\text{Titolo}}\big(\text{CORSI} \bowtie_{\text{Docente}=\text{IdDoc}} \sigma_{\text{Nome}=\text{'Rossi'}}(\text{DOCENTI})\big)$$

| Titolo |
|---|
| Basi di Dati |
| Algoritmi |

> La condizione è sul **nome** del docente, che sta in DOCENTI: serve il join, non basta $\sigma_{\text{Docente}=\text{'D1'}}(\text{CORSI})$ (che funzionerebbe solo sapendo già che Rossi ha codice D1).

### Soluzione 4
**a)**
$$\pi_{\text{Città}}(\text{STUDENTI}) - \pi_{\text{Città}}(\text{TUTOR})$$
$\{\text{Cagliari}, \text{Sassari}, \text{Oristano}, \text{Nuoro}\} - \{\text{Sassari}, \text{Cagliari}\}$:

| Città |
|---|
| Oristano |
| Nuoro |

**b)**
$$\pi_{\text{Matricola}}(\text{TUTOR}) \cap \rho_{\text{Matricola} \leftarrow \text{Studente}}\big(\pi_{\text{Studente}}(\text{ESAMI})\big)$$
$\{102, 103, 201\} \cap \{101, 102, 103, 104\}$:

| Matricola |
|---|
| 102 |
| 103 |

> Nella b) la ridenominazione è **obbligatoria**: l'intersezione richiede lo stesso schema, e gli attributi si chiamano `Matricola` e `Studente`. In alternativa: $\pi_{\text{Matricola}}(\text{TUTOR} \bowtie_{\text{Matricola}=\text{Studente}} \text{ESAMI})$.

### Soluzione 5
$$\pi_{\text{Nome}}\Big(\text{DOCENTI} \bowtie \big(\pi_{\text{IdDoc}}(\text{DOCENTI}) - \rho_{\text{IdDoc} \leftarrow \text{Docente}}(\pi_{\text{Docente}}(\text{CORSI}))\big)\Big)$$

1. Tutti i docenti: {D1, D2, D3}.
2. Docenti che tengono almeno un corso: {D1, D2}.
3. Differenza: {D3}.
4. Join naturale con DOCENTI (sull'attributo comune `IdDoc`) per recuperare il nome.

| Nome |
|---|
| Verdi |

> La differenza va fatta sulle **chiavi** (`IdDoc`), non sui nomi: due docenti diversi potrebbero chiamarsi allo stesso modo. Dopo la differenza si **torna in join** con la tabella di partenza per recuperare gli altri attributi.

### Soluzione 6
Prima gli studenti che **hanno** sostenuto un esame di Bianchi:
$$\text{CONB} = \rho_{\text{Matricola} \leftarrow \text{Studente}}\Big(\pi_{\text{Studente}}\big(\sigma_{\text{Nome}=\text{'Bianchi'}}(\text{DOCENTI}) \bowtie_{\text{IdDoc}=\text{Docente}} \text{CORSI} \bowtie_{\text{Codice}=\text{Corso}} \text{ESAMI}\big)\Big)$$

Poi tutti gli studenti meno questi, e il join per il nome:
$$\pi_{\text{Matricola},\text{Nome}}\big(\text{STUDENTI} \bowtie (\pi_{\text{Matricola}}(\text{STUDENTI}) - \text{CONB})\big)$$

I corsi di Bianchi sono IUM e RET; gli esami in questi corsi sono 101–IUM e 104–IUM, quindi CONB = {101, 104}.

| Matricola | Nome |
|---|---|
| 102 | Bruno |
| 103 | Carla |
| 105 | Elena |

> [!warning] Trappole
> - Elena **deve** comparire: non ha esami, quindi non ne ha sostenuti con Bianchi. Partendo da $\pi_{\text{Studente}}(\text{ESAMI})$ invece che da STUDENTI la si perderebbe.
> - $\pi(\sigma_{\text{Nome} \neq \text{'Bianchi'}}(\dots))$ è **sbagliata**: troverebbe chi ha sostenuto *almeno un esame con un altro docente* (ad esempio Anna, che ha anche BD con Rossi), non chi non ne ha *nessuno* con Bianchi.

**Albero dell'espressione**
```
                        π Matricola,Nome
                               │
                               ⋈ (naturale su Matricola)
                 ┌─────────────┴─────────────┐
             STUDENTI                         −
                                ┌─────────────┴─────────────┐
                         π Matricola              ρ Matricola ← Studente
                               │                            │
                           STUDENTI                    π Studente
                                                            │
                                                    ⋈ Codice=Corso
                                               ┌────────────┴───────────┐
                                        ⋈ IdDoc=Docente               ESAMI
                                       ┌───────┴───────┐
                               σ Nome='Bianchi'      CORSI
                                       │
                                    DOCENTI
```

### Soluzione 7
$$\pi_{\text{Titolo},\text{Nome},\text{Studente}}\big((\text{CORSI} \bowtie_{\text{Docente}=\text{IdDoc}} \text{DOCENTI}) ⟕_{\text{Codice}=\text{Corso}} \text{ESAMI}\big)$$

| Titolo | Nome | Studente |
|---|---|---|
| Basi di Dati | Rossi | 101 |
| Basi di Dati | Rossi | 102 |
| Basi di Dati | Rossi | 103 |
| Interazione Uomo Macchina | Bianchi | 101 |
| Interazione Uomo Macchina | Bianchi | 104 |
| Algoritmi | Rossi | 103 |
| Reti | Bianchi | NULL |

> - Il join CORSI–DOCENTI è **interno**: ogni corso ha un docente, quindi non si perde nessun corso (lo garantisce la chiave esterna).
> - Il join con ESAMI è **esterno sinistro**: è quello che conserva *Reti*, che non ha esami. Con un join interno Reti sparirebbe.
> - Un join **destro** sarebbe sbagliato: conserverebbe gli esami senza corso, che non esistono.

### Soluzione 8
Con due copie ridenominate di ESAMI:
- $\text{E1} = \rho_{S1,C1,V1 \leftarrow \text{Studente},\text{Corso},\text{Voto}}(\text{ESAMI})$
- $\text{E2} = \rho_{S2,C2,V2 \leftarrow \text{Studente},\text{Corso},\text{Voto}}(\text{ESAMI})$

$$\pi_{S1,S2}\big(\sigma_{C1=C2 \;\wedge\; S1 < S2}(\text{E1} \times \text{E2})\big)$$

(equivalente: $\pi_{S1,S2}(\text{E1} \bowtie_{C1=C2 \,\wedge\, S1<S2} \text{E2})$)

- BD è stato sostenuto da 101, 102, 103 → (101, 102), (101, 103), (102, 103)
- IUM da 101, 104 → (101, 104)
- ALG solo da 103 → nessuna coppia

| S1 | S2 |
|---|---|
| 101 | 102 |
| 101 | 103 |
| 102 | 103 |
| 101 | 104 |

> La condizione $S1 < S2$ fa **due cose insieme**: esclude le coppie di uno studente con sé stesso e tiene **una sola** delle due coppie simmetriche. Con $S1 \neq S2$ ogni coppia uscirebbe due volte.

### Soluzione 9
Voti di BD (chiamiamo questa relazione VBD):
$$\text{VBD} = \pi_{\text{Voto}}(\sigma_{\text{Corso}=\text{'BD'}}(\text{ESAMI})) = \{28, 24, 27\}$$

Il minimo si ottiene togliendo i voti **maggiori di almeno un altro** voto:
$$\text{MINBD} = \text{VBD} - \rho_{\text{Voto} \leftarrow V1}\Big(\pi_{V1}\big(\sigma_{V1 > V2}(\rho_{V1 \leftarrow \text{Voto}}(\text{VBD}) \times \rho_{V2 \leftarrow \text{Voto}}(\text{VBD}))\big)\Big)$$

I voti maggiori di un altro sono {28, 27}, quindi MINBD = {24}.

Infine si torna agli esami di BD con quel voto e allo studente:
$$\pi_{\text{Nome}}\Big(\text{STUDENTI} \bowtie_{\text{Matricola}=\text{Studente}} \big(\sigma_{\text{Corso}=\text{'BD'}}(\text{ESAMI}) \bowtie \text{MINBD}\big)\Big)$$

| Nome |
|---|
| Bruno |

> - Il join naturale con MINBD avviene sull'attributo `Voto`, ed è per questo che MINBD viene ridenominato in `Voto` alla fine.
> - La selezione $\text{Corso}=\text{'BD'}$ va ripetuta nell'ultimo passaggio: senza, il voto 24 abbinerebbe anche eventuali esami di altri corsi con lo stesso voto.

### Soluzione 10
1. Corsi sostenuti da Dario:
$$\text{CD} = \pi_{\text{Corso}}(\sigma_{\text{Studente}=104}(\text{ESAMI})) = \{\text{IUM}\}$$
2. Tutte le coppie (studente, corso di Dario):
$$\text{TUTTE} = \rho_{\text{Studente} \leftarrow \text{Matricola}}(\pi_{\text{Matricola}}(\text{STUDENTI})) \times \text{CD}$$
→ (101, IUM), (102, IUM), (103, IUM), (104, IUM), (105, IUM)
3. Le coppie **mancanti** (non sostenute):
$$\text{MANCANTI} = \text{TUTTE} - \pi_{\text{Studente},\text{Corso}}(\text{ESAMI})$$
→ (102, IUM), (103, IUM), (105, IUM)
4. Studenti a cui non manca nulla, escluso Dario, con il nome:
$$\pi_{\text{Nome}}\Big(\text{STUDENTI} \bowtie_{\text{Matricola}=\text{Studente}} \sigma_{\text{Studente} \neq 104}\big(\pi_{\text{Studente}}(\text{TUTTE}) - \pi_{\text{Studente}}(\text{MANCANTI})\big)\Big)$$
$\{101, \dots, 105\} - \{102, 103, 105\} = \{101, 104\}$, tolto 104 → {101}.

| Nome |
|---|
| Anna |

> [!tip] Schema del "per ogni"
> $\pi_A(R) - \pi_A\big((\pi_A(R) \times S) - T\big)$: **tutti** i candidati meno quelli a cui **manca almeno una** combinazione richiesta. Qui $R$ = studenti, $S$ = corsi di Dario, $T$ = esami sostenuti.
> L'esclusione di Dario si può fare in qualunque punto (anche all'inizio, su STUDENTI), ma va fatta: Dario ha banalmente sostenuto tutti i propri corsi.
