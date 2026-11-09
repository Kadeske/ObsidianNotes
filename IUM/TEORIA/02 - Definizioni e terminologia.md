# 02 - Definizioni e terminologia

> Fonte: [[IUM/TEORIA/PDF/02- definizioni.pdf]]
> Slide di Albrecht Schmidt (CC BY-SA 4.0, <https://hci-lecture.de>), riviste e adattate da Davide Spano

---

## 1. Human-Computer Interaction

> [!quote] Definizione ACM SIGCHI (1992)
> *"Human-computer interaction is a discipline concerned with the **design, evaluation and implementation** of interactive computing systems for human use and with the study of major phenomena surrounding them."*

**Definizione pragmatica** (da informatici): *come abilitare l'interazione tra uno o più esseri umani e uno o più dispositivi computazionali, e con i dati.*

## 2. Sistema interattivo

Un **sistema interattivo** è un sistema computazionale che permette agli utenti di interagire **in tempo reale**: ogni interazione riceve un **feedback immediato** e visibile.

> [!info] "Tempo reale"
> Qui significa risposte percepite dall'utente come **istantanee**: in pratica, entro circa **100 ms** (Nielsen, *Powers of 10: Time Scales in UX*).

Il termine *interactive computing* è usato in modo simile, con meno enfasi sul sistema.

**Esempi**: interfacce grafiche (Windows, macOS) · dispositivi mobili (telefono Android) · console di gioco (Xbox, Nintendo Switch) · biglietterie automatiche dei treni · interfacce a riga di comando (console SSH).

## 3. Interfaccia utente

La **parte di un sistema con cui l'utente può interagire**. Caratteristiche:
- può supportare **input, output o entrambi**;
- **non** è limitata alle tecnologie digitali né ai sistemi interattivi.

**Termini simili**: interfaccia uomo-computer (HCI), interfaccia uomo-macchina (MMI, *Man-Machine Interface*, o HMI, *Human-Machine Interface*).

**Esempi**: la GUI di una pagina web · l'interfaccia vocale di uno smart speaker (Alexa) · pulsanti, interruttori, manopole e leve della cabina di pilotaggio di un aereo.

## 4. Definizioni dello standard ISO 9241-210:2019

| Termine | Definizione |
|---|---|
| **Utente** (*user*) | Persona che interagisce con il sistema, prodotto o servizio |
| **Contesto d'uso** | Combinazione di utenti, obiettivi e compiti, risorse e ambiente |
| **Sistema interattivo** | Combinazione di hardware e/o software e/o servizi e/o persone con cui gli utenti interagiscono per raggiungere un determinato obiettivo |
| **Interfaccia utente** | Tutti i componenti (software o hardware) di un sistema interattivo che forniscono informazioni e permettono all'utente di controllarlo per completare specifici compiti |

## 5. UI Design, Interaction Design, UX Design

| Disciplina | Di cosa si occupa |
|---|---|
| **User Interface Design** | Concezione e progettazione dell'interfaccia: scelta delle **modalità**, selezione degli **elementi** e loro **posizionamento** |
| **Interaction Design (IxD)** | Progettazione di prodotti e servizi interattivi in cui l'attenzione va oltre l'oggetto e include **il modo in cui gli utenti interagiranno con esso** |
| **User Experience (UX) Design** | Approccio **più olistico**: progetta l'**esperienza** complessiva di una persona con un dispositivo, sistema o applicazione, considerando l'interfaccia dentro un **contesto d'uso più ampio** |

Altre definizioni di Interaction Design:
- *"Progettare prodotti interattivi per supportare le persone nella loro vita quotidiana e lavorativa"* — Preece, Rogers, Sharp (2002)
- *"La progettazione di spazi per la comunicazione e l'interazione umana"* — Winograd (1997)

## 6. Utilità, usabilità, piacevolezza

| Qualità | Significato |
|---|---|
| **Utilità** | Il prodotto serve a raggiungere un obiettivo o completare un compito. È **essenziale** |
| **Usabilità** | Qualità ed efficienza: **quanto bene** il prodotto supporta l'utente nel raggiungere l'obiettivo |
| **Piacevolezza** | Può essere legata a utilità e usabilità, ma non necessariamente: un prodotto può piacere per qualunque altro motivo |

## 7. Usabilità secondo Jakob Nielsen

> [!quote] *Usability 101* (Nielsen, 2003)
> *"L'usabilità è un **attributo di qualità** che valuta quanto siano facili da usare le interfacce utente. Il termine si riferisce anche ai **metodi** volti a migliorare la facilità d'uso durante il processo di progettazione."*

### Le cinque componenti dell'usabilità

| Componente | Domanda chiave |
|---|---|
| **Apprendibilità** (*learnability*) | Quanto è facile svolgere i compiti di base **la prima volta**? |
| **Efficienza** (*efficiency*) | Una volta imparato, **quanto velocemente** si svolgono i compiti? |
| **Memorabilità** (*memorability*, nelle slide "retentività") | Tornando dopo un periodo di inattività, quanto facilmente si **recupera la competenza**? |
| **Errori** (*errors*) | Quanti errori si commettono, quanto sono **gravi** e quanto è facile **recuperare**? |
| **Soddisfazione** (*satisfaction*) | Quanto è **piacevole** usare il design? |

## 8. HCI e la "fuffa"

Aggettivi molto usati nel marketing delle UI, spesso **vaghi e privi di contenuto misurabile**, da **non usare** in questo corso per descrivere un'interfaccia:

*intuitive · user-friendly · responsive · clean · modern · fast · simple · attractive · seamless · interactive*

> [!tip] Al loro posto, si usano le componenti misurabili dell'usabilità (apprendibilità, efficienza, memorabilità, errori, soddisfazione).

## Riferimenti
- T. T. Hewett et al. (1992). *ACM SIGCHI Curricula for Human-Computer Interaction*. ACM.
- ISO 9241-210:2019 — <https://www.iso.org/obp/ui/#iso:std:iso:9241:-210:ed-2:v1:en>
- J. Nielsen (2003). *Usability 101: Introduction to Usability* — <https://www.nngroup.com/articles/usability-101-introduction-to-usability/>
- J. Nielsen (2009). *Powers of 10: Time Scales in User Experience* — <https://www.nngroup.com/articles/powers-of-10-time-scales-in-ux/>
- J. Preece, Y. Rogers, H. Sharp (2002). *Interaction Design*, cap. 9.
- T. Winograd (1997). *From computing machinery to interaction design*.
- Interaction Design Foundation — <https://www.interaction-design.org/literature/topics/interaction-design>

---

## Termini e concetti da ricordare

- **HCI** (ACM SIGCHI 1992): design, valutazione e implementazione di sistemi interattivi per l'uso umano.
- **Sistema interattivo**: risponde in tempo reale con feedback immediato; "tempo reale" ≈ **< 100 ms**.
- **Interfaccia utente**: la parte del sistema con cui si interagisce; input e/o output; non solo digitale.
- **HMI / MMI**: sinonimi di interfaccia uomo-macchina.
- **ISO 9241-210**: definizioni di *utente*, *contesto d'uso* (utenti + obiettivi e compiti + risorse + ambiente), *sistema interattivo*, *interfaccia utente*.
- **UI Design** (modalità, elementi, posizionamento) ⊂ **Interaction Design** (come si interagisce) ⊂ **UX Design** (l'esperienza complessiva nel contesto).
- **Utilità** (essenziale) · **usabilità** (quanto bene) · **piacevolezza** (gradimento).
- **Usabilità secondo Nielsen**: attributo di qualità *e* insieme di metodi.
- **5 componenti**: apprendibilità, efficienza, memorabilità, errori, soddisfazione.
- **"Fuffa"**: aggettivi di marketing (intuitive, user-friendly, seamless…) da evitare.
