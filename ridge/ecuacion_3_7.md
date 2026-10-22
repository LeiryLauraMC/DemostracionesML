# Demostración de la ecuación 3.7

:::{admonition} Enunciado
:class: note

El estimador ridge para el vector de coeficientes está dado por la minimización de

$$
S_\lambda(\beta_0^\star,\boldsymbol\beta_s)=(\boldsymbol y-\beta_0^\star\mathbf 1-Z\boldsymbol\beta_s)^T(\boldsymbol y-\beta_0^\star\mathbf 1-Z\boldsymbol\beta_s)+\lambda\boldsymbol\beta_s^T\boldsymbol\beta_s , \tag{3.7}
$$

donde el término de regularización $L^2$, $\lambda\boldsymbol\beta_s^T\boldsymbol\beta_s$, con parámetro $\lambda>0$, se agrega a todos los coeficientes excepto al intercepto. Este término se conoce como *weight decay*.
:::

El objetivo es encontrar los valores $(\hat\beta_0^\star,\hat{\boldsymbol\beta}_s)$ que minimizan $S_\lambda$ y demostrar que esa solución **existe**, es **única** y tiene una **forma cerrada**.

## Notación y herramientas

- $\boldsymbol y\in\mathbb R^n$ es el vector de respuestas y $\mathbf 1\in\mathbb R^n$ el vector de unos.
- $Z\in\mathbb R^{n\times p}$ es la matriz de predictores estandarizados, con entradas $z_{ij}=(x_{ij}-\bar x_j)/s_j$ (ecuación 3.4).
- $\beta_0^\star=\beta_0+\sum_j\beta_j\bar x_j$ es el intercepto en la escala estandarizada, y $\boldsymbol\beta_s=(s_1\beta_1,\dots,s_p\beta_p)^T$.

De la sección {doc}`desarrollo_3_7_y_3_8` usaremos tres resultados:

$$
Z^T\mathbf 1=\mathbf 0,\qquad
\frac{\partial\left(\boldsymbol a^T\boldsymbol\beta\right)}{\partial\boldsymbol\beta}=\boldsymbol a,\qquad
\frac{\partial\left(\boldsymbol\beta^TA\boldsymbol\beta\right)}{\partial\boldsymbol\beta}=2A\boldsymbol\beta\ \ (A\text{ simétrica}).
$$

## Paso 1. Expansión de la función objetivo

Para separar el papel de $\beta_0^\star$ y el de $\boldsymbol\beta_s$, llamemos $\boldsymbol r=\boldsymbol y-\beta_0^\star\mathbf 1$. Con esto,

$$
S_\lambda=(\boldsymbol r-Z\boldsymbol\beta_s)^T(\boldsymbol r-Z\boldsymbol\beta_s)+\lambda\boldsymbol\beta_s^T\boldsymbol\beta_s .
$$

Desarrollamos el producto como en la sección anterior. Los términos cruzados $\boldsymbol r^TZ\boldsymbol\beta_s$ y $\boldsymbol\beta_s^TZ^T\boldsymbol r$ son escalares, uno transpuesto del otro, así que son iguales y se suman:

$$
\begin{aligned}
S_\lambda&=\boldsymbol r^T\boldsymbol r-\boldsymbol r^TZ\boldsymbol\beta_s-\boldsymbol\beta_s^TZ^T\boldsymbol r+\boldsymbol\beta_s^TZ^TZ\boldsymbol\beta_s+\lambda\boldsymbol\beta_s^T\boldsymbol\beta_s\\
&=\boldsymbol r^T\boldsymbol r-2\boldsymbol\beta_s^TZ^T\boldsymbol r+\boldsymbol\beta_s^TZ^TZ\boldsymbol\beta_s+\lambda\boldsymbol\beta_s^T\boldsymbol\beta_s .
\end{aligned}
$$

Finalmente, escribimos $\lambda\boldsymbol\beta_s^T\boldsymbol\beta_s=\boldsymbol\beta_s^T(\lambda I)\boldsymbol\beta_s$ y agrupamos los dos términos cuadráticos en $\boldsymbol\beta_s$:

$$
S_\lambda=\boldsymbol r^T\boldsymbol r-2\boldsymbol\beta_s^TZ^T\boldsymbol r+\boldsymbol\beta_s^T\left(Z^TZ+\lambda I\right)\boldsymbol\beta_s .
$$

Así, la penalización ridge simplemente suma $\lambda$ a la diagonal de $Z^TZ$.

## Paso 2. Condición de primer orden para el intercepto

Derivamos la forma original (3.7) respecto a $\beta_0^\star$ con la regla de la cadena. La derivada de $\boldsymbol u^T\boldsymbol u$ respecto a un escalar $t$ es $2\boldsymbol u^T\frac{\partial\boldsymbol u}{\partial t}$, y aquí $\boldsymbol u=\boldsymbol y-\beta_0^\star\mathbf 1-Z\boldsymbol\beta_s$, cuya derivada respecto a $\beta_0^\star$ es $-\mathbf 1$. La penalización no depende de $\beta_0^\star$. Entonces

$$
\frac{\partial S_\lambda}{\partial\beta_0^\star}=-2\,\mathbf 1^T(\boldsymbol y-\beta_0^\star\mathbf 1-Z\boldsymbol\beta_s)=0 .
$$

Dividiendo entre $-2$ y distribuyendo $\mathbf 1^T$,

$$
\mathbf 1^T\boldsymbol y-\beta_0^\star\,\mathbf 1^T\mathbf 1-\mathbf 1^TZ\boldsymbol\beta_s=0 .
$$

Ahora usamos que $\mathbf 1^T\boldsymbol y=n\bar y$, que $\mathbf 1^T\mathbf 1=n$ y que $\mathbf 1^TZ=\mathbf 0^T$:

$$
n\bar y-n\beta_0^\star-0=0
\qquad\Longrightarrow\qquad
\hat\beta_0^\star=\bar y=\frac1n\sum_{i=1}^n y_i .
$$

Este es el mismo resultado de la ecuación (3.8). Como $Z$ está centrada, el intercepto queda desacoplado del resto de coeficientes: su estimador es la media de $\boldsymbol y$, sin importar el valor de $\lambda$. Por eso tiene sentido no penalizarlo.

## Paso 3. Condición de primer orden para $\boldsymbol\beta_s$

Derivamos la expresión del Paso 1 respecto a $\boldsymbol\beta_s$, término por término:

- $\boldsymbol r^T\boldsymbol r$ no depende de $\boldsymbol\beta_s$, así que su derivada es $\mathbf 0$;
- $-2\boldsymbol\beta_s^TZ^T\boldsymbol r$ es lineal en $\boldsymbol\beta_s$, con $\boldsymbol a=Z^T\boldsymbol r$, así que su derivada es $-2Z^T\boldsymbol r$;
- $\boldsymbol\beta_s^T(Z^TZ+\lambda I)\boldsymbol\beta_s$ es una forma cuadrática con matriz simétrica, así que su derivada es $2(Z^TZ+\lambda I)\boldsymbol\beta_s$.

Igualando el gradiente a cero,

$$
\nabla_{\boldsymbol\beta_s}S_\lambda=-2Z^T\boldsymbol r+2\left(Z^TZ+\lambda I\right)\boldsymbol\beta_s=\mathbf 0
\qquad\Longrightarrow\qquad
\left(Z^TZ+\lambda I\right)\boldsymbol\beta_s=Z^T\boldsymbol r .
$$

En el óptimo, $\boldsymbol r=\boldsymbol y-\bar y\mathbf 1$. Distribuyendo $Z^T$ y usando de nuevo $Z^T\mathbf 1=\mathbf 0$,

$$
Z^T\boldsymbol r=Z^T\boldsymbol y-\bar y\,Z^T\mathbf 1=Z^T\boldsymbol y .
$$

Se obtienen así las **ecuaciones normales penalizadas**:

$$
\left(Z^TZ+\lambda I\right)\boldsymbol\beta_s=Z^T\boldsymbol y .
$$

## Paso 4. La matriz $Z^TZ+\lambda I$ siempre es invertible

Para despejar $\boldsymbol\beta_s$ necesitamos que $Z^TZ+\lambda I$ tenga inversa. Lo demostramos en tres partes.

**(a) $Z^TZ$ es semidefinida positiva.** Para cualquier vector $\boldsymbol v\in\mathbb R^p$,

$$
\boldsymbol v^TZ^TZ\boldsymbol v=(Z\boldsymbol v)^T(Z\boldsymbol v)=\|Z\boldsymbol v\|^2\ge 0 .
$$

Si $\boldsymbol u$ es un vector propio de $Z^TZ$ con valor propio $d^2$, es decir, $Z^TZ\boldsymbol u=d^2\boldsymbol u$ con $\boldsymbol u\neq\mathbf 0$, entonces $0\le\boldsymbol u^TZ^TZ\boldsymbol u=d^2\|\boldsymbol u\|^2$, y como $\|\boldsymbol u\|^2>0$, se concluye $d^2\ge0$. Algunos de estos valores propios pueden ser cero; eso pasa, por ejemplo, cuando hay multicolinealidad o cuando $p>n$.

**(b) Sumar $\lambda I$ desplaza los valores propios.** Con el mismo vector propio $\boldsymbol u$,

$$
(Z^TZ+\lambda I)\boldsymbol u=Z^TZ\boldsymbol u+\lambda\boldsymbol u=d^2\boldsymbol u+\lambda\boldsymbol u=(d^2+\lambda)\boldsymbol u .
$$

Es decir, $Z^TZ+\lambda I$ tiene los mismos vectores propios, con valores propios $d_j^2+\lambda\ge\lambda>0$.

**(c) Conclusión.** Una matriz simétrica cuyos valores propios son todos positivos es definida positiva, y su determinante (el producto de los valores propios) es distinto de cero; por lo tanto es invertible. Despejando,

$$
\boxed{\ \hat{\boldsymbol\beta}_s^{\,\text{ridge}}=\left(Z^TZ+\lambda I\right)^{-1}Z^T\boldsymbol y\ }
$$

Esto explica lo que se observa en la simulación de las notas con la **matriz de Hilbert**: $H^TH$ tiene valores propios extremadamente cercanos a cero, y por eso su inversa en mínimos cuadrados es numéricamente inestable. Al sumar $\lambda I$, el número de condición queda acotado:

$$
\kappa=\frac{d_{\max}^2+\lambda}{d_{\min}^2+\lambda}\ \ll\ \frac{d_{\max}^2}{d_{\min}^2}.
$$

## Paso 5. Es un mínimo global y único

Falta comprobar que el punto encontrado es un mínimo y no un máximo o un punto de silla. Calculamos la matriz Hessiana conjunta respecto a $(\beta_0^\star,\boldsymbol\beta_s)$. Las segundas derivadas se leen de la expansión de la sección {doc}`desarrollo_3_7_y_3_8`:

- $\dfrac{\partial^2S_\lambda}{\partial(\beta_0^\star)^2}=2n$;
- $\dfrac{\partial^2S_\lambda}{\partial\beta_0^\star\,\partial\boldsymbol\beta_s}=2Z^T\mathbf 1$, que viene del término $2\beta_0^\star\mathbf 1^TZ\boldsymbol\beta_s$;
- $\nabla^2_{\boldsymbol\beta_s}S_\lambda=2(Z^TZ+\lambda I)$.

Entonces

$$
\nabla^2S_\lambda=2\begin{pmatrix} n & \mathbf 1^TZ\\ Z^T\mathbf 1 & Z^TZ+\lambda I\end{pmatrix}
=2\begin{pmatrix} n & \mathbf 0^T\\ \mathbf 0 & Z^TZ+\lambda I\end{pmatrix}.
$$

La Hessiana es diagonal por bloques. El primer bloque, $n$, es positivo, y el segundo es definido positivo por el Paso 4. Por lo tanto, la Hessiana es definida positiva, lo que implica que:

- $S_\lambda$ es **estrictamente convexa**;
- el punto crítico encontrado es su **único mínimo global**. $\blacksquare$

## Consecuencias de la fórmula

### Contracción (*shrinkage*)

Sea $Z=UDV^T$ la descomposición en valores singulares de $Z$, donde $U$ y $V$ tienen columnas ortonormales y $D=\operatorname{diag}(d_1,\dots,d_p)$. Entonces $Z^TZ=VD^2V^T$ y $Z^TZ+\lambda I=V(D^2+\lambda I)V^T$, de modo que

$$
\begin{aligned}
Z\hat{\boldsymbol\beta}^{\,\text{ridge}}&=UDV^T\,V(D^2+\lambda I)^{-1}V^T\,VDU^T\boldsymbol y\\
&=UD(D^2+\lambda I)^{-1}DU^T\boldsymbol y=\sum_{j=1}^p \boldsymbol u_j\,\frac{d_j^2}{d_j^2+\lambda}\,\boldsymbol u_j^T\boldsymbol y .
\end{aligned}
$$

Cada dirección principal se encoge por el factor $\frac{d_j^2}{d_j^2+\lambda}\in(0,1)$. Las direcciones con poca varianza ($d_j$ pequeño) son las que más se encogen.

### Casos límite

- Si $\lambda\to 0$, se recupera el estimador de mínimos cuadrados $(Z^TZ)^{-1}Z^T\boldsymbol y$, cuando existe.
- Si $\lambda\to\infty$, el factor $\frac{d_j^2}{d_j^2+\lambda}\to0$ y $\hat{\boldsymbol\beta}_s\to\mathbf 0$.

### Por qué se llama *weight decay*

Si se minimiza $S_\lambda$ por descenso de gradiente con tasa de aprendizaje $\eta$, la actualización es

$$
\boldsymbol\beta_s^{(t+1)}=\boldsymbol\beta_s^{(t)}-\eta\left[\nabla\text{RSS}+2\lambda\boldsymbol\beta_s^{(t)}\right]
=(1-2\eta\lambda)\,\boldsymbol\beta_s^{(t)}-\eta\nabla\text{RSS}.
$$

Como $1-2\eta\lambda<1$, en cada iteración los pesos se reducen ("decaen") multiplicativamente antes de corregirse con el error.

### Forma restringida equivalente

Por el método de multiplicadores de Lagrange, minimizar $S_\lambda$ equivale a minimizar la suma de cuadrados de los residuos sujeta a $\|\boldsymbol\beta_s\|^2\le t$, para algún $t>0$ que depende de $\lambda$.
