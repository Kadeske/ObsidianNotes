
I metodi iterativi consentono di determinare la soluzione di un sistema lineare $Ax = b$ per approssimazioni successive, costruendo iterativamente una sequenza di vettori $\{x^{(k)}\}_{k \in \mathbb{N}}$. Sotto determinate condizioni, la successione converge alla soluzione esatta $x^*$ per $k \to +\infty$.

L'approssimazione della soluzione al passo $k+1$ è definita dalla formula generale:

$$x^{(k+1)} = B x^{(k)} + f$$

dove $B$ prende il nome di **matrice di iterazione** ed $f$ è detto **vettore iterativo**.

### Condizione di convergenza

Un risultato teorico fondamentale garantisce che, se la matrice dei coefficienti $A$ è **strettamente diagonalmente dominante**, sia il metodo di Jacobi che il metodo di Gauss-Seidel convergono alla soluzione. Per questo motivo, nei test abbiamo generato appositamente matrici con questa proprietà.

#### Generazione di una Matrice Strettamente Diagonalmente Dominante

Per costruire (in MATLAB) una matrice strettamente diagonalmente dominante (per righe), partiamo da una matrice casuale, azzeriamo la sua diagonale principale e la sostituiamo con la somma dei valori assoluti degli elementi di ciascuna riga, moltiplicata per un fattore maggiore di 1 (nel nostro caso 2):

``` matlab
A = randn(5);

% Estrazione e rimozione della diagonale originale
D = diag(diag(A));
A = A - D;

% Calcolo della somma dei moduli degli elementi fuori diagonale per ogni riga
% Nota: sum() lavora per colonne, quindi sommiamo sulla trasposta A'
d = sum(abs(A'));
d = 2 * d;

% Inserimento della nuova diagonale dominante
A = A + diag(d);
```

### Test: Jacobi vs Gauss-Seidel

Per verificare facilmente l'errore relativo, abbiamo fissato a priori la soluzione esatta `x` come un vettore di tutti uni (`ones(n, 1)`), ricavando di conseguenza il termine noto tramite il prodotto $b = Ax$.

Come parametri di arresto per gli algoritmi iterativi sono stati impostati:
- `x0`: vettore iniziale nullo (`zeros(n, 1)`).
- `tau`: tolleranza sull'errore pari a `1e-10`.
- `kmax`: numero massimo di iterazioni consentite pari a `1000`.


``` matlab
for n = 10 : 10 : 100
    % Generazione matrice strettamente diagonalmente dominante
    A = randn(n);
    D = diag(diag(A));
    A = A - D;
    d = sum(abs(A'));
    d = 2 * d;
    A = A + diag(d);
    
    % Costruzione della soluzione esatta
    x = ones(n, 1);
    b = A * x;
    
    % Parametri di inizio e fine
    x0 = zeros(n, 1);
    tau = 1e-10;
    kmax = 1000;
    
    % Esecuzione dei metodi
    [xgs, kgs] = gauss_seidel(A, b, x0, tau, kmax);
    [xj, kj] = jacobi(A, b, x0, tau, kmax);
    
    % Calcolo degli errori relativi
    err_gs = norm(xgs - x) / norm(x);
    err_j = norm(xj - x) / norm(x);
    
    % Stampa dei risultati
    fprintf("Dimensione %d\n", n);
    fprintf("Gauss-Seidel -> Errore: %e; Iterazioni: %d\n", err_gs, kgs);
    fprintf("Jacobi       -> Errore: %e; Iterazioni: %d\n\n", err_j, kj);
end
```
output:
```
Dimensione 10  
Gauss-Seidel -> Errore: 1.506200e-12; Iterazioni: 9  
Jacobi -> Errore: 1.036606e-11; Iterazioni: 15  
  
Dimensione 20  
Gauss-Seidel -> Errore: 2.518304e-12; Iterazioni: 9  
Jacobi -> Errore: 1.154604e-11; Iterazioni: 13  
  
Dimensione 30  
Gauss-Seidel -> Errore: 1.738532e-12; Iterazioni: 9
Jacobi -> Errore: 4.246138e-12; Iterazioni: 13  
  
Dimensione 40  
Gauss-Seidel -> Errore: 3.441198e-13; Iterazioni: 9  
Jacobi -> Errore: 9.044905e-12; Iterazioni: 11  
  
Dimensione 50  
Gauss-Seidel -> Errore: 1.439190e-13; Iterazioni: 9  
Jacobi -> Errore: 1.657480e-12; Iterazioni: 11

Dimensione 60  
Gauss-Seidel -> Errore: 7.550125e-14; Iterazioni: 9  
Jacobi -> Errore: 1.019262e-12; Iterazioni: 11  
  
Dimensione 70  
Gauss-Seidel -> Errore: 1.305493e-12; Iterazioni: 8  
Jacobi -> Errore: 3.200823e-12; Iterazioni: 10

Dimensione 80  
Gauss-Seidel -> Errore: 6.937308e-13; Iterazioni: 8  
Jacobi -> Errore: 2.973776e-12; Iterazioni: 10  
  
Dimensione 90  
Gauss-Seidel -> Errore: 6.916133e-13; Iterazioni: 8  
Jacobi -> Errore: 1.542954e-12; Iterazioni: 10  
  
Dimensione 100  
Gauss-Seidel -> Errore: 2.826720e-13; Iterazioni: 8
Jacobi -> Errore: 9.766420e-13; Iterazioni: 10
```