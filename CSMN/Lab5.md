## Risoluzione di sistemi lineari.

In matlab un sistema lineare nella forma Ax=b si risolve tramite l'operatore \, che corrisponde a calcolare inv(A)*b.

l'utilizzo di \ è consigliato perchè va ad usare l'algoritmo ottimale in base alla struttura della matrice.

In caso la matrice non abbia una struttura particolare utilizza Gauss.

``` matlab
A = randn(5);

A = A*A';

b = rand(5,1);

  

if abs(det(A)) <= 1e-14

error("La matrice è singolare")

end

if size(A,1) ~= size(b,1)

error ("A e b devono avere lo stesso numero di righe")

end

if size(b,2) ~= 1

error ("b deve essere un vettore")

end

x = A \ b; %risolve il sistema lineare Ax=b
```

## Test della funzione di Gauss

``` matlab
for n = 10:10:100

A = randn(n);

A = A*A';

b = rand(n,1);

x = A \ b;

tic;

[U, b1]= gauss(A,b);

%soluzione con riduzione di gauss

x1 = U \ b1;

time=toc;

  

%calcolo errore

err=norm(x-x1)/norm(x);

fprintf("L'errore relativo con dimensione = %d è %e il tempo impiegato è %e\n",n,err,time);

end
```

## Test ALU

``` matlab
A = randn(5);

A=A*A';

[L,U]=alu(A);

A1 = L*U;

err=norm(A-A1)/norm(A);

fprintf("Errore della decomposizione %e\n", err);
```

Test Gauss A = LU

andiamo a risolvere i sistemi:

- Ly = b
- Ux = y

``` matlab
for n = 10: 10 : 100

A = rand(n);

A = A*A';

b = rand(n,1);

x = A\b;

tic;

[L,U] = alu(A);

y = L\b;

x1 = U\y;

time=toc;

  

err = norm(x-x1) / norm(x);

fprintf("L'errore relativo con dimensione = %d è %e il tempo impiegato è %e\n",n,err,time);

end
```

## Test Gauss con pivoting

``` matlab
for n = 10: 10 : 100

A = rand(n);

A = A*A';

b = rand(n,1);

x = A\b;

tic;

[U,b2] = gauss_pivot(A,b);

x1 = U\b2;

time=toc;

  

err = norm(x-x1) / norm(x);

fprintf("L'errore relativo con dimensione = %d è %e il tempo impiegato è %e\n",n,err,time);

end
```

## Test sistema PA=LU

Test PA=LU

Risolviamo i sistemi e calcoliamo l'errore relativo:

- Ly = Pb
- Ux = y

``` matlab
for n = 10: 10 : 100

A = rand(n);

A = A*A';

b = rand(n,1);

x = A\b;

tic;

[L,U,P] = palu_decomposition(A);

y = L\(P*b);

x1 = U\y;

time=toc;

  

err = norm(x-x1) / norm(x);

fprintf("L'errore relativo con dimensione = %d è %e il tempo impiegato è %e\n",n,err,time);

end
```