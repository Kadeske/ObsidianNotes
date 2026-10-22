La funzione format in Matlab controlla come vengono visualizzati (solo visualizzati non come vengono salvati) i numeri nella command window.

- format short -> mostra max 4 cifre decimali
- format shorte -> mostra max 4 cifre decimali in notazione scientifica
- format long -> mostra max 15 cifre decimali
- format longe -> mostra max 15 cifre decimali in notazione scientifica

``` matlab
format short

pi

format shorte

pi

format long

pi

format longe

pi
```

  

Per arrotondare un numero reale esistono diversi approcci, in Matlab le funzioni che si possono usare sono:

- fix(n) -> tronca la parte decimale.
- floor(n) -> arrotonda all'intero più vicino a -inf.
- ceil(n) -> arrotonda all'intero più vicino a +inf.
- round(n) -> arrotonda all'intero più vicino.
- round(n,t) -> come round ma arrotonda usando t cifre dopo la virgola.
- round(n,t,'significant') -> arrotonda alle t cifre significative.

``` matlab
fix(123.456)

ceil(123.456)

floor(123.654)

round(123.356)

round(0.0012345, 3)

round(0.0012345, 3, 'significant')
```

  

``` matlab
a = input("inserisci numero");

b = input("inserisci numero");

c = input("inserisci numero");

  

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

  

err1 = abs(r1 - flr1)/abs(r1);

err2 = abs(r2 - flr2)/abs(r2);

  

fprintf("Il risultato con i numeri macchina di (a+b)+c = %d \n", flr1);

fprintf("Il risultato con i numeri macchina di a+(b+c) = %d \n", flr2);

fprintf("Il risultato senza i numeri macchina di (a+b)+c = %d \n", r1);

fprintf("Il risultato senza i numeri macchina di a+(b+c) = %d \n", r2);

fprintf("Nel caso (a+b)+c l'errore è: %d \n", err1);

fprintf("Nel caso a+(b+c) l'errore è: %d \n", err2);
```

Algoritmo calcolo epsilon di macchina:

L'epsilon di macchina eps è il più piccolo valore positivo che se sommato a 1 produce un risultato diverso da 1.

``` matlab
e = 1;

  

while 1+e>1

e=e/2;

end

e=e*2;

disp("L'epsilon di macchina calcolato è pari a "+e);

  

eps
```

  

Supponiamo di voler calcolare il lim per x che tende a 0 di:

f(x) = (1-cos(x))/x^2, un modo sarebbe quello calcolare il valore della funzione in valori di x sempre più piccoli e vedere piano piano verso che valore tende.

  
 
```matlab
for i=-1:-1:-20

x = 10^i;

y = (1-cos(x))/x^2

y2 = 1/2*(sin(x/2)/(x/2))^2

pause

end
```

  

Quello che succede è che nonostante le due funzioni siano la stessa e che quindi il limite dovrebbe tendere per entrambe a 1/2 accade che nella prima formulazione avviene un errore di cancellazione per via della presenza di 1-cos(x).