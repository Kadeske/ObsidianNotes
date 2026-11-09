# 09 - Udito, tatto e movimento

> Fonte: [[IUM/TEORIA/PDF/09-udito-movimento.pdf]]
> Riferimento: Dix, Finlay, Abowd, Beale — *Interazione uomo-macchina*, cap. 1 (sezioni Udito, Tatto e Movimento)

---

## 1. Udito

### Caratteri generali
- L'udito è la capacità di percepire i **suoni**: vibrazioni di un corpo che oscilla in un mezzo elastico (l'aria).
- Fornisce informazioni sull'**ambiente**: distanze, direzioni, oggetti…
- La percezione uditiva riguarda soprattutto le **sorgenti primarie** (gli oggetti che **emettono** il suono); quella visiva le **sorgenti secondarie** (gli oggetti che **riflettono** la luce).
- È un canale di input che **non richiede attenzione preventiva**: si può sfruttare nelle interfacce (es. **segnali di allarme**).

### L'orecchio

![[IUM09-orecchio.png|380]]

| Parte | Funzione |
|---|---|
| **Orecchio esterno** | Protegge l'orecchio interno e **amplifica** il suono |
| **Orecchio medio** | Trasmette le onde sonore come **vibrazioni** verso l'interno |
| **Orecchio interno** | Rilascia trasmettitori chimici che generano **impulsi nel nervo uditivo**; nell'**apparato vestibolare** percepisce anche le **accelerazioni** (equilibrio; un conflitto con la vista causa la **chinetosi**, il mal di movimento) |

### Caratteristiche del suono

![[IUM09-suono.png|360]]

| Caratteristica | Significato |
|---|---|
| **Altezza** (*pitch*) | **Frequenza**: grave = bassa frequenza, acuto = alta frequenza |
| **Intensità** (*loudness*) | **Volume** (ampiezza): la stessa frequenza può avere volumi diversi |
| **Timbro** | Dipende dallo **strumento** o dalla fonte che produce il suono |

### Capacità uditive
- Si sentono frequenze da circa **20 Hz a 15 kHz**.
- Le **alte frequenze** si distinguono **meno accuratamente** delle basse.
- Il sistema uditivo **filtra** i suoni: il rumore di fondo viene eliminato e si possono seguire suoni familiari senza farci caso (**cocktail party effect**: seguire una conversazione in mezzo alla folla). Un suono **"strano"**, invece, **non** viene filtrato.

## 2. Tatto

- Fornisce feedback importanti sull'ambiente (temperatura, peso…), fondamentali per **coordinare i movimenti**.
- **Feedback aptico = tatto + cinestesia**.
- Non è localizzato ma **diffuso su tutto il corpo**; alcune aree sono più sensibili di altre, a seconda del loro uso.
- Può essere un senso fondamentale per chi ha **disabilità visive**.

### Recettori della pelle

| Recettore | Sensibile a |
|---|---|
| **Termocettori** | Temperatura |
| **Nocicettori** (nelle slide: *chemiocettori*) | Pressione, calore e dolore **intensi** |
| **Meccanocettori** | Pressione |

> [!note] Nel testo di Dix et al. i recettori di calore, pressione e dolore intensi sono i **nocicettori**; le slide li chiamano "chemiocettori".

### Cinestesia e propriocezione
- **Cinestesia**: consapevolezza della **posizione e del movimento** del proprio corpo e degli arti, grazie a recettori nei **muscoli e nei tendini**; fornisce feedback al sistema motorio.
- **Sistema propriocettivo**: utilissimo nella vita quotidiana, ci aiuta anche **senza la vista**, ma è **assente in alcuni ambienti di simulazione** (es. realtà virtuale).

## 3. Movimento

Il tempo per rispondere a uno stimolo è:

$$\text{tempo di risposta} = \text{tempo di reazione} + \text{tempo di movimento}$$

- Il **tempo di movimento** dipende da età, forma fisica, allenamento…
- Il **tempo di reazione** dipende dal tipo di stimolo:

| Stimolo | Tempo di reazione |
|---|---|
| Uditivo | ~**150 ms** |
| Visivo | ~**200 ms** |
| Dolore | ~**700 ms** |

- Chiedere tempi di risposta sempre più brevi **riduce l'accuratezza** dell'utente inesperto, ma non di quello addestrato.

Nel seguito ci si concentra sul movimento per **puntare un oggetto sullo schermo**.

### La legge di Fitts

Descrive il **tempo necessario per raggiungere un bersaglio** sullo schermo, mettendo in relazione velocità e precisione:

$$MT = a + b \log_2\!\left(\frac{D}{W} + 1\right)$$

| Simbolo | Significato |
|---|---|
| $MT$ | Tempo di movimento |
| $a$, $b$ | Costanti determinate empiricamente |
| $D$ | Distanza da percorrere |
| $W$ | Dimensione (larghezza) del bersaglio |

### Conseguenze per il design
- Per ridurre il tempo di movimento: **aumentare la dimensione** del bersaglio e/o **ridurre la distanza**.
- **Ingrandire i controlli usati più spesso** e **raggrupparli** in una zona facilmente raggiungibile.
- ⚠️ **La legge è logaritmica**: ingrandire del 20% un oggetto **piccolo** riduce sensibilmente il tempo, mentre ingrandire del 20% un oggetto **già grande** cambia poco.

![[IUM09-fitts-logaritmo.png|320]]

### Fasi del movimento
1. **Movimento iniziale rapido**, ad alta velocità ma impreciso.
2. **Decelerazione** finale, per raggiungere il bersaglio con precisione.

![[IUM09-fasi-movimento.png|400]]

### Gli angoli attivi
Il puntatore **non può uscire dai bordi dello schermo**: bordi e soprattutto **angoli** si comportano come bersagli di **dimensione infinita** e sono quindi i punti più facili da raggiungere (es. menu e pulsanti posti agli angoli).

![[IUM09-angoli-attivi.png|400]]

**Esempi**: le icone dello smartphone ingrandite per facilitare il tocco (le piccole "x" per eliminare le app sono invece bersagli difficili); gli elementi principali di un'interfaccia mobile posti vicino al pollice.

---

## Termini e concetti da ricordare

- **Udito**: sorgenti **primarie** (emettono suono) vs vista: sorgenti **secondarie** (riflettono luce); canale che **non richiede attenzione** → allarmi.
- **Orecchio** esterno (amplifica), medio (vibrazioni), interno (nervo uditivo + **apparato vestibolare**, accelerazioni, **chinetosi**).
- **Suono**: altezza (frequenza), intensità (volume), timbro (sorgente).
- Gamma udibile **20 Hz – 15 kHz**; alte frequenze distinte peggio; **cocktail party effect**.
- **Feedback aptico = tatto + cinestesia**.
- **Recettori**: termocettori (temperatura), nocicettori/"chemiocettori" (stimoli intensi), meccanocettori (pressione).
- **Cinestesia** e **propriocezione**: posizione del corpo; assenti in alcune simulazioni.
- **Tempo di risposta = reazione + movimento**; reazione: uditivo ~150 ms, visivo ~200 ms, dolore ~700 ms.
- **Legge di Fitts**: $MT = a + b\log_2(D/W + 1)$; bersagli grandi e vicini; effetto **logaritmico**.
- **Fasi del movimento**: veloce e impreciso → decelerazione e precisione.
- **Angoli attivi**: bordi e angoli dello schermo = bersagli di dimensione infinita.
