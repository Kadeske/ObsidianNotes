## Autovalori e autovettori

Per calcolare gli autovalori e gli autovettori di una matrice si calcolano con la funzione eig, questa funzione è polimorfica, ovvero cambia il suo funzionamento in base a come viene chiamata.

facendo:

x = eig(A) in x avremo un vettore che contiene gli autovalori della matrice A.

facendo:

[V, D] = eig(A)

si avrenno in V come colonne gli autovettori di A, mentre in D una matrice diagonale che contiene nella diagonale gli autovalori associati a quei vettori.

L'autovalore associato all'autovettore che è in i-esima colonna di V sarà l'elemento D(i,i)

Per estrarre da D il vettore che contiene gli autovalori si può usare la funzione diag(D).

Come recuperare il raggio spettrale?

- Un modo potrebbe essere usare abs per creare il valore assoluto di tutti gli elementi dello spettro, poi estrarre il maggiore con max.
- Un altro modo potrebbe essere sempre usare abs, poi ordinare gli elemeni in ordine decrescente e prendere il massimo.

``` matlab
A = rand(10);

  

spettro1 = eig(A)

  

[V,D] = eig(A)

  

spettro = diag(D)

  

rho1 = max(abs(spettro))

  

tmp = sort(abs(spettro),'descend')

rho2 = tmp(1)
```

## Creazione matrici di forma particolare

Per creare una matrice triangolare inferiore si può generare prima una matrice con una delle funzioni rand che abbiamo già visto poi estrarre la parte sotto la diagonale con tril.

``` matlab
L = tril(randn(3))
```

Analogamente si può fare per la generazione di matrici triangolari superiori con triu.

``` matlab
U = triu(randn(3))
```

Per creare una matrice diagonale invece si può sfruttare la funzione diag, questa funzione è overloaded, cioè cambia comportamento a seconda di cosa gli si passa, infatti passando un vettore, crea una matrice diagonale che contiene in diagonale proprio le entrate di quel vettore, mentre se gli si passa una matrice estrae gli elementi nella diagonale di quella matrice.

Quindi per creare una matrice diagonale si può fare:

``` matlab
D = diag(randn(1,3))

%oppure

D2 = diag(diag(randn(3)))
```

La funzione orth invece permette, partendo da una matrice A di ottenere una matrice ortogonale, andando a creare una base ortonormale del sottospazio generato dalle colonne di A e usando i versori di questa base come colonne della nuova matrice.

Se A non ha rango massimo la dimensione della matrice di output sarà minore.

``` matlab
A = randn(3)

Q = orth(A)

Q*Q'
```

## Creazione grafici

Per poter creare i grafici in Matlab si può utilizzare la funzione plot, che ha la sequente sintassi:

plot(x1, y1, ..., xn, yn)

dove i vari xi yi sono i vettori contendenti le ascisse e le ordinate rispettivamente dei grafici da visualizzare.

si può usare legend per creare una leggenda per il grafico e si può usare title per creare un titolo che verrà visualizzato sopra il grafico.

Per modificare lo spessore delle curve in un plot si usa la seguente sintassi:

plot(x1, y1, ... altri parametri ... "LineWidth", n) dove n è lo spessore da dare alle curve in questo plot.

Analogamente per il colore si fa:

plot(x1, y1, ... altri parametri ... "Color", "codiceColore") alcuni valori possibili per codiceColore sono:

- "r" per il rosso
- "g" per il verde
- "b" per blue

Si può anche modificare lo stile della linea, facendo punteggiata, a trattini etc con:

plot(x1, y1, ... altri parametri ... "LineStyle", "--") cosi si fa la curva tratteggiata

altri valori per LineStyle e per Color qui: [https://it.mathworks.com/help/matlab/ref/matlab.graphics.chart.primitive.line-properties.html](https://it.mathworks.com/help/matlab/ref/matlab.graphics.chart.primitive.line-properties.html)

per creare un'altra curva sopra con un altro stile bisogna creare un altro plot, ma usare la funzione hold on per aggiungere la curva sopra.

``` matlab
%questo crea un vettore x con valori che vanno da 0 a 10 con uno step di

%0.01, quindi x conterrà 0, 0.01, 0.02,0.03... 9.99,10.0

x=0:0.01:10;

  

%alternativa per generare le nostre x usare linspace

%linspace(inizio, fine, numero_punti)

%x=linspace(0, 10, 100)

  

yc=cos(x);

ys=sin(x);

  

plot(x,yc,"LineWidth", 1, "LineStyle", ":", "Color", "b");

hold on

plot(x,ys,"LineWidth", 2, "Color", "r");

title("Cosine and Sine")

legend("Cosine function","Sine function");
```