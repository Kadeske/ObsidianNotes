
### Formattazione dell'Output

La funzione `format` in MATLAB controlla esclusivamente **come vengono visualizzati** i numeri nella Command Window, senza alterare il modo in cui il programma li salva o la loro precisione in memoria.

I principali formati visti a lezione sono:
- `format short`: mostra massimo 4 cifre decimali (è il formato di default).
- `format shorte`: mostra massimo 4 cifre decimali in notazione scientifica.
- `format long`: mostra massimo 15 cifre decimali.
- `format longe`: mostra massimo 15 cifre decimali in notazione scientifica.

``` matlab
format short
pi    %ans = 3.1416
format shorte
pi    %ans = 3.1416e+00
format long
pi    %ans = 3.141592653589793
format longe
pi    %ans = 3.141592653589793e+00
```
output:

### Funzioni di Arrotondamento

Per arrotondare un numero reale esistono diversi approcci. In MATLAB si utilizzano le seguenti funzioni predefinite:

- `fix(n)`: tronca semplicemente la parte decimale (arrotonda verso lo zero).
- `floor(n)`: arrotonda all'intero più vicino verso $-\infty$ (difetto).
- `ceil(n)`: arrotonda all'intero più vicino verso $+\infty$ (eccesso).
- `round(n)`: arrotonda all'intero più vicino matematicamente.

La funzione `round` possiede anche due varianti avanzate per specificare la precisione:
- `round(n, t)`: arrotonda usando `t` cifre dopo la virgola.
- `round(n, t, 'significant')`: arrotonda mantenendo `t` cifre **significative**.


``` matlab
fix(123.456)         % Risultato: 123
ceil(123.456)        % Risultato: 124
round(0.0012345, 3)  % Risultato: 0.001
round(0.0012345, 3, 'significant') % Risultato: 0.00123
```

### Aritmetica di Macchina e Perdita di Associatività

Abbiamo verificato sperimentalmente che, lavorando con i "numeri macchina" (cioè numeri con precisione finita e un numero limitato di cifre significative), **non vale più la proprietà associativa dell'addizione**.

Calcolare $(a+b)+c$ può dare un risultato diverso da $a+(b+c)$.

Nello script seguente arrotondiamo ogni singola operazione a 3 cifre significative per simulare una macchina a bassa precisione, per poi calcolare l'errore relativo rispetto all'operazione esatta:

``` matlab
a = input("inserisci numero ");
b = input("inserisci numero ");
c = input("inserisci numero ");

fla=round(a,3,"significant");
flb=round(b,3,"significant");
flc=round(c,3,"significant");

%calcolo (a+b)+c
flab = round(fla+flb, 3, "significant");
flr1 = round(flab+flc, 3, "significant");
r1 = (a+b)+c;

%calcolo a+(b+c)
flbc = round(flb+flc, 3, "significant");
flr2 = round(flbc+fla, 3, "significant");
r2 = a+(b+c);

% calcolo errori
err1 = abs(r1 - flr1)/abs(r1);
err2 = abs(r2 - flr2)/abs(r2);

fprintf("Il risultato con i numeri macchina di (a+b)+c = %d \n", flr1);
fprintf("Il risultato con i numeri macchina di a+(b+c) = %d \n", flr2);
fprintf("Il risultato senza i numeri macchina di (a+b)+c = %d \n", r1);
fprintf("Il risultato senza i numeri macchina di a+(b+c) = %d \n", r2);
fprintf("Nel caso (a+b)+c l'errore è: %d \n", err1);
fprintf("Nel caso a+(b+c) l'errore è: %d \n", err2);
```
output:
``` matlab
inserisci numero 100
inserisci numero 0.678
inserisci numero 0.678

Il risultato con i numeri macchina di (a+b)+c = 102  
Il risultato con i numeri macchina di a+(b+c) = 101  
Il risultato senza i numeri macchina di (a+b)+c = 1.013560e+02  
Il risultato senza i numeri macchina di a+(b+c) = 1.013560e+02  
Nel caso (a+b)+c l'errore è: 6.353842e-03  
Nel caso a+(b+c) l'errore è: 3.512372e-03
```
#### Epsilon di Macchina

L'**epsilon di macchina** (`eps`) è definito come il più piccolo valore positivo che, se sommato a 1, produce in memoria un risultato strettamente maggiore di 1. Rappresenta la distanza tra 1 e il numero macchina successivo.

Algoritmo per il calcolo dell'epsilon:
``` matlab
e = 1;

while 1 + e > 1
    e = e / 2;
end
e = e * 2;

disp("L'epsilon di macchina calcolato è pari a " + e);
eps % Comando predefinito per mostrare l'epsilon
```
output: `L'epsilon di macchina calcolato è pari a 2.2204e-16`
### Errore di Cancellazione (Cancellazione Numerica)

Supponiamo di voler calcolare:

$$\lim_{x \to 0} \frac{1-\cos(x)}{x^2}$$

Un approccio empirico sarebbe calcolare il valore della funzione per valori di $x$ sempre più piccoli ($10^{-1}, 10^{-2}, \dots$).


``` matlab
for i = -1 : -1 : -20
    x = 10^i;
    y = (1-cos(x)) / x^2;
    y2 = 1/2 * (sin(x/2) / (x/2))^2; % Formulazione matematica equivalente
    disp([y, y2]);
    pause
end
```
Output (riassunto): $y$ tende a 0 mentre $y_{2}$ tende a $5.000000000000000e-01$

Entrambe le formule sono matematicamente equivalenti e il limite teorico per entrambe è $1/2$. Tuttavia, per $x$ molto piccoli, la prima funzione produce un **errore di cancellazione**. Questo accade a causa dell'operazione $1 - \cos(x)$: poiché per $x \to 0$ il coseno tende a $1$, stiamo sottraendo tra loro due numeri quasi identici. La seconda formulazione, invece, evita questa sottrazione e risulta numericamente stabile.