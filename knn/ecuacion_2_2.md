# Demostración de la ecuación 2.2

:::{admonition} Enunciado (Definición 2.3 de las notas: error empírico y verdadero)
:class: note

El **riesgo** o **error verdadero** de un predictor $h$ se define como

$$
L_{\mathcal D}(h):=\underset{(\boldsymbol x,y)\sim\mathcal D}{\mathbb P}\left[h(\boldsymbol x)\neq y\right]:=\mathcal D\left(\{(\boldsymbol x,y):\ h(\boldsymbol x)\neq y\}\right). \tag{2.2}
$$

Como $\mathcal D$ es desconocida, en la práctica se usa el **error empírico** sobre la muestra de entrenamiento $S$:

$$
L_S(h):=\frac{\left|\{i\in[m]:\ h(\boldsymbol x_i)\neq y_i\}\right|}{m}.
$$

Además, las notas afirman que el predictor de Bayes $f_{\mathcal D}$ satisface $L_{\mathcal D}(f_{\mathcal D})\le L_{\mathcal D}(g)$ para cualquier otro clasificador $g$.
:::

Los símbolos $:=$ indican que (2.2) es una **definición**, de modo que, en sentido estricto, no se demuestra. Lo que sí se puede demostrar son los hechos que le dan sentido y que la conectan con el resto de la definición:

1. Las dos expresiones de la derecha de (2.2) están bien definidas y son iguales.
2. $L_{\mathcal D}(h)$ es el valor esperado de la pérdida 0-1.
3. El error empírico $L_S(h)$ es un estimador **insesgado** y **consistente** del error verdadero $L_{\mathcal D}(h)$.
4. El predictor de Bayes $f_{\mathcal D}$ tiene el menor error verdadero posible.

## Supuestos

- $\mathcal D$ es una distribución de probabilidad sobre $\mathcal X\times\mathcal Y$, con $\mathcal Y=\{0,1\}$.
- $h:\mathcal X\to\mathcal Y$ es una función medible.
- La muestra $S=\{(\boldsymbol x_1,y_1),\dots,(\boldsymbol x_m,y_m)\}$ está formada por observaciones independientes, todas con distribución $\mathcal D$.

Llamemos **evento de error** de $h$ al conjunto de puntos donde $h$ se equivoca:

$$
A_h=\{(\boldsymbol x,y)\in\mathcal X\times\mathcal Y:\ h(\boldsymbol x)\neq y\}.
$$

## Las dos expresiones de (2.2) coinciden

**Primero veamos que $A_h$ es medible**, para que tenga sentido calcular su probabilidad. Como $y$ solo toma los valores 0 y 1, la única forma de que $h(\boldsymbol x)\neq y$ es que $h(\boldsymbol x)=1$ y $y=0$, o que $h(\boldsymbol x)=0$ y $y=1$. Entonces

$$
A_h=\left(h^{-1}(\{1\})\times\{0\}\right)\ \cup\ \left(h^{-1}(\{0\})\times\{1\}\right).
$$

Como $h$ es medible, las preimágenes $h^{-1}(\{0\})$ y $h^{-1}(\{1\})$ son conjuntos medibles de $\mathcal X$. Su producto con $\{0\}$ o $\{1\}$ es medible en $\mathcal X\times\mathcal Y$, y la unión de dos conjuntos medibles también lo es. Por lo tanto, $\mathcal D(A_h)$ existe.

**Ahora la igualdad.** Si $(\boldsymbol x,y)$ tiene distribución $\mathcal D$, la probabilidad de cualquier evento es la medida $\mathcal D$ del conjunto de puntos donde ese evento ocurre. El evento "$h(\boldsymbol x)\neq y$" es exactamente "$(\boldsymbol x,y)\in A_h$", así que

$$
\underset{(\boldsymbol x,y)\sim\mathcal D}{\mathbb P}\left[h(\boldsymbol x)\neq y\right]
=\mathbb P\left[(\boldsymbol x,y)\in A_h\right]
=\mathcal D(A_h).
$$

Además, como $\mathcal D$ es una medida de probabilidad, $0\le L_{\mathcal D}(h)\le 1$.

## El riesgo verdadero es la pérdida 0-1 esperada

La **pérdida 0-1** asigna un 1 cuando el predictor se equivoca y un 0 cuando acierta:

$$
\ell_{0\text{-}1}\left(h,(\boldsymbol x,y)\right)=\mathbb 1\left[h(\boldsymbol x)\neq y\right]=\mathbb 1_{A_h}(\boldsymbol x,y).
$$

Esta función es una variable aleatoria que solo toma los valores 0 y 1: vale 1 con probabilidad $\mathcal D(A_h)$ y 0 con probabilidad $1-\mathcal D(A_h)$. Su esperanza es

$$
\mathbb E_{(\boldsymbol x,y)\sim\mathcal D}\left[\mathbb 1_{A_h}\right]
=1\cdot\mathcal D(A_h)+0\cdot\left(1-\mathcal D(A_h)\right)=\mathcal D(A_h)=L_{\mathcal D}(h).
$$

Escribir el riesgo como "pérdida esperada" es lo que permite generalizar la definición a otras funciones de pérdida, por ejemplo la pérdida cuadrática en regresión.

## Relación entre el error empírico y el error verdadero

Contar cuántas observaciones clasifica mal $h$ es lo mismo que sumar las indicadoras de error. Por eso el error empírico se puede escribir como un promedio:

$$
L_S(h)=\frac{\left|\{i\in[m]:\ h(\boldsymbol x_i)\neq y_i\}\right|}{m}=\frac1m\sum_{i=1}^m Z_i,
\qquad\text{donde } Z_i=\mathbb 1\left[h(\boldsymbol x_i)\neq y_i\right].
$$

Cada $Z_i$ es una variable de **Bernoulli** con parámetro $L_{\mathcal D}(h)$. En efecto, como $(\boldsymbol x_i,y_i)\sim\mathcal D$, por la sección anterior

$$
\mathbb P(Z_i=1)=\mathcal D(A_h)=L_{\mathcal D}(h),\qquad \mathbb E[Z_i]=L_{\mathcal D}(h).
$$

Además, las $Z_i$ son independientes entre sí, porque cada una es función de una observación distinta y las observaciones son independientes.

### Insesgadez

Por la linealidad de la esperanza,

$$
\mathbb E_{S\sim\mathcal D^m}\left[L_S(h)\right]=\frac1m\sum_{i=1}^m\mathbb E[Z_i]=\frac1m\cdot m\,L_{\mathcal D}(h)=L_{\mathcal D}(h).
$$

Es decir, en promedio sobre todas las muestras posibles, el error empírico es igual al error verdadero.

### Varianza

La varianza de una Bernoulli con parámetro $q$ es $q(1-q)$. Como las $Z_i$ son independientes, la varianza de su suma es la suma de las varianzas, y al sacar la constante $\frac1m$ de la varianza queda elevada al cuadrado:

$$
\begin{aligned}
\operatorname{Var}\left[L_S(h)\right]&=\frac1{m^2}\sum_{i=1}^m\operatorname{Var}(Z_i)=\frac1{m^2}\cdot m\,L_{\mathcal D}(h)\left(1-L_{\mathcal D}(h)\right)\\
&=\frac{L_{\mathcal D}(h)\left(1-L_{\mathcal D}(h)\right)}{m}.
\end{aligned}
$$

La función $q(1-q)$ alcanza su máximo en $q=\frac12$, donde vale $\frac14$. Por lo tanto,

$$
\operatorname{Var}\left[L_S(h)\right]\le\frac1{4m}.
$$

### Consistencia

La desigualdad de Chebyshev dice que, para una variable aleatoria $W$ con media $\mu$ y cualquier $\varepsilon>0$, $\mathbb P(|W-\mu|\ge\varepsilon)\le\operatorname{Var}(W)/\varepsilon^2$. Aplicándola a $W=L_S(h)$, con media $L_{\mathcal D}(h)$,

$$
\mathbb P\left(\left|L_S(h)-L_{\mathcal D}(h)\right|\ge\varepsilon\right)\le\frac{\operatorname{Var}\left[L_S(h)\right]}{\varepsilon^2}\le\frac{1}{4m\varepsilon^2}\xrightarrow[m\to\infty]{}0 .
$$

Así, $L_S(h)$ converge en probabilidad a $L_{\mathcal D}(h)$ cuando el tamaño de la muestra crece; esta es la ley débil de los grandes números. Con la desigualdad de Hoeffding se obtiene una cota más fuerte, que decae exponencialmente: $2e^{-2m\varepsilon^2}$.

:::{admonition} Interpretación
:class: tip

$L_{\mathcal D}(h)$ no se puede calcular porque no conocemos $\mathcal D$, pero $L_S(h)$ sí. Lo que acabamos de demostrar es que, para un predictor $h$ **fijo**, el error empírico es una buena aproximación del verdadero cuando $m$ es grande. Esto justifica el principio de minimización del riesgo empírico (ERM).

Hay que tener cuidado con una sutileza: si $h$ se elige mirando la misma muestra $S$, las $Z_i$ ya no son independientes de la elección de $h$, y la insesgadez deja de valer. Esa es precisamente la raíz del sobreajuste.
:::

## El predictor de Bayes minimiza el riesgo verdadero

Definamos la probabilidad condicional de la clase 1,

$$
\eta(\boldsymbol x)=\mathbb P[y=1\mid\boldsymbol x],
$$

y el predictor de Bayes de las notas,

$$
f_{\mathcal D}(\boldsymbol x)=\begin{cases}1,& \eta(\boldsymbol x)\ge \frac12\\[2pt] 0,&\text{en otro caso.}\end{cases}
$$

Queremos demostrar que $L_{\mathcal D}(f_{\mathcal D})\le L_{\mathcal D}(g)$ para todo clasificador $g:\mathcal X\to\{0,1\}$.

**Paso 1. Condicionar en $\boldsymbol x$.** Por la sección anterior, el riesgo es la esperanza de la indicadora de error. Usando la ley de la esperanza total (primero se promedia sobre $y$ dado $\boldsymbol x$, y luego sobre $\boldsymbol x$):

$$
L_{\mathcal D}(g)=\mathbb E\left[\mathbb 1[g(\boldsymbol x)\neq y]\right]=\mathbb E_{\boldsymbol x}\Big[\,\mathbb E\left[\mathbb 1[g(\boldsymbol x)\neq y]\mid\boldsymbol x\right]\Big]=\mathbb E_{\boldsymbol x}\Big[\,\mathbb P\left(g(\boldsymbol x)\neq y\mid\boldsymbol x\right)\Big].
$$

**Paso 2. Calcular el error condicional.** Con $\boldsymbol x$ fijo, $g(\boldsymbol x)$ es un número (0 o 1), y lo único aleatorio es $y$:

- si $g(\boldsymbol x)=1$, el clasificador se equivoca cuando $y=0$, lo que ocurre con probabilidad $1-\eta(\boldsymbol x)$;
- si $g(\boldsymbol x)=0$, se equivoca cuando $y=1$, lo que ocurre con probabilidad $\eta(\boldsymbol x)$.

Juntando los dos casos en una sola expresión:

$$
\mathbb P\left(g(\boldsymbol x)\neq y\mid\boldsymbol x\right)=
\mathbb 1[g(\boldsymbol x)=1]\left(1-\eta(\boldsymbol x)\right)+\mathbb 1[g(\boldsymbol x)=0]\,\eta(\boldsymbol x).
$$

**Paso 3. Comparar punto a punto.** El predictor de Bayes siempre escoge la opción con menor error condicional:

- si $\eta(\boldsymbol x)\ge\frac12$, entonces $1-\eta(\boldsymbol x)\le\eta(\boldsymbol x)$, y $f_{\mathcal D}$ escoge 1, cuyo error es $1-\eta(\boldsymbol x)$, el menor de los dos;
- si $\eta(\boldsymbol x)<\frac12$, entonces $\eta(\boldsymbol x)<1-\eta(\boldsymbol x)$, y $f_{\mathcal D}$ escoge 0, cuyo error es $\eta(\boldsymbol x)$, de nuevo el menor.

En ambos casos el error condicional de $f_{\mathcal D}$ es el mínimo de los dos valores posibles, y cualquier otro $g$ obtiene uno de esos mismos dos valores. Por lo tanto, para todo $\boldsymbol x$,

$$
\mathbb P\left(f_{\mathcal D}(\boldsymbol x)\neq y\mid\boldsymbol x\right)=\min\{\eta(\boldsymbol x),\,1-\eta(\boldsymbol x)\}\le\mathbb P\left(g(\boldsymbol x)\neq y\mid\boldsymbol x\right).
$$

**Paso 4. Integrar.** Si una función es menor o igual que otra en todo punto, su esperanza también es menor o igual. Tomando $\mathbb E_{\boldsymbol x}$ a ambos lados y usando el Paso 1:

$$
L_{\mathcal D}(f_{\mathcal D})=\mathbb E_{\boldsymbol x}\left[\min\{\eta(\boldsymbol x),1-\eta(\boldsymbol x)\}\right]\le L_{\mathcal D}(g). \qquad\blacksquare
$$

El valor $\mathbb E_{\boldsymbol x}\left[\min\{\eta(\boldsymbol x),1-\eta(\boldsymbol x)\}\right]$ se conoce como **error de Bayes**. Ningún clasificador puede tener un error verdadero menor, ni siquiera conociendo $\mathcal D$. Esta expresión reaparece en la siguiente sección, en la cota del error de 1-NN.

:::{admonition} Observación sobre el texto de las notas
:class: warning

Justo después de (2.2), las notas dicen que el objetivo es encontrar la $h$ que minimiza $L_{\mathcal D}(h)$, pero la fórmula de ERM que aparece a continuación minimiza $L_S(h)$:

$$
\text{ERM}_{\mathcal H}(S)\in\underset{h\in\mathcal H}{\arg\min}\ L_S(h).
$$

Lo ideal sería minimizar $L_{\mathcal D}$, pero como no se puede calcular, ERM minimiza su aproximación $L_S$. La sección sobre la relación entre ambos errores es justamente la que justifica ese cambio.
:::
