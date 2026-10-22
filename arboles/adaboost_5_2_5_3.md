# Demostración de (5.2) y (5.3) a partir del Algorithm 5.1

:::{admonition} Enunciado (algoritmo AdaBoost)
:class: note

Considere una tarea de clasificación de dos clases con $N$ observaciones de entrenamiento $(y_n,\boldsymbol x_n)$, $n=1,\dots,N$, con $y_n\in\{-1,1\}$. El objetivo es diseñar un clasificador binario

$$
f(\boldsymbol x)=\operatorname{sgn}\{F(\boldsymbol x)\}, \tag{5.2}
$$

donde

$$
F(\boldsymbol x):=\sum_{k=1}^K a_k\,\phi(\boldsymbol x;\boldsymbol\theta_k), \tag{5.3}
$$

y $\phi(\boldsymbol x;\boldsymbol\theta_k)\in\{-1,1\}$ es el clasificador base en la iteración $k$, definido por un conjunto de parámetros $\boldsymbol\theta_k$ que deben estimarse.
:::

Las ecuaciones (5.2) y (5.3) son **definiciones**: el símbolo $:=$ indica la forma que se le quiere dar al clasificador. Lo que sí se puede demostrar es que:

1. el Algorithm 5.1 de las notas construye exactamente un clasificador de esa forma, y que cada una de sus líneas se deduce de minimizar la pérdida exponencial sobre funciones del tipo (5.3);
2. tomar el **signo** de $F$, como hace (5.2), es la decisión correcta.

## El algoritmo de referencia

Para tenerlo a la vista, el Algorithm 5.1 de las notas es (con la notación corregida que se comenta al final de la sección):

> **Inicialización:** $\omega_n^{(1)}=1/N$ para $n=1,\dots,N$; $i=1$.
>
> **Repetir:**
> 1. Calcular el $\boldsymbol\theta_i$ óptimo en $\phi(\cdot;\boldsymbol\theta_i)$ minimizando $P_i$, y calcular el $P_i$ óptimo.
> 2. $a_i=\frac12\ln\dfrac{1-P_i}{P_i}$.
> 3. $Z_i=0$. Para $n=1,\dots,N$: $\ \omega_n^{(i+1)}=\omega_n^{(i)}\exp\left(-y_na_i\phi(\boldsymbol x_n;\boldsymbol\theta_i)\right)$ y $Z_i=Z_i+\omega_n^{(i+1)}$.
> 4. Para $n=1,\dots,N$: $\ \omega_n^{(i+1)}=\omega_n^{(i+1)}/Z_i$.
> 5. $K=i$, $\ i=i+1$.
>
> **Hasta** que se cumpla un criterio de parada.
>
> **Salida:** $f(\cdot)=\operatorname{sgn}\left(\sum_{k=1}^Ka_k\phi(\cdot;\boldsymbol\theta_k)\right)$.

## Planteamiento

Buscamos una $F$ de la forma (5.3) que minimice la **pérdida exponencial** sobre los datos de entrenamiento:

$$
J(F)=\sum_{n=1}^N\exp\left(-y_nF(\boldsymbol x_n)\right).
$$

Esta pérdida es pequeña cuando $y_nF(\boldsymbol x_n)$ es grande y positivo, es decir, cuando $F$ tiene el mismo signo que la etiqueta, y crece rápidamente cuando tienen signos opuestos.

Optimizar los $2K$ parámetros $(a_k,\boldsymbol\theta_k)$ al mismo tiempo es computacionalmente muy costoso. Por eso AdaBoost lo hace de forma **voraz** (*greedy*): agrega un término en cada iteración y deja fijos los que ya calculó.

## Paso 1. Estructura recursiva

Definimos las sumas parciales

$$
F_0(\boldsymbol x)=0,\qquad F_i(\boldsymbol x)=F_{i-1}(\boldsymbol x)+a_i\,\phi(\boldsymbol x;\boldsymbol\theta_i),\quad i=1,\dots,K .
$$

Veamos por inducción que $F_i=\sum_{k=1}^i a_k\phi(\cdot;\boldsymbol\theta_k)$:

- **Caso base ($i=1$):** $F_1=F_0+a_1\phi(\cdot;\boldsymbol\theta_1)=a_1\phi(\cdot;\boldsymbol\theta_1)$, que es la suma con un solo término.
- **Paso inductivo:** si $F_{i-1}=\sum_{k=1}^{i-1}a_k\phi(\cdot;\boldsymbol\theta_k)$, entonces

$$
F_i=F_{i-1}+a_i\phi(\cdot;\boldsymbol\theta_i)=\sum_{k=1}^{i-1}a_k\phi(\cdot;\boldsymbol\theta_k)+a_i\phi(\cdot;\boldsymbol\theta_i)=\sum_{k=1}^{i}a_k\phi(\cdot;\boldsymbol\theta_k).
$$

En particular, $F_K$ es exactamente la $F$ de (5.3). Lo que falta demostrar es que los pares $(a_i,\boldsymbol\theta_i)$ que calcula el algoritmo son los óptimos en cada paso.

## Paso 2. El costo de cada iteración se escribe con pesos

En la iteración $i$, la función $F_{i-1}$ ya está fija, y buscamos el mejor par $(a,\boldsymbol\theta)$ para agregar. El costo es

$$
J(a,\boldsymbol\theta)=\sum_{n=1}^N\exp\left(-y_n\left[F_{i-1}(\boldsymbol x_n)+a\,\phi(\boldsymbol x_n;\boldsymbol\theta)\right]\right).
$$

Como la exponencial de una suma es el producto de las exponenciales,

$$
\begin{aligned}
J(a,\boldsymbol\theta)&=\sum_{n=1}^N\underbrace{\exp\left(-y_nF_{i-1}(\boldsymbol x_n)\right)}_{w_n^{(i)}}\exp\left(-y_na\,\phi(\boldsymbol x_n;\boldsymbol\theta)\right)\\
&=\sum_{n=1}^N w_n^{(i)}\exp\left(-y_na\,\phi(\boldsymbol x_n;\boldsymbol\theta)\right).
\end{aligned}
$$

Los **pesos** $w_n^{(i)}=\exp\left(-y_nF_{i-1}(\boldsymbol x_n)\right)$ no dependen de $a$ ni de $\boldsymbol\theta$: solo dependen de lo que se hizo en las iteraciones anteriores. Son grandes para las observaciones que $F_{i-1}$ clasifica mal y pequeños para las que clasifica bien.

**Inicialización.** Como $F_0=0$, todos los pesos iniciales valen $w_n^{(1)}=e^0=1$. Al normalizarlos para que sumen 1, cada uno vale $\omega_n^{(1)}=1/N$, que es la inicialización del algoritmo.

**La normalización no cambia el óptimo.** Si dividimos todos los pesos entre su suma, $\omega_n^{(i)}=w_n^{(i)}/\sum_m w_m^{(i)}$, el costo $J$ queda multiplicado por la constante positiva $1/\sum_m w_m^{(i)}$, y multiplicar una función por una constante positiva no cambia dónde está su mínimo. Por eso, en adelante, trabajamos con los pesos normalizados $\omega_n^{(i)}$, que suman 1.

## Paso 3. El $\boldsymbol\theta$ óptimo minimiza el error ponderado

Como tanto $y_n$ como $\phi(\boldsymbol x_n;\boldsymbol\theta)$ valen $\pm1$, su producto también vale $\pm1$:

- si el clasificador base **acierta**, $y_n\phi(\boldsymbol x_n;\boldsymbol\theta)=1$ y el factor exponencial vale $e^{-a}$;
- si **falla**, $y_n\phi(\boldsymbol x_n;\boldsymbol\theta)=-1$ y el factor vale $e^{a}$.

Definimos el **error ponderado** del clasificador base como la suma de los pesos de las observaciones que clasifica mal:

$$
P(\boldsymbol\theta)=\sum_{n:\ y_n\phi(\boldsymbol x_n;\boldsymbol\theta)<0}\omega_n^{(i)} .
$$

Como los pesos suman 1, los de las observaciones bien clasificadas suman $1-P(\boldsymbol\theta)$. Separando la suma de $J$ en aciertos y errores,

$$
J(a,\boldsymbol\theta)=e^{-a}\sum_{\text{aciertos}}\omega_n^{(i)}+e^{a}\sum_{\text{errores}}\omega_n^{(i)}=(1-P(\boldsymbol\theta))\,e^{-a}+P(\boldsymbol\theta)\,e^{a}.
$$

Reorganizando los términos,

$$
J(a,\boldsymbol\theta)=e^{-a}+\left(e^{a}-e^{-a}\right)P(\boldsymbol\theta).
$$

Si $a>0$, el factor $e^{a}-e^{-a}$ es positivo, y $e^{-a}$ no depende de $\boldsymbol\theta$. Por lo tanto, minimizar $J$ respecto a $\boldsymbol\theta$ es lo mismo que minimizar el error ponderado:

$$
\boldsymbol\theta_i=\arg\min_{\boldsymbol\theta}P(\boldsymbol\theta),\qquad P_i=P(\boldsymbol\theta_i).
$$

Esta es la primera línea del ciclo del algoritmo, y corresponde a la ecuación (5.6) de las notas.

## Paso 4. El coeficiente $a$ óptimo

Con $\boldsymbol\theta_i$ ya fijo, el costo depende solo de $a$:

$$
g(a)=(1-P_i)\,e^{-a}+P_i\,e^{a}.
$$

Derivamos e igualamos a cero:

$$
g'(a)=-(1-P_i)\,e^{-a}+P_i\,e^{a}=0
\qquad\Longrightarrow\qquad
P_i\,e^{a}=(1-P_i)\,e^{-a}.
$$

Multiplicando ambos lados por $e^{a}$ y dividiendo entre $P_i$,

$$
e^{2a}=\frac{1-P_i}{P_i}
\qquad\Longrightarrow\qquad
2a=\ln\frac{1-P_i}{P_i}
\qquad\Longrightarrow\qquad
a_i=\frac12\ln\frac{1-P_i}{P_i},
$$

que es la segunda línea del ciclo del algoritmo.

**Es un mínimo**, porque la segunda derivada es positiva para todo $a$:

$$
g''(a)=(1-P_i)\,e^{-a}+P_i\,e^{a}>0 .
$$

**Coherencia con el supuesto $a>0$ del Paso 3.**

$$
a_i>0\iff\frac{1-P_i}{P_i}>1\iff 1-P_i>P_i\iff P_i<\frac12 .
$$

Es decir, basta con que el clasificador base sea mejor que lanzar una moneda. Si en algún paso $P_i>\frac12$, se puede usar $-\phi$ en lugar de $\phi$, cuyo error ponderado es $1-P_i<\frac12$.

## Paso 5. La actualización de los pesos

Por el Paso 1, $F_i=F_{i-1}+a_i\phi(\cdot;\boldsymbol\theta_i)$. Entonces los pesos para la siguiente iteración son

$$
\begin{aligned}
w_n^{(i+1)}&=\exp\left(-y_nF_i(\boldsymbol x_n)\right)
=\exp\left(-y_nF_{i-1}(\boldsymbol x_n)\right)\exp\left(-y_na_i\phi(\boldsymbol x_n;\boldsymbol\theta_i)\right)\\
&=w_n^{(i)}\exp\left(-y_na_i\phi(\boldsymbol x_n;\boldsymbol\theta_i)\right).
\end{aligned}
$$

Para mantenerlos normalizados se divide entre su suma:

$$
\omega_n^{(i+1)}=\frac{\omega_n^{(i)}\exp\left(-y_na_i\phi(\boldsymbol x_n;\boldsymbol\theta_i)\right)}{Z_i},\qquad Z_i=\sum_{n=1}^N\omega_n^{(i)}\exp\left(-y_na_i\phi(\boldsymbol x_n;\boldsymbol\theta_i)\right).
$$

Estos son exactamente los dos ciclos "Para $n=1,\dots,N$" del algoritmo: el primero calcula los pesos sin normalizar y va acumulando su suma en $Z_i$; el segundo divide cada peso entre $Z_i$.

**Interpretación:**

- si $\phi_i$ **falla** en $\boldsymbol x_n$, el peso se multiplica por $e^{a_i}>1$ y aumenta;
- si **acierta**, se multiplica por $e^{-a_i}<1$ y disminuye.

Así, en la siguiente iteración el clasificador base se concentra en las observaciones difíciles.

## Paso 6. El resultado del algoritmo

Cuando se cumple el criterio de parada después de $K$ iteraciones, el algoritmo devuelve

$$
f(\cdot)=\operatorname{sgn}\left(\sum_{k=1}^K a_k\phi(\cdot;\boldsymbol\theta_k)\right)=\operatorname{sgn}\{F(\cdot)\}.
$$

Por el Paso 1, $F$ tiene la forma (5.3), y por los Pasos 2 a 5, cada par $(a_k,\boldsymbol\theta_k)$ es el que minimiza la pérdida exponencial en su etapa. Esto es justamente (5.2) y (5.3). $\blacksquare$

## Por qué se toma el signo de $F$

Falta justificar la elección de la función signo en (5.2). Fijemos un punto $\boldsymbol x$ y llamemos $p=\mathbb P(y=1\mid\boldsymbol x)$, de modo que $\mathbb P(y=-1\mid\boldsymbol x)=1-p$. Si $F$ toma en ese punto el valor $F(\boldsymbol x)$, la pérdida exponencial esperada es

$$
\mathbb E\left[e^{-yF(\boldsymbol x)}\mid\boldsymbol x\right]=p\,e^{-F(\boldsymbol x)}+(1-p)\,e^{F(\boldsymbol x)}.
$$

Derivando respecto a $F(\boldsymbol x)$ e igualando a cero,

$$
-p\,e^{-F}+(1-p)\,e^{F}=0
\qquad\Longrightarrow\qquad
e^{2F}=\frac{p}{1-p},
$$

y despejando,

$$
F^\star(\boldsymbol x)=\frac12\ln\frac{\mathbb P(y=1\mid\boldsymbol x)}{\mathbb P(y=-1\mid\boldsymbol x)} ,
$$

que es la ecuación (5.9) de las notas. La segunda derivada, $p\,e^{-F}+(1-p)\,e^{F}$, es positiva, así que $F^\star$ es un mínimo.

Ahora miremos su signo. El logaritmo es positivo cuando su argumento es mayor que 1, así que

$$
F^\star(\boldsymbol x)>0\iff\frac{p}{1-p}>1\iff p>\frac12 .
$$

Es decir, $\operatorname{sgn}F^\star(\boldsymbol x)$ asigna la clase más probable en cada punto: es el **clasificador de Bayes**, que según {doc}`../knn/ecuacion_2_2` es el de menor error posible. Como $F$ es la aproximación de $F^\star$ construida con los datos, tomar $\operatorname{sgn}F$ está plenamente justificado.

:::{admonition} Observaciones sobre el texto de las notas
:class: warning

- En la inicialización del Algorithm 5.1 aparece $\omega_n^{(1)}=1/N,\ i=1,2,\dots,N$. El índice debe ser $n=1,\dots,N$, porque recorre las observaciones.
- El coeficiente aparece como $a_i=1/2[\ln(1-P_i)/P_i]$, que se leería como $\frac{\ln(1-P_i)}{2P_i}$. Lo correcto, como se dedujo en el Paso 4 y como aparece en el texto previo al algoritmo, es $a_i=\frac12\ln\frac{1-P_i}{P_i}$.
- En la primera línea de la ecuación (5.5) se escribe $\boldsymbol\theta_i=\arg\min_{a,\boldsymbol\theta}\sum\cdots$. Como en ese paso $a$ se mantiene fijo, debe ser $\arg\min_{\boldsymbol\theta}$, tal como aparece en la segunda línea.
:::
