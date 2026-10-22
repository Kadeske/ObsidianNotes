
L'assegnazione di una variabile si esegue con `=`. 
Dopo un'assegnazione se non viene inserito il punto e virgola a fine riga, MATLAB stampa a schermo il risultato dell'assegnazione.

``` matlab
x = 1; % Il punto e virgola sopprime l'output a video
```

Il **Workspace** è la finestra (solitamente a sinistra) che mostra tutte le variabili in memoria. È uno strumento essenziale per il debugging. I comandi base visti per gestirlo dalla Command Window sono:


- `whos`: mostra tutte le variabili in memoria e i loro dettagli (usando `whos x` si vedono solo i dettagli di `x`).
- `clear`: elimina tutto dal workspace (con `clear x` si elimina solo `x`).
- `save("nomeFile.mat")`: salva lo stato attuale del workspace.
- `load("nomeFile.mat")`: ricarica un workspace salvato in precedenza.

I file con cui viene salvato il workspace ha estensione .mat


La funzione `diary` permette di salvare in un file `.txt` tutto l'output e i comandi digitati nella Command Window:

- `diary("nomeFile.txt")`: avvia la registrazione.
- `diary off` / `diary on`: sospende e riprende la registrazione.



gli operatori matematici principali sono:
- $+$ somma
- $-$ sottrazione
-  $*$ prodotto
- $/$ divisione
- $mod(x,y)$ operazione di x modulo y
  
Per misurare le prestazioni di uno script, è possibile calcolare il tempo di esecuzione di un blocco di codice inserendolo tra i comandi `tic` e `toc`:


``` matlab
tic
result = 10 + 20;
toc
```

### Input e Output

Per la visualizzazione delle variabili in output abbiamo tre alternative:

1. **Implicita**: scrivendo solo il nome della variabile senza `;`.
2. **Semplice**: usando `disp("Il valore è: " + x)`.
3. **Formattata**: usando `fprintf`, che permette di gestire il tipo di dato (`%d` per interi, `%f` per decimali, `%s` per stringhe).

esempio fprintf:
``` matlab
%d numeri interi.
%f numeri in virgola mobile.
%s stringhe.

fprintf("Il valore di x è: %d", x)
```

Per prendere dati in input dall'utente si usa la funzione `input()`. Se l'input deve essere letto come stringa testuale, serve un secondo parametro `"s"`:

  esempio input:

```
x = input("Inserire un numero\n");
s = input("Inserire una stringa\n", "s");
```

#### Costrutti

In MATLAB, ogni blocco (`if`, `switch`, `for`, `while`) deve essere sempre chiuso con la parola chiave `end`. _In caso di dubbi sulla sintassi, si può usare il comando `help nomefunzione`._

  
**Condizionali:**

L'**if** segue la classica struttura `if`, `elseif`, `else`.
``` matlab
x = input("Inserisci un numero\n")
if x>5
	disp("x è maggiore di 5");
elseif x>3
	disp("x è maggiore di 3");
else
	disp("x è minore o uguale di 3");
end
```

Lo **switch** utilizza `case` per le varie casistiche e `otherwise` come caso di default (l'equivalente del "default" in C/Java).
``` matlab
x = input("Inserisci un numero\n")

switch(x)
	case 1
		disp("Hai inserito 1");
	case 2
		disp("Hai inserito 2");
	case {3,4}
		disp("Hai inserito 3 o 4");
	otherwise
		disp("Hai inserito un numero diverso da 1, 2, 3 e 4");
end
```

**Iterativi:**

  Il **for** si basa su un range e segue la sintassi `for indice = start : step : fine`. A differenza di altri linguaggi, il ciclo viene eseguito anche quando l'indice è esattamente uguale al valore `fine`.
``` matlab
for i = 0 : 2: 20
	disp(i)
end
```
  
Il **while** ha la sintassi standard `while(condizione)` e cicla finché la condizione è vera.
``` matlab
x = 10
while(x > 0)
	x = x-1
end
```