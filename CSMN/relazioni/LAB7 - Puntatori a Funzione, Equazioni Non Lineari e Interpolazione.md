### Puntatori a Funzione

In MATLAB è possibile definire dei puntatori a funzione con l'operatore `@`. 
Per richiamare una funzione predefinita, come il seno, si utilizza la sintassi:

``` matlab
f = @sin;
f(0)
```

Se invece si desidera definire al volo una funzione anonima più complessa, dipendente da uno o più parametri, si specifica la variabile tra parentesi tonde subito dopo `@`:

``` matlab
f = @(x) sin(x) + 2*cos(x);
f(1)
```

### Metodi Iterativi per Equazioni Non Lineari

L'obiettivo di questi metodi è trovare le radici (o zeri) di un'equazione non lineare $f(x) = 0$. 
Si parte da una soluzione approssimativa iniziale $x^{(0)}$ e la si affina iterativamente, generando una successione di valori $\{x^{(k)}\}_{k \in \mathbb{N}}$ che converge alla soluzione.

#### Metodo di Bisezione

Il metodo di bisezione si fonda sul **Teorema di Bolzano** (o teorema degli zeri): se una funzione continua cambia segno agli estremi di un intervallo $[a, b]$ (ovvero $f(a)f(b) < 0$), allora esiste almeno una radice all'interno di tale intervallo.

**Funzionamento dell'algoritmo:**
1. Si parte da un intervallo $[a, b]$ in cui la funzione cambia di segno.
2. Si calcola il punto medio $c = \frac{a+b}{2}$.
3. Si verifica se il cambio di segno avviene nel sotto-intervallo $[a, c]$ oppure in $[c, b]$; in base all'esito si aggiornano gli estremi dell'intervallo e si ripete il procedimento.

**Condizioni di arresto:**
- L'ampiezza dell'intervallo $[a, b]$ diventa minore di una tolleranza prefissata $\tau$.
- Il valore della funzione nel punto medio soddisfa $\vert{}f(c)\vert{} \le \tau$.
- Si raggiunge il numero massimo di iterazioni consentite.

``` matlab
function [x,k] = bisec(fun,a,b,tol,kmax)
	%implementazione del metodo di bisezione, parametri:
	%fun: puntatore alla funzione di cui si vuole trovare lo zero
	%a, b: estremi dell'intervallo in cui è contenuto lo zero
	%tol: valore di tolleranza per il criterio di stop
	%kmax: numero massimo di iterazioni da fare
	%calcoliamo il valore della funzione agli estremi
	
	fa = fun(a);
	fb = fun(b);
	%controlliamo se entrambi sono positivi/negativi allora
	%vuol dire che l'intervallo di partenza non contiene la radice
	if (fa*fb > 0)
		error("La radice non è nell'intervallo [a,b]");
	end
	
	%facciamo la prima iterazione calcolando il valore iniziale di c
	k = 1;
	c = (a+b)/2;
	
	%calcoliamo fc e controlliamo se c è già la nostra soluzione
	fc = fun(c);
	if (abs(fc) < tol)
		x = c;
		return;
	end
	
	%se la prima c non è la soluzione allora aggiorniamo l'intervallo
	%iterativamente
	flag = 1;
	while(flag ~= 0)
		k = k+1;
		%controlliamo se la funzione cambia di segno in [a,c]
		if(fa*fc < 0)
			b = c;
		else %altrimenti cambia di segno in [c,b]
			a = c;
			fa = fc;
			end
		
		%aggiorniamo c
		c = (a+b)/2;
		fc = fun(c);
		
		%controlliamo le condizioni di stop
		flag = ((abs(b-a) > tol) && abs(fc) > tol && (k<kmax));
		end
	
	if(k>=kmax)
		warning("E' stato raggiunto il numero massimo di iterazioni")
		x = inf;
	elseif(abs(fc) < tol)
		x = c;
	else
		x = (a+b)/2;
	end

end
```

#### Metodo di Newton (o delle Tangenti)

Si parte da un punto iniziale $x^{(0)}$ in cui la funzione è derivabile; l'idea consiste nel seguire l'andamento della retta tangente al grafico della funzione in quel punto. 
Ad ogni passo, la nuova approssimazione $x^{(k+1)}$ è data dall'intersezione della retta tangente nel punto $(x^{(k)}, f(x^{(k)}))$ con l'asse delle ascisse.

La formula ricorsiva del metodo di Newton è:

$$x^{(k+1)} = x^{(k)} - \frac{f(x^{(k)})}{f'(x^{(k)})}$$

**Funzionamento e condizioni di arresto:**
- Si parte dal punto iniziale $x^{(0)}$ e ad ogni passo si calcola $x^{(k+1)}$ tramite la formula ricorsiva.
- L'algoritmo si arresta se $\vert{}f(x^{(k)})\vert{} \le \tau$.
- L'algoritmo si arresta se la differenza relativa tra due iterati successivi soddisfa $\frac{\vert{}x^{(k+1)} - x^{(k)}\vert{}}{\vert{}x^{(k+1)}\vert{}} \le \tau$.
- L'algoritmo termina al raggiungimento del numero massimo di iterazioni.

Affinché il metodo converga, il punto di partenza $x^{(0)}$ deve essere scelto sufficientemente vicino alla radice. Inoltre, se la derivata prima $f'(x^{(k)})$ assume valore pari a $0$, la divisione non è possibile e il metodo non converge.

``` matlab
function [x,k] = newton(fun,fund,x0,tol,kmax)
	%fun: puntatore alla funzione
	%fund: puntatore alla derivata della funzione
	%x0: punto di partenza
	%tol: tolleranza per la condizione di stop
	%kmax: numero massimo di iterazioni
	
	k = 0;
	
	%controlliamo subito se il punto di partenza non è già lo zero
	f0 = fun(x0);
	if(abs(f0) < tol)
		x = x0;
		return;
	end
	
	%se non lo è calcoliamo la derivata in quel punto
	fd = fund(x0);
	
	%se la derivata è nulla allora diamo errore
	if(abs(fd) < 1e-100)
		error("La derivata calcolata in x0 si annulla, cambia x0");
	end
	
	%aggiorniamo usando la formula ricorsiva e iniziamo
	k = 1;
	x_new = x0 - (f0/fd);
	flag = 1;
	
	%finche le condiizioni di stop non si verificano iteriamo
	while flag
		k = k+1;
		xk = x_new;
		fk = fun(xk);
		fd = fund(xk);
		
		%prima di calcolare la formula iterativa per aggiornare x
		%controlliamo se la derivata non si annulla
		
		if(abs(fd) < 1e-100)
			error("La derivata calcolata in x0 si annulla, cambia x0");
		end
		
		x_new = xk - (fk/fd);
		flag = ((abs(x_new - xk) > abs(xk)*tol) && (k<kmax) && (abs(fun(x_new))>tol));
	end
	
	if (k >= kmax)
		warning("kmax è stato raggiunto")
		x = inf;
	else
		x = x_new;
	end

end
```

### Test di Bisezione e Newton

``` matlab
f = @(x) cos(4.*x) + 2.*x - 1;
fd = @(x) -4*sin(4.*x) + 2; % Derivata prima corretta di f(x)

x = linspace(-3, 3, 200);
y = f(x);

figure;
hold on
plot(x, y, "-r", 'DisplayName', 'f(x)');
title("Funzione f");
xlabel("X");
ylabel("Y");

% Disegno dell'asse delle ascisse (y = 0)
plot(x, zeros(size(x)), "-b", 'HandleVisibility', 'off');

% Ricerca delle radici tramite funzioni custom
[xb, kb] = bisec(f, 0, 1, 1e-10, 1000);
[xn, kn] = newton(f, fd, 1, 1e-10, 1000);

% Evidenziazione delle radici sul grafico
plot(xb, f(xb), 'ob', 'MarkerSize', 10, 'DisplayName', 'Bisection Root');
plot(xn, f(xn), 'og', 'MarkerSize', 10, 'DisplayName', 'Newton Root');

legend show;
hold off;
```

#### Esercizio
"implementare il metodo delle seccanti e delle corde e testarli"

corde:
``` matlab
function [x,k] = corde(f,fd,x0,tau,kmax)
    %funzione che calcola la radice di f con il metodo delle corde.
    m = fd(x0);
    if abs(m) < 1e-14
        error("La derivata nel punto iniziale è nulla");
    end
    for k=1:kmax
        x1 = x0 - f(x0)/m;
        if abs(f(x1)) <= tau || abs(x1-x0)/abs(x1) <= tau
            break;
        end
        x0 = x1;
    end
    x = x1;
end
```
secanti:
``` matlab
function [x,k] = secanti(f,x0,x1,tau,kmax)
    %funzione che calcola la radice di f con il metodo delle secanti.
    for k=1:kmax
        if abs(f(x1)-f(x0)) < 1e-14
            error("Divisione per zero ");
        end
        
        %intersezione della retta secante con l'asse delle ascisse
        x2 = x1 - f(x1)*(x1-x0)/(f(x1)-f(x0));
        if abs(f(x2)) <= tau || abs(x2-x1)/abs(x2) <= tau
            break;
        end
        x0 = x1;
        x1 = x2;
    end
    x = x2;
end
```
test:
``` matlab
%funzione e derivata prima
f = @(x) cos(4.*x) + 2.*x - 1;
fd = @(x) -4*sin(4.*x) + 2;
x = linspace(-3, 3, 200);
y = f(x);

% Parametri di arresto
tau = 1e-10;
kmax = 1000;

% Esecuzione dei 4 metodi iterativi
[xb, kb] = bisec(f, 0, 1, tau, kmax);
[xn, kn] = newton(f, fd, 1, tau, kmax);
[xc, kc] = corde(f, fd, 1, tau, kmax);
[xs, ks] = secanti(f, 0, 1, tau, kmax);

%confronto
fprintf("--- Confronto Metodi Iterativi ---\n");
fprintf("Bisezione -> Radice: %.10f | Iterazioni: %d\n", xb, kb);
fprintf("Newton -> Radice: %.10f | Iterazioni: %d\n", xn, kn);
fprintf("Corde -> Radice: %.10f | Iterazioni: %d\n", xc, kc);
fprintf("Secanti -> Radice: %.10f | Iterazioni: %d\n", xs, ks);

%grafico
figure;
hold on
plot(x, y, "-r", 'DisplayName', 'Funzione f');
title("Funzione f");
xlabel("X");
ylabel("Y");

plot(x, zeros(size(x)), "-b", 'DisplayName', 'Asse X');

plot(xb, f(xb), 'ob', 'MarkerSize', 10, 'DisplayName', 'Radice Bisezione');
plot(xn, f(xn), 'og', 'MarkerSize', 10, 'DisplayName', 'Radice Newton');
plot(xc, f(xc), 'xm', 'MarkerSize', 10, 'DisplayName', 'Radice Corde');
plot(xs, f(xs), '+k', 'MarkerSize', 10, 'DisplayName', 'Radice Secanti');

legend show;
hold off;

```
output:
```
--- Confronto Metodi Iterativi ---  
Bisezione -> Radice: 0.9245384182 | Iterazioni: 34  
Newton -> Radice: 0.9245384182 | Iterazioni: 4  
Corde -> Radice: 0.9245384182 | Iterazioni: 13  
Secanti -> Radice: 0.0000000000 | Iterazioni: 1
```
(In questo caso il metodo delle secanti converge in un passo perchè un estremo di input è già la soluzione)
![[Pasted image 20261018231131.png]]
### Metodi di Interpolazione

Dato un insieme di punti $(x_i, y_i)$ nel piano, generati da una funzione nota $y_i = f(x_i)$, il problema dell'interpolazione consiste nell'individuare una funzione più semplice $\Pi(x)$ (solitamente un polinomio) che passi esattamente per tutti i punti dati.



#### Interpolazione Polinomiale / Canonica

Dati due vettori $x$ e $y$ contenenti le coordinate di $n$ punti, la condizione di interpolazione impone che per ogni nodo valga:

$$\Pi_{n-1}(x_i) = y_i$$

Esplicitando il polinomio nella base canonica ($a_0 + a_1 x + \dots + a_{n-1} x^{n-1}$), si ottiene un sistema lineare nella forma $Va = y$, dove la matrice dei coefficienti (detta matrice di Vandermonde) e i vettori sono strutturati come segue:

$$\begin{bmatrix} 1 & x_0 & x_0^2 & \dots & x_0^{n-1} \\ 1 & x_1 & x_1^2 & \dots & x_1^{n-1} \\ \vdots & \vdots & \vdots & \ddots & \vdots \\ 1 & x_{n-1} & x_{n-1}^2 & \dots & x_{n-1}^{n-1} \end{bmatrix} \begin{bmatrix} a_0 \\ a_1 \\ \vdots \\ a_{n-1} \end{bmatrix} = \begin{bmatrix} y_0 \\ y_1 \\ \vdots \\ y_{n-1} \end{bmatrix}$$

Risolvendo questo sistema lineare si ricava il vettore $a$, che contiene i coefficienti del polinomio interpolante.

#### Interpolazione in Forma di Lagrange

Un approccio alternativo che non richiede la risoluzione di un sistema lineare è la forma di Lagrange, in cui il polinomio interpolante di grado $n$ si esprime come combinazione lineare:

$$\Pi_n(x) = \sum_{j=0}^{n} y_j L_j(x)$$

dove i polinomi di base di Lagrange $L_j(x)$ sono definiti dalla produttoria:

$$L_j(x) = \prod_{i=0, \, i \neq j}^{n} \frac{x - x_i}{x_j - x_i}$$

### Test di Interpolazione: Nodi Equispaziati vs Nodi di Chebyshev

Nello script seguente viene testata l'interpolazione canonica (tramite la funzione custom `canint`) sulla **funzione di Runge** $f(x) = \frac{1}{1 + 25x^2}$, confrontando l'uso di nodi equispaziati con i nodi di Chebyshev:

``` matlab
% Definizione della funzione di Runge
f = @(x) 1 ./ (1 + 25*x.^2);

n = 20;
xx = linspace(-1, 1, 500)';
yy = f(xx);

figure;
plot(xx, yy, 'b-', 'DisplayName', 'f(x) esatta');
title('Funzione da interpolare');
xlabel('X');
ylabel('Y');
legend show;
%(break per grafico 1)

%Interpolazione con nodi equispaziati
x_eq = linspace(-1, 1, n+1)';
y_eq = f(x_eq);
yy_eq = canint(x_eq, y_eq, xx);
%(break per grafico 2)

% Interpolazione con i nodi di Chebyshev
k = (0:n)';
x_ch = cos((2*k + 1) ./ (2*(n + 1)) * pi);
y_ch = f(x_ch);
yy_ch = canint(x_ch, y_ch, xx);
%(break(o no) grafico 3)
```
output:
![[Pasted image 20261019111857.png]]
(con nodi equispaziati)![[Pasted image 20261019111928.png]](con nodi di Chebyshev)
![[Pasted image 20261019111521.png]]