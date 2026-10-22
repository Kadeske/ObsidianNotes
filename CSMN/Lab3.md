## Matrici e Vettori.

In matlab matrici e vettori si definiscono tramite [ ].

Per convenzione si usano le prime lettere maiuscole per le matrci e le ultime minuscole per i vettori.

I valori si separano usando:

- , o " " (space) per separare gli elementi di una riga;
- ; per gli elementi delle colonne.

si può trasformare un vettore colonna in un vettore riga o viceversa facendone la trasposta con '.

``` matlab
v1 = [1 2 3] %creazione vettore riga con " "

v2 = [1,2,3] %creazione vettore riga con ,

v3 = [1;2;3] % creazione vettore colonna

A = [1 2 3;
	4,5,6;
	7,8,9] % creazione di una matrice

A = [v1;
	v2;
	v3']
```

## Funzioni per la creazione di matrici e vettori.

- eye(n) | eye(n,m) -> genera una matrice identità di dimensione nxn o nxm
- zeros(n) | zeros(n,m) -> genera una matrice di zeri di dimensione n x n o n x m
- ones(n) | ones(n,m) -> genera una matrice di uno di dimensione n x n o n x m
- rand(n) | rand(n,m) -> genera una matrice di dimensione n x n o n x m con valori casuali nell'intervallo aperto (0,1) con una distribuzione uniforme.
- randn(n) | randn(n,m) -> come prima ma con valori campionati da una gaussiana con media 0 e deviazione standard 1.
- randi(max) | randi([min,max], n) | randi([min,max],n,m) -> crea una matrice di valori interi compresi tra min e max compresi e di dimensione n x n o n x m, se non si specifica la dimensione genera un singolo valore.

```matlab
n = 3;

m = 2;

imin = 10;
imax = 30;
 
I = eye(n)

A = zeros(n,m)

A = ones(n)

A = rand(n)

A = randn(n,m)

A = randi([imin imax],n,m)
```

  

## Operazioni tra matrici / vettori.

le operazioni matricili possibili sono:

- + esegue la somma matriciale
- - esegue la sottrazione matriciale
- * esegue il prodotto matriciale righe per colonne
- ^ esegue l'elevazione a potenza della matrice
- .* esegue la moltiplicazione elemento per elemento tra matrici
- .^ esegue l'elevazione a potenza dei singoli elementi della matrice
- > | < | == eseguono i confronti elemento per elemento tra le matrici e restituiscono una matrice di valori booleani.

Attenzione alla dimensioni delle matrici o dei vettori nel prodotto matriciale:

Se A ha dimensione nxm e B ha dimensione B pxq l'operazione matriciale è possibile solo se m=p e il risultato sarà una matrice con dimensione nxq.

Attenzione alle proprietà del prodotto matriciale: gode della proprietà associativa, ma non di quella commutativa.

``` matlab
A = randn(3)

B = randn(3)

  

A+B

A-B

  

C = A*B

D = B*A

E = A.*B

C == D

C == E

  

A^2

A.^2
```

## Accesso agli elementi di una matrice.

Per accedere agli elementi di una matrice si usano le (), gli indici delle righe e delle colonne partono da 1 e non da 0.

Con A(i,j) si accede all'elemento in posizione i, j della matrice.

Con A(:) si accede a tutti gli elementi della matrice (vengono dati in un vettore colonna).

Con A(i,:) si accede a tutti gli elementi della riga i-esima.

Con A(:,j) si accede a tutti gli elementi della colonna j-esima.

Con A(i:j,:) si accede alle righe da i a j.

Con A(:,i:j) si accede alle colonne da i a j.

Con A(i:j, k:l) si accede alla sotto matrice che parte in i,k e termina in j,l.

In certi casi se non sappiamo quanto è grande la matrice si può usare l'indice speciale end che indica l'ultimo indice di quella dimensione della matrice.
 
``` matlab
A = randn(5)

A(:)

A(1,:)

A(:,1)

A(1,1)

A(1:size(A,1),size(A,2):end)

A(1,2) = 9
```

## Trasposta, inversa, determinante, rango e norme

la trasposta (o aggiunta nel caso di matrici complesse) si genera con l'operatore '

l'inversa si può calcolare come:

- A^-1
- inv(A)

Sono due metodi instabili, vedremo che quando vorremo calcolare l'inversa per risolvere un sistema lineare ad esempio useremo un'altro operatore.

L'inversa è calcolabile se la matrice non è singolare, ovvero se il suo determinante è diverso da 0.

il determinante si calcola con il comando det(A).

il rango di una matrice si calcola con rank(A).

la norma di matrice si calcola con:

- norm(A) -> norma euclidea per i vettori e norma di frobenius per le matrici (equivale alla norma 2).
- norm(A,i) -> calcola la norma i della matrice o vettore.
- norm(A,inf) -> calcola la norma infinito.

``` matlab
A=randi(10,3)

norm(A) %calcolo norma di Frobenius

norm(A,1) %calcolo norma 1

norm(A,2) %calcolo norma 2

norm(A,inf) %calcolo norma infinito
```

la funzione reshape serve a cambiare la forma (le dimensioni) di una matrice o di un array senza modificarne i dati contenuti o l'ordine degli elementi in memoria.

La sintassi della funzione è:

B = reshape(A, sz1, sz2)

sz1 e sz2 sono le righe e le colonne della dimensione che si vuole ottenere.

La matrice che si ottiene viene formata leggendo gli elementi in ordine di colonna:

ad esempio A = [1 3; 2 4], reshape(A, 1, 4) metterà gli elementi in una riga leggendo prima la prima colonna poi la seconda quindi si otterrà [1 2 3 4].

## Esercizio

Generare un numero casuale n tra 10 e 30 finchè non è divisibile per 5. Generare una matrice quadrata di dimensione [n/5 x n/5] e popolarla con i numeri da 1 a (n/5)^2 ordinati per riga.

#### esempio

n = 25

creo una matrice 5x5

|   |   |   |   |   |
|---|---|---|---|---|
|1|2|3|4|5|
|6|7|8|9|10|
|11|12|13|14|15|
|16|17|18|19|20|
|21|22|23|24|25|

  

  

``` matlab
%Generazione del numero n divisibile per 5

n = 0;

while mod(n, 5) ~= 0 || n < 10

n = randi([10, 30]);

end

  

fprintf('Il numero generato è: %d\n', n);

  

%Definizione dimensione e inizializzazione matrice

dim = n / 5;

matrice = zeros(dim, dim); % Pre-allocazione per efficienza

  

%Popolamento con cicli annidati

contatore = 1;

for i = 1:dim

for j = 1:dim

matrice(i, j) = contatore;

contatore = contatore + 1;

end

end

disp(matrice)

  

%in alternativa si può fare cosi:

%1:(dim*dim) genera un vettore colonna con i valori da 1 a dim*dim

%poi usiamo reshape per creare la matrice quadrata

matrice = reshape(1:(dim*dim), dim, dim)';

disp(matrice)

  

%Visualizzazione

disp('La matrice [n/5 x n/5] popolata per riga:');

disp(matrice);
```