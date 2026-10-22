## Introduzione a Matlab

L'assegnazione di una variabile si fa utilizzando =. Dopo che si esegue un'assegnazione se non si mette il ; viene mostrato il valore assegnato alla variabile.
``` matlab
x = 1;
```
  

All'interno del vostro ambiente di sviluppo avete il Workspace (a sinistra solitamente) che è una finestra che vi mostra tutte le variabili in memoria, cosa molto utile per il debugging.

Comandi di base per la gestione del workspace.

- whos mostra tutte le variabili del workspace
- whos x mostra la variabile x del workspace
- clear elimina tutto dal workspace
- clear x elimina la varibile x dal workspace
- save(nomeFile) salva il workspace
- load(nomeFile) carica il workspace specificato

I file in cui salvate il workspace devono avere l'estensione .mat.

``` matlab
whos

whos x

save workspace.mat

clear x

clear

load workspace.mat
```

  

Il diary è una funzionalità di Matlab che permette di registrare la sessione, cioè permette di salvare in un file .txt tutto quello che viene scritto nella command window.

- diary(nomeFile) avvia la registrazione della command window nel file specificato.
- diary off sospende temporaneamente la registrazione.
- diary on permette di riprendere la registrazione.

gli operatori matematici principali sono:

- + somma
- - sottrazione
- * prodotto
- / divisione
- mod(x,y) operazione di x modulo y (equivale a x%y in C)

E' possibile misurare la durata di esecuzione di una porzione di codice mettendola tra i comandi tic e toc.

``` matlab
x = 10;
y = 20;

tic
result = x + y;
toc
```

  

Come si possono visualizzare le variabili in matlab?

1) Visualizzazione implicita: non mettere il ; dopo la variabile.

``` matlab
x
```

2) Visualizzazione con disp.

``` matlab
disp("Il valore di x è: " + x)
```

3) Visualizzazione con fprintf, stampa formattata.

``` matlab
%d numeri interi.

%f numeri in virgola mobile.

%s stringhe.

fprintf("Il valore di x è: %d", x)

```
  

Per prendere un valore in input, potete usare la funzione input(prompt, "s"). Il secondo parametro è opzionale e si mette se si vuole che l'input venga trattato come se fosse una stringa.

``` matlab
x = input("Inserire un numero\n");

disp(x);

  

s = input("Inserire una stringa\n", "s");

disp(s);
```

  

#### Costrutti condizionali

- if, else, elseif.
- switch, case, otherwise.

esempio:

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

  

esempio switch:

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

con il comando help funzione si possono vedere informazioni relative alle funzioni.

  

### Costrutti iterativi

il ciclo for ha la seguente sintassi:

for indice = start : step : end

operazioni

end

dove:

- indice = start -> è l'inizializzazione dell'indice
- step -> incremento della variabile ad ogni iterazione
- end -> il ciclo continua fino a quando il valore dell'indice è minore di end

``` matlab
for i = 0 : 2: 20

disp(i)

end

```
  

il ciclo while ha la seguente sintassi:

while(condizione)

operazioni

end

dove il corpo del while viene ripetuto finchè la condizione è verificata

``` matlab
x = 10

while(x > 0)

x = x-1

end
```

