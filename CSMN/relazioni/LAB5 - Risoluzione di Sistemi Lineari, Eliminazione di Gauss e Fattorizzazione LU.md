### Risoluzione di Sistemi Lineari

In MATLAB, un sistema lineare nella forma $Ax = b$ viene risolto tramite l'operatore backslash `\`, che dal punto di vista matematico corrisponde al calcolo di `inv(A) * b`. 

L'utilizzo di `\` è consigliato rispetto al calcolo esplicito dell'inversa perchè seleziona automaticamente l'algoritmo ottimale in base alla struttura della matrice. Nel caso in cui la matrice non presenti una struttura particolare ricorre al metodo di eliminazione di Gauss.

``` matlab
A = randn(5);
A = A * A';
b = rand(5, 1);

if abs(det(A)) <= 1e-14
    error("La matrice è singolare");
end

if size(A, 1) ~= size(b, 1)
    error("A e b devono avere lo stesso numero di righe");
end

if size(b, 2) ~= 1
    error("b deve essere un vettore colonna");
end

%Risoluzione del sistema lineare Ax = b
x = A \ b; 
```

### Metodo di Eliminazione di Gauss (Senza e Con Pivoting)

Abbiamo testato delle funzioni personalizzate che implementano la riduzione di Gauss su matrici di dimensioni crescenti (da $n = 10$ a $n = 100$), confrontando l'errore relativo e il tempo di esecuzione rispetto alla soluzione di riferimento `A \ b` di MATLAB.
#### Riduzione di Gauss standard (test)

La funzione `gauss(A, b)` trasforma il sistema originale in un sistema triangolare superiore equivalente $Ux = b_1$:

``` matlab
for n = 10 : 10 : 100
    A = randn(n);
    A = A * A';
    b = rand(n, 1);
    x = A \ b; % Soluzione di riferimento
    
    tic;
    [U, b1] = gauss(A, b);
    x1 = U \ b1; % Soluzione con riduzione di Gauss
    time = toc;
    
    % Calcolo errore relativo
    err = norm(x - x1) / norm(x);
    fprintf("Dimensione = %d | Errore relativo: %e | Tempo: %e\n", n, err, time);
end
```
output:
```
Dimensione = 10 | Errore relativo: 2.213942e-14 | Tempo: 6.900000e-05  
Dimensione = 20 | Errore relativo: 7.364660e-15 | Tempo: 6.800000e-05  
Dimensione = 30 | Errore relativo: 1.340871e-11 | Tempo: 8.300000e-05  
Dimensione = 40 | Errore relativo: 1.144142e-13 | Tempo: 1.100000e-04  
Dimensione = 50 | Errore relativo: 2.010936e-14 | Tempo: 1.520000e-04  
Dimensione = 60 | Errore relativo: 6.694461e-14 | Tempo: 2.920000e-04  
Dimensione = 70 | Errore relativo: 3.512325e-12 | Tempo: 3.610000e-04  
Dimensione = 80 | Errore relativo: 2.816051e-14 | Tempo: 5.210000e-04  
Dimensione = 90 | Errore relativo: 5.285278e-06 | Tempo: 7.930000e-04  
Dimensione = 100 | Errore relativo: 4.516695e-12 | Tempo: 6.780000e-04
```
#### Riduzione di Gauss con Pivoting (test)

In modo analogo, abbiamo eseguito il test introducendo la tecnica del pivoting tramite la funzione `gauss_pivot(A, b)`, che restituisce la matrice triangolare superiore `U` e il termine noto modificato `b2`:

``` matlab
for n = 10 : 10 : 100
    A = rand(n);
    A = A * A';
    b = rand(n, 1);
    x = A \ b;
    
    tic;
    [U, b2] = gauss_pivot(A, b);
    x1 = U \ b2;
    time = toc;
    
    err = norm(x - x1) / norm(x);
    fprintf("Dimensione = %d | Errore relativo: %e | Tempo: %e\n", n, err, time);
end
```
output:
```
Dimensione = 10 | Errore relativo: 5.823562e-12 | Tempo: 1.320000e-04  
Dimensione = 20 | Errore relativo: 1.088766e-13 | Tempo: 1.160000e-04  
Dimensione = 30 | Errore relativo: 2.177676e-13 | Tempo: 1.560000e-04  
Dimensione = 40 | Errore relativo: 2.531372e-13 | Tempo: 2.310000e-04  
Dimensione = 50 | Errore relativo: 2.626036e-12 | Tempo: 3.530000e-04  
Dimensione = 60 | Errore relativo: 2.418854e-11 | Tempo: 5.870000e-04  
Dimensione = 70 | Errore relativo: 2.735622e-10 | Tempo: 6.310000e-04  
Dimensione = 80 | Errore relativo: 1.668947e-12 | Tempo: 9.920000e-04  
Dimensione = 90 | Errore relativo: 8.390067e-13 | Tempo: 1.234000e-03  
Dimensione = 100 | Errore relativo: 3.504413e-12 | Tempo: 1.600000e-03
```
### Fattorizzazione $A = LU$

La decomposizione $LU$ consente di scomporre la matrice `A` nel prodotto di una matrice triangolare inferiore `L` e una triangolare superiore `U`.

Prima di applicarla ai sistemi lineari, abbiamo verificato l'accuratezza della funzione di decomposizione `alu(A)` calcolando l'errore relativo sulla ricostruzione della matrice di partenza:

``` matlab
A = randn(5);
A = A * A';

[L, U] = alu(A);
A1 = L * U;

err = norm(A - A1) / norm(A);
fprintf("Errore della decomposizione: %e\n", err);
```

Successivamente, per risolvere il sistema lineare $Ax = b$ tramite la fattorizzazione $A = LU$, si procede risolvendo in sequenza due sistemi triangolari:
1. $Ly = b$
2. $Ux = y$

``` matlab
for n = 10 : 10 : 100
    A = rand(n);
    A = A * A';
    b = rand(n, 1);
    x = A \ b;
    
    tic;
    [L, U] = alu(A);
    y = L \ b;
    x1 = U \ y;
    time = toc;
    
    err = norm(x - x1) / norm(x);
    fprintf("Dimensione = %d | Errore relativo: %e | Tempo: %e\n", n, err, time);
end
```
output:
```
Dimensione = 10 | Errore relativo: 1.934106e-14 | Tempo: 1.384000e-03  
Dimensione = 20 | Errore relativo: 1.862933e-14 | Tempo: 2.290000e-04  
Dimensione = 30 | Errore relativo: 9.062192e-14 | Tempo: 1.179000e-03  
Dimensione = 40 | Errore relativo: 1.761237e-12 | Tempo: 1.890000e-04  
Dimensione = 50 | Errore relativo: 2.868972e-12 | Tempo: 9.610000e-04  
Dimensione = 60 | Errore relativo: 9.279234e-13 | Tempo: 4.190000e-04  
Dimensione = 70 | Errore relativo: 1.123399e-12 | Tempo: 5.690000e-04  
Dimensione = 80 | Errore relativo: 4.332156e-12 | Tempo: 7.010000e-04  
Dimensione = 90 | Errore relativo: 1.672476e-11 | Tempo: 8.240000e-04  
Dimensione = 100 | Errore relativo: 6.416446e-11 | Tempo: 9.750000e-04
```

### Fattorizzazione $PA = LU$

Infine, abbiamo analizzato la decomposizione $PA = LU$ (che include la matrice di permutazione `P` derivante dal pivoting) attraverso la funzione `palu_decomposition(A)`. In questo caso, i due sistemi triangolari da risolvere in cascata diventano:
1. $Ly = Pb$
2. $Ux = y$

``` matlab
for n = 10 : 10 : 100
    A = rand(n);
    A = A * A';
    b = rand(n, 1);
    x = A \ b;
    
    tic;
    [L, U, P] = palu_decomposition(A);
    y = L \ (P * b);
    x1 = U \ y;
    time = toc;
    
    err = norm(x - x1) / norm(x);
    fprintf("Dimensione = %d | Errore relativo: %e | Tempo: %e\n", n, err, time);
end
```
output:
```
Dimensione = 10 | Errore relativo: 1.070273e-14 | Tempo: 3.700740e-01  
Dimensione = 20 | Errore relativo: 3.631817e-13 | Tempo: 1.343000e-03  
Dimensione = 30 | Errore relativo: 2.558047e-12 | Tempo: 3.190000e-04  
Dimensione = 40 | Errore relativo: 1.910897e-12 | Tempo: 6.300000e-04  
Dimensione = 50 | Errore relativo: 4.082266e-12 | Tempo: 2.492000e-03  
Dimensione = 60 | Errore relativo: 2.301505e-12 | Tempo: 1.234000e-03  
Dimensione = 70 | Errore relativo: 7.884899e-12 | Tempo: 1.564000e-03  
Dimensione = 80 | Errore relativo: 1.451536e-12 | Tempo: 1.894000e-03  
Dimensione = 90 | Errore relativo: 2.923180e-10 | Tempo: 2.354000e-03  
Dimensione = 100 | Errore relativo: 5.823574e-11 | Tempo: 3.326000e-03
```


### Funzioni utilizzate nei test precedenti

Gauss (no pivot)
``` matlab
function [U,b] = gauss(A,b)
%funzione che calcola la riduzione gaussiana di una matrice.

	[n,m] = size(A);
	
	if m~=n
		error("La matrice A deve essere quadrata");
	end
	
	if n~=size(b,1)
		error("La dimensione di b deve essere compatibile con A");
	end
	
	if abs(det(A)) < 1e-14
		error("La matrice A è singolare")
	end
	
	for k=1:n-1
		for j=k+1:n
			%elemento da annullare diviso il pivot
			m = A(j,k)/A(k,k);
			% for j = k:n
			% A(i,j) = A(i,j) - m * A(k,j);
			% end
			
			A(j,:)=A(j,:)-m*A(k,:);
			b(j)=b(j)-m*b(k);
		end
	end
	U=A;
end
```

(gauss con pivot)
```matlab
function [U,b] = gauss_pivot(A,b)
    %funzione che calcola la riduzione gaussiana con pivoting di una matrice.
    [n,m] = size(A);
    if m~=n
        error("La matrice A deve essere quadrata");
    end
    if n~=size(b,1)
        error("La dimensione di b deve essere compatibile con A");
    end
    if abs(det(A)) < 1e-14
        error("La matrice A è singolare")
    end
    for k=1:n-1
        %cerchiamo qual'è la riga con l'elemento massimo tra le righe
        %ancora da processare
        [~,pos]=max(abs(A(k:n,k)));
        l = pos + k - 1;
        
        %inversione delle righe
        A([k l],:) = A([l k],:);
        b([k l]) = b([l k]);
        
        for j=k+1:n
            %elemento da annullare diviso il pivot
            m = A(j,k)/A(k,k);
            A(j,:)=A(j,:)-m*A(k,:);
            b(j)=b(j)-m*b(k);
        end
    end
    U=A;
end 
```

Alu
``` matlab
function [L,U]=alu(A)
    [n,m] = size(A);
    if m~=n
        error("La matrice A deve essere quadrata");
    end
    if abs(det(A)) < 1e-14
        error("La matrice A è singolare")
    end
    n=size(A,1);
    L = eye(n);
    for k = 1:n-1
        for j = k+1:n
            m = A(j,k)/A(k,k);
            A(j,:)=A(j,:)-m*A(k,:);
            L(j,k)=m;
        end
    end
    U=A;
end
```


Palu
``` matlab
function [L,U,P] = palu_decomposition(A)
    [n,m] = size(A);
    if m~=n
        error("La matrice A deve essere quadrata");
    end
    if abs(det(A)) < 1e-14
        error("La matrice A è singolare")
    end
    P=eye(n);
    L=eye(n);
    for k=1:n-1
        %cerchiamo qual'è la riga con l'elemento massimo tra le righe
        %ancora da processare
        [~,pos]=max(abs(A(k:n,k)));
        l = pos + k - 1;
        
        %inversione delle righe
        A([k l],:) = A([l k],:);
        P([k l],:) = P([l k],:);
        %bisogna stare attenti quando si fa lo scambio in L a non toccare
        %la parte che fa parte della diagonale e le parti sopra
        L([k,l], 1:k-1) = L([l,k], 1:k-1);
        
        for j=k+1:n
            %elemento da annullare diviso il pivot
            m = A(j,k)/A(k,k);
            A(j,:)=A(j,:)-m*A(k,:);
            L(j,k)=m;
        end
    end
    U=A;
end
```