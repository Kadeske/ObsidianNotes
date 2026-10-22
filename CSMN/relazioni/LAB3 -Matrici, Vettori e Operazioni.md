### Creazione di Matrici e Vettori

Le matrici e i vettori si definiscono utilizzando le parentesi quadre `[ ]`. 
Per convenzione, si utilizzano le prime lettere maiuscole per nominare le matrici e le lettere minuscole per i vettori. 

I valori al loro interno si separano con:
- Lo spazio `" "` o la virgola `,` per separare gli elementi all'interno di una stessa riga.
- Il punto e virgola `;` per separare gli elementi delle colonne, andando di fatto a capo.

È possibile trasformare un vettore riga in un vettore colonna (o viceversa) calcolandone la trasposta tramite l'operatore apice `'`.
``` matlab
v1 = [1 2 3]; % Creazione di un vettore riga con lo spazio
v2 = [1, 2, 3]; % Creazione di un vettore riga con la virgola
v3 = [1; 2; 3]; % Creazione di un vettore colonna

A = [v1;
	v2;
	v3'] % matrice 3x3 con v3 trasposto

A = [1 2 3;
	 4 5 6;
	 7 8 9]; % Creazione di una matrice 3x3
```

### Funzioni predefinite per la creazione di matrici

MATLAB mette a disposizione diverse funzioni per generare rapidamente matrici di dimensione `n x n` (quadrate) o `n x m` (rettangolari):

- `eye(n)` o `eye(n,m)`: genera una matrice identità.
- `zeros(n)` o `zeros(n,m)`: genera una matrice composta interamente da zeri.
- `ones(n)` o `ones(n,m)`: genera una matrice composta interamente da uni.
- `rand(n)` o `rand(n,m)`: genera una matrice contenente valori casuali nell'intervallo aperto (0, 1), distribuiti in modo uniforme.
- `randn(n)` o `randn(n,m)`: genera una matrice di valori casuali da una distribuzione gaussiana con media 0 e deviazione standard 1.
- `randi(max)`, `randi([min, max], n)` o `randi([min, max], n, m)`: genera una matrice popolata da valori interi casuali compresi nell'intervallo specificato tra `min` e `max`. Se la dimensione non viene specificata, la funzione restituisce un singolo valore casuale.

### Operazioni tra Matrici e Vettori

Le principali operazioni matriciali e i relativi operatori sono:

- `+` e `-`: somma e sottrazione matriciale.
- `*`: prodotto matriciale classico per righe e colonne. 
	- Se la matrice `A` ha dimensione `n x m` e la matrice `B` ha dimensione `p x q`, l'operazione è possibile solo se `m = p`. Il risultato sarà una nuova matrice di dimensioni `n x q`.
- `^`: esegue l'elevamento a potenza dell'intera matrice.
- `.*` e `.^`: l'aggiunta del punto prima del simbolo dell'operatore indica che la moltiplicazione o l'elevamento a potenza devono essere eseguiti singolarmente **elemento per elemento**.
- `>`, `<`, `==`: eseguono un confronto logico elemento per elemento tra matrici, restituendo una nuova matrice composta da valori booleani.

_Nota sul prodotto matriciale:_ Il prodotto riga per colonna gode della proprietà associativa, ma **non** di quella commutativa.

  

### Accesso agli elementi (Indicizzazione)

Per accedere a uno o più elementi di una matrice si usano le parentesi tonde `()`. È fondamentale ricordare che gli indici di riga e colonna partono da 1 (e non da 0).

- `A(i, j)`: accede all'elemento in posizione `i, j`.
- `A(:)`: formatta l'intera matrice e ne restituisce tutti gli elementi sotto forma di un unico vettore colonna.
- `A(i, :)`: accede a tutti gli elementi della riga `i`-esima.
- `A(:, j)`: accede a tutti gli elementi della colonna `j`-esima.
- `A(i:j, :)`: accede a tutte le righe comprese tra `i` e `j`.
- `A(:, i:j)`: accede a tutte le colonne comprese tra `i` e `j`.
- `A(i:j, k:l)`: estrae la sottomatrice definita dalle righe da `i` a `j` e dalle colonne da `k` a `l`.

Quando le dimensioni esatte della matrice non sono note, si può utilizzare la parola chiave `end`, che funge da indice speciale per indicare l'ultima riga o l'ultima colonna di quella dimensione.
### Algebra Lineare: Inversa, Determinante e Norme

Abbiamo esplorato le funzioni necessarie per il calcolo delle grandezze fondamentali:

- **Trasposta**: si genera tramite l'operatore apice `'`. In presenza di numeri complessi, questo operatore calcola la matrice aggiunta.
- **Inversa**: si ottiene con la sintassi `A^-1` o utilizzando il comando `inv(A)`. Affinché l'inversa sia calcolabile, la matrice non deve essere singolare (ovvero il suo determinante deve essere diverso da 0). Questi due metodi diretti sono numericamente instabili, quindi si ricorrerà ad altri operatori per la risoluzione pratica dei sistemi lineari.
- **Determinante e Rango**: si calcolano rispettivamente con le funzioni `det(A)` e `rank(A)`.
- **Norme**: la funzione `norm(A)` calcola di default la norma euclidea se applicata ai vettori, e la norma di Frobenius se applicata alle matrici (che equivale alla norma 2). Si può specificare il calcolo di una norma differente con `norm(A, i)` per la norma `i`-esima, oppure `norm(A, inf)` per calcolare la norma infinito.
- **Reshape**: il comando `reshape(A, sz1, sz2)` consente di cambiare le dimensioni (la forma) di una matrice in `sz1` righe e `sz2` colonne, senza modificarne i dati o l'ordinamento in memoria. La nuova matrice viene popolata leggendo gli elementi del dato originale in ordine di colonna (scorrendo prima tutta la prima colonna, poi la seconda, e così via).
#### Esercizio

**Testo:** Generare un numero casuale `n` compreso tra 10 e 30, iterando finché tale numero non risulta divisibile per 5. Generare poi una matrice quadrata di dimensione `(n/5) x (n/5)` e popolarla con i numeri progressivi da 1 a `(n/5)^2`, ordinandoli per riga.

  
``` matlab
% Generazione del numero n divisibile per 5
n = 0;
while mod(n, 5) ~= 0 || n < 10
    n = randi([10, 30]);
end

fprintf('Il numero trovato è: %d\n', n);

% Definizione dimensione
dim = n / 5;

% --- Metodo 1: Uso dei cicli for annidati ---
matrice = zeros(dim, dim); % Pre-allocazione della memoria per efficienza
contatore = 1;

for i = 1:dim
    for j = 1:dim
        matrice(i, j) = contatore;
        contatore = contatore + 1;
    end
end
disp('Stampa Metodo 1:');
disp(matrice);

% --- Metodo 2: Uso vettorizzato e reshape ---
% L'istruzione 1:(dim*dim) crea un vettore riga con i valori progressivi.
% Tramite reshape creiamo la matrice, e usiamo la trasposta (') 
% per far sì che il popolamento avvenga per riga anziché per colonna.
matrice2 = reshape(1:(dim*dim), dim, dim)';

disp('La matrice popolata per riga (Metodo 2):');
disp(matrice2);
```
output:
```
Il numero trovato è: 30  
Stampa Metodo 1:  
1 2 3 4 5 6  
7 8 9 10 11 12  
13 14 15 16 17 18  
19 20 21 22 23 24  
25 26 27 28 29 30  
31 32 33 34 35 36  
  
La matrice popolata per riga (Metodo 2):
1 2 3 4 5 6  
7 8 9 10 11 12  
13 14 15 16 17 18  
19 20 21 22 23 24  
25 26 27 28 29 30  
31 32 33 34 35 36
```