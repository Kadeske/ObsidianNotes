
### Autovalori e Autovettori

Per calcolare gli autovalori e gli autovettori di una matrice quadrata si utilizza la funzione `eig`. Questa funzione è polimorfica, ovvero cambia comportamento a seconda di come viene invocata:

- `x = eig(A)`: restituisce un vettore `x` contenente tutti gli autovalori della matrice `A`.
- `[V, D] = eig(A)`: restituisce due matrici. Nelle colonne di `V` troviamo  i relativi autovettori di `A`, mentre in `D` abbiamo una matrice (diagonale) che contiene sulla diagonale principale gli autovalori associati. Ciascun autovalore nella colonna $i$-esima di `V` corrisponde all'elemento `D(i,i)`.

È possibile estrarre gli autovalori direttamente dalla matrice diagonale `D` usando la funzione `diag(D)`.

### Calcolo del Raggio Spettrale

Il raggio spettrale di una matrice corrisponde al valore assoluto massimo tra i suoi autovalori.

Si può calcolare in diversi modi:
1. Applicando il valore assoluto agli elementi dello spettro e trovando il massimo con `max`.
2. Applicando il valore assoluto, ordinando gli elementi in ordine decrescente tramite e prendendo il primo elemento.

(cambia solo il modo in cui si trova il massimo ma fanno la stessa cosa)

``` matlab
A = rand(10);
spettro1 = eig(A);

[V, D] = eig(A);
spettro = diag(D);

rho1 = max(abs(spettro));

tmp = sort(abs(spettro), 'descend');
rho2 = tmp(1);
```

### Matrici di Forma Particolare

**Matrici triangolari:** La funzione `tril` estrae la parte inferiore di una matrice, mentre `triu` estrae quella superiore.
``` matlab
L = tril(randn(3)); % Matrice triangolare inferiore
U = triu(randn(3)); % Matrice triangolare superiore
```

**Matrici diagonali:** La funzione `diag` è polimorfica. Se le si passa un vettore, costruisce una matrice diagonale con quei valori; se le si passa una matrice, ne estrae la diagonale.
``` matlab
D = diag(randn(1, 3));
D2 = diag(diag(randn(3)));
```

**Matrici ortogonali (`orth`):** La funzione `orth(A)` riceve una matrice `A` e restituisce una matrice `Q` le cui colonne formano una base ortonormale del sottospazio generato dalle colonne di `A`. Se `A` non ha rango pieno, la dimensione della matrice risultante sarà inferiore.
``` matlab
A = randn(3);
Q = orth(A);
Q * Q' % Il risultato tende alla matrice identità
```

### Creazione di Grafici

Per rappresentare i dati graficamente si utilizza la funzione `plot`. 
`plot(x1, y1, ..., xn, yn)`.
I vari xi yi sono i vettori contendenti le ascisse e le ordinate rispettivamente dei grafici da visualizzare

È possibile modificare l'aspetto grafico dei plot aggiungendo dei parametri aggiuntivi.
Alcuni esempi:
- `"LineWidth", n`: imposta lo spessore delle linee a `n`.
- `"Color", "codiceColore"`: imposta il colore della curva (es. `"r"` per rosso, `"g"` per verde, `"b"` per blu).
- `"LineStyle", "--"`: definisce lo stile della linea (es. tratteggiata `--`, puntata `:`, ecc.).


Per sovrapporre più curve sullo stesso grafico si ricorre al comando `hold on`. Inoltre, si possono aggiungere elementi descrittivi come un titolo (`title`) o una legenda (`legend`).

``` matlab
% Generazione del vettore x da 0 a 10 con passo 0.01
x = 0 : 0.01 : 10;

% Alternativa con linspace: linspace(inizio, fine, numero_punti)
% x = linspace(0, 10, 1000);

yc = cos(x);
ys = sin(x);

% Primo plot con linea personalizzata
plot(x, yc, "LineWidth", 1, "LineStyle", ":", "Color", "b");
hold on

% Secondo plot sovrapposto
plot(x, ys, "LineWidth", 2, "Color", "r");

title("Cosine and Sine");
legend("Cosine function", "Sine function");
```
output:
![[Pasted image 20261017124304.png]]