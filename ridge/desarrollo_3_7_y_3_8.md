# Verificación de la expansión de (3.7), la ecuación (3.8) y la forma cuadrática

:::{admonition} Enunciado
:class: note

Las notas afirman que la función objetivo de ridge, ecuación (3.7), puede reescribirse al desarrollar los productos y aplicar propiedades de la transpuesta como

$$
\begin{aligned}
S_\lambda(\beta_0^\star,\boldsymbol\beta_s)={}&\boldsymbol y^T\boldsymbol y-2\boldsymbol y^T\beta_0^\star\mathbf 1-2\boldsymbol y^TZ\boldsymbol\beta_s+2\beta_0^\star\mathbf 1^TZ\boldsymbol\beta_s\\
&+n(\beta_0^\star)^2+\boldsymbol\beta_s^TZ^TZ\boldsymbol\beta_s+\lambda\boldsymbol\beta_s^T\boldsymbol\beta_s ,
\end{aligned}
$$

que al derivar respecto a $\beta_0^\star$ se obtiene

$$
\frac{\partial S_\lambda(\beta_0^\star,\boldsymbol\beta_s)}{\partial\beta_0^\star}=-2n\bar y+2n\beta_0^\star=0, \tag{3.8}
$$

que $Z^T\mathbf 1=\mathbf 1^TZ=0$ (verifíquelo), y que la forma cuadrática $\boldsymbol x^TA\boldsymbol x$ se puede escribir como

$$
\boldsymbol x^TA\boldsymbol x=\sum_{j=1}^p\left(\sum_{i=1}^p x_iA_{ij}\right)x_j .
$$
:::

Esta sección reúne las herramientas algebraicas que se usan en las demostraciones de ridge y lasso, así que conviene hacerla con todo detalle.

## El modelo de partida

Las notas parten del modelo lineal

$$
y_i=\beta_0+\beta_1x_{i1}+\cdots+\beta_px_{ip}+\varepsilon_i,\qquad i=1,\dots,n, \tag{3.3}
$$

y estandarizan los predictores con

$$
z_{ij}=\frac{x_{ij}-\bar x_j}{s_j}. \tag{3.4}
$$

Despejando, $x_{ij}=\bar x_j+s_jz_{ij}$. Al reemplazar en (3.3), cada término $\beta_jx_{ij}$ se separa en $\beta_j\bar x_j+(s_j\beta_j)z_{ij}$, y agrupando las constantes queda

$$
y_i=\beta_0^\star+\beta_1^\star z_{i1}+\cdots+\beta_p^\star z_{ip}+\varepsilon_i, \tag{3.5}
$$

donde

$$
\beta_0^\star=\beta_0+\sum_{j=1}^p\beta_j\bar x_j,\qquad \beta_j^\star=s_j\beta_j .
$$

En forma matricial, con $Z\in\mathbb R^{n\times p}$ la matriz de entradas $z_{ij}$ y $\boldsymbol\beta_s=(\beta_1^\star,\dots,\beta_p^\star)^T$,

$$
\boldsymbol y=\beta_0^\star\mathbf 1+Z\boldsymbol\beta_s+\boldsymbol\varepsilon , \tag{3.6}
$$

y la función objetivo de ridge es

$$
S_\lambda(\beta_0^\star,\boldsymbol\beta_s)=(\boldsymbol y-\beta_0^\star\mathbf 1-Z\boldsymbol\beta_s)^T(\boldsymbol y-\beta_0^\star\mathbf 1-Z\boldsymbol\beta_s)+\lambda\boldsymbol\beta_s^T\boldsymbol\beta_s . \tag{3.7}
$$

## Expansión de la función objetivo

Para no cargar tanta notación, llamemos

$$
\boldsymbol a=\boldsymbol y,\qquad \boldsymbol b=\beta_0^\star\mathbf 1,\qquad \boldsymbol c=Z\boldsymbol\beta_s ,
$$

los tres vectores de $\mathbb R^n$ que aparecen dentro del paréntesis. Así, $S_\lambda=(\boldsymbol a-\boldsymbol b-\boldsymbol c)^T(\boldsymbol a-\boldsymbol b-\boldsymbol c)+\lambda\boldsymbol\beta_s^T\boldsymbol\beta_s$.

**Desarrollo del producto.** La transpuesta de una resta es la resta de las transpuestas, $(\boldsymbol a-\boldsymbol b-\boldsymbol c)^T=\boldsymbol a^T-\boldsymbol b^T-\boldsymbol c^T$. Multiplicando término a término (propiedad distributiva) aparecen nueve productos:

$$
\begin{aligned}
(\boldsymbol a-\boldsymbol b-\boldsymbol c)^T(\boldsymbol a-\boldsymbol b-\boldsymbol c)
&=\boldsymbol a^T\boldsymbol a-\boldsymbol a^T\boldsymbol b-\boldsymbol a^T\boldsymbol c\\
&\quad-\boldsymbol b^T\boldsymbol a+\boldsymbol b^T\boldsymbol b+\boldsymbol b^T\boldsymbol c\\
&\quad-\boldsymbol c^T\boldsymbol a+\boldsymbol c^T\boldsymbol b+\boldsymbol c^T\boldsymbol c .
\end{aligned}
$$

**Agrupación de los términos cruzados.** Cada producto $\boldsymbol u^T\boldsymbol v$ es un escalar y, como un escalar es igual a su transpuesta, $\boldsymbol v^T\boldsymbol u=\boldsymbol u^T\boldsymbol v$. Entonces $\boldsymbol b^T\boldsymbol a=\boldsymbol a^T\boldsymbol b$, $\boldsymbol c^T\boldsymbol a=\boldsymbol a^T\boldsymbol c$ y $\boldsymbol c^T\boldsymbol b=\boldsymbol b^T\boldsymbol c$, de modo que los términos cruzados se juntan de a dos:

$$
=\boldsymbol a^T\boldsymbol a+\boldsymbol b^T\boldsymbol b+\boldsymbol c^T\boldsymbol c-2\boldsymbol a^T\boldsymbol b-2\boldsymbol a^T\boldsymbol c+2\boldsymbol b^T\boldsymbol c .
$$

**Cálculo de cada término.** Usamos que $\beta_0^\star$ es un escalar (sale de los productos), que $\mathbf 1^T\mathbf 1=1+1+\cdots+1=n$ y que $(Z\boldsymbol\beta_s)^T=\boldsymbol\beta_s^TZ^T$:

| Término | Desarrollo | Resultado |
|---|---|---|
| $\boldsymbol a^T\boldsymbol a$ | | $\boldsymbol y^T\boldsymbol y$ |
| $\boldsymbol b^T\boldsymbol b$ | $(\beta_0^\star\mathbf 1)^T(\beta_0^\star\mathbf 1)=(\beta_0^\star)^2\,\mathbf 1^T\mathbf 1$ | $n(\beta_0^\star)^2$ |
| $\boldsymbol c^T\boldsymbol c$ | $(Z\boldsymbol\beta_s)^T(Z\boldsymbol\beta_s)$ | $\boldsymbol\beta_s^TZ^TZ\boldsymbol\beta_s$ |
| $-2\boldsymbol a^T\boldsymbol b$ | $-2\boldsymbol y^T(\beta_0^\star\mathbf 1)$ | $-2\boldsymbol y^T\beta_0^\star\mathbf 1$ |
| $-2\boldsymbol a^T\boldsymbol c$ | $-2\boldsymbol y^T(Z\boldsymbol\beta_s)$ | $-2\boldsymbol y^TZ\boldsymbol\beta_s$ |
| $2\boldsymbol b^T\boldsymbol c$ | $2(\beta_0^\star\mathbf 1)^T(Z\boldsymbol\beta_s)$ | $2\beta_0^\star\,\mathbf 1^TZ\boldsymbol\beta_s$ |

Sumando estos seis términos y la penalización $\lambda\boldsymbol\beta_s^T\boldsymbol\beta_s$ se obtiene

$$
\begin{aligned}
S_\lambda(\beta_0^\star,\boldsymbol\beta_s)={}&\boldsymbol y^T\boldsymbol y-2\boldsymbol y^T\beta_0^\star\mathbf 1-2\boldsymbol y^TZ\boldsymbol\beta_s+2\beta_0^\star\mathbf 1^TZ\boldsymbol\beta_s\\
&+n(\beta_0^\star)^2+\boldsymbol\beta_s^TZ^TZ\boldsymbol\beta_s+\lambda\boldsymbol\beta_s^T\boldsymbol\beta_s ,
\end{aligned}
$$

que es la expresión de las notas.

:::{admonition} Observación
:class: warning

En las notas, el cuarto término aparece como $2\beta_0\mathbf 1^TZ\boldsymbol\beta_s$, sin la estrella. Debe ser $2\beta_0^\star\mathbf 1^TZ\boldsymbol\beta_s$, porque proviene del vector $\boldsymbol b=\beta_0^\star\mathbf 1$.
:::

## Verificación de $Z^T\mathbf 1=\mathbf 0$ y $\mathbf 1^TZ=\mathbf 0^T$

El vector $Z^T\mathbf 1$ tiene $p$ componentes. La componente $j$ se obtiene multiplicando la fila $j$ de $Z^T$, que es la columna $j$ de $Z$, por el vector de unos; es decir, es la suma de la columna $j$ de $Z$:

$$
\left(Z^T\mathbf 1\right)_j=\sum_{i=1}^n z_{ij}=\sum_{i=1}^n\frac{x_{ij}-\bar x_j}{s_j}.
$$

Como $s_j$ no depende de $i$, sale de la suma:

$$
\left(Z^T\mathbf 1\right)_j=\frac1{s_j}\left(\sum_{i=1}^n x_{ij}-\sum_{i=1}^n\bar x_j\right)=\frac1{s_j}\left(\sum_{i=1}^n x_{ij}-n\bar x_j\right).
$$

Por la definición de la media, $n\bar x_j=n\cdot\frac1n\sum_{i=1}^n x_{ij}=\sum_{i=1}^n x_{ij}$. Reemplazando,

$$
\left(Z^T\mathbf 1\right)_j=\frac1{s_j}\left(\sum_{i=1}^n x_{ij}-\sum_{i=1}^n x_{ij}\right)=0 .
$$

Esto vale para cada $j=1,\dots,p$, así que $Z^T\mathbf 1=\mathbf 0$. Transponiendo, $\mathbf 1^TZ=\left(Z^T\mathbf 1\right)^T=\mathbf 0^T$.

En palabras: cada columna de $Z$ suma cero porque a cada predictor se le restó su media.

:::{admonition} Observación
:class: warning

$Z^T\mathbf 1$ es un vector columna de tamaño $p\times1$ y $\mathbf 1^TZ$ es un vector fila de tamaño $1\times p$, así que escribir $Z^T\mathbf 1=\mathbf 1^TZ$ es un abuso de notación. Lo preciso es $Z^T\mathbf 1=\mathbf 0$ y $\mathbf 1^TZ=\mathbf 0^T$.
:::

## Verificación de la ecuación (3.8)

Derivamos la expresión expandida término por término respecto a $\beta_0^\star$. Los términos que no contienen $\beta_0^\star$ tienen derivada cero, y en los demás $\beta_0^\star$ aparece multiplicado por una constante o al cuadrado:

| Término | Derivada respecto a $\beta_0^\star$ |
|---|---|
| $\boldsymbol y^T\boldsymbol y$ | $0$ |
| $-2\beta_0^\star\,\boldsymbol y^T\mathbf 1$ | $-2\boldsymbol y^T\mathbf 1$ |
| $-2\boldsymbol y^TZ\boldsymbol\beta_s$ | $0$ |
| $2\beta_0^\star\,\mathbf 1^TZ\boldsymbol\beta_s$ | $2\,\mathbf 1^TZ\boldsymbol\beta_s$ |
| $n(\beta_0^\star)^2$ | $2n\beta_0^\star$ |
| $\boldsymbol\beta_s^TZ^TZ\boldsymbol\beta_s$ | $0$ |
| $\lambda\boldsymbol\beta_s^T\boldsymbol\beta_s$ | $0$ |

Sumando,

$$
\frac{\partial S_\lambda}{\partial\beta_0^\star}=-2\boldsymbol y^T\mathbf 1+2\,\mathbf 1^TZ\boldsymbol\beta_s+2n\beta_0^\star .
$$

Ahora usamos dos hechos:

- $\boldsymbol y^T\mathbf 1=\sum_{i=1}^n y_i=n\bar y$, por la definición de la media;
- $\mathbf 1^TZ=\mathbf 0^T$, por la sección anterior, así que $2\,\mathbf 1^TZ\boldsymbol\beta_s=0$.

Con esto,

$$
\frac{\partial S_\lambda(\beta_0^\star,\boldsymbol\beta_s)}{\partial\beta_0^\star}=-2n\bar y+2n\beta_0^\star ,
$$

que es la ecuación (3.8). Al igualarla a cero y dividir entre $2n$ se obtiene

$$
\hat\beta_0^\star=\bar y .
$$

Vale la pena notar por qué las notas piden verificar $Z^T\mathbf 1=\mathbf 0$ justo en este punto: sin esa propiedad sobraría el término $2\,\mathbf 1^TZ\boldsymbol\beta_s$, y el intercepto dependería de los demás coeficientes. Gracias a la estandarización, el intercepto se estima por separado y es simplemente la media de la respuesta.

## La forma cuadrática en sumatorias

Sea $A\in\mathbb R^{p\times p}$ y $\boldsymbol x\in\mathbb R^p$. Calculamos $\boldsymbol x^TA\boldsymbol x$ en dos pasos, de izquierda a derecha.

**Paso 1.** $\boldsymbol x^TA$ es un vector fila de tamaño $1\times p$. Su componente $j$ se obtiene multiplicando $\boldsymbol x^T$ por la columna $j$ de $A$:

$$
\left(\boldsymbol x^TA\right)_j=x_1A_{1j}+x_2A_{2j}+\cdots+x_pA_{pj}=\sum_{i=1}^p x_iA_{ij}.
$$

**Paso 2.** Al multiplicar ese vector fila por el vector columna $\boldsymbol x$, se multiplican las componentes correspondientes y se suman:

$$
\boldsymbol x^TA\boldsymbol x=\left(\boldsymbol x^TA\right)\boldsymbol x=\sum_{j=1}^p\left(\boldsymbol x^TA\right)_jx_j=\sum_{j=1}^p\left(\sum_{i=1}^p x_iA_{ij}\right)x_j ,
$$

que es la expresión de las notas. Como las sumas son finitas, también se puede escribir como la doble suma $\sum_{i=1}^p\sum_{j=1}^p x_iA_{ij}x_j$.

:::{admonition} Observación
:class: warning

En la matriz que aparece en las notas, la fila $(A_{31},A_{32},\dots,A_{3p})$ está ubicada después de los puntos suspensivos verticales. Debería ir justo debajo de la fila 2, con los puntos suspensivos entre la fila 3 y la fila $p$.
:::

## Derivadas que se usan en las demostraciones

Las notas indican que, para derivar respecto a $\boldsymbol\beta_s$, primero hay que estudiar la derivada de la forma cuadrática. Usamos la convención de que la derivada de una función escalar respecto a un vector es el vector de sus derivadas parciales (el gradiente).

### Derivada de un término lineal

Sea $\boldsymbol a\in\mathbb R^p$ constante. Como $\boldsymbol a^T\boldsymbol x=\sum_{i=1}^p a_ix_i$, al derivar respecto a $x_k$ solo sobrevive el término con $i=k$:

$$
\frac{\partial}{\partial x_k}\sum_{i=1}^p a_ix_i=a_k
\qquad\Longrightarrow\qquad
\frac{\partial\left(\boldsymbol a^T\boldsymbol x\right)}{\partial\boldsymbol x}=\boldsymbol a .
$$

### Derivada de la forma cuadrática

Partimos de la doble suma $\sum_{i=1}^p\sum_{j=1}^p x_iA_{ij}x_j$ y derivamos respecto a una componente $x_k$. Los términos en los que no aparece $x_k$ tienen derivada cero. $x_k$ aparece en dos situaciones:

- cuando $i=k$: términos $x_kA_{kj}x_j$, cuya derivada respecto a $x_k$ es $A_{kj}x_j$;
- cuando $j=k$: términos $x_iA_{ik}x_k$, cuya derivada respecto a $x_k$ es $x_iA_{ik}$.

(El término con $i=j=k$, que es $A_{kk}x_k^2$, tiene derivada $2A_{kk}x_k$, y queda contado exactamente una vez en cada una de las dos sumas.) Por lo tanto,

$$
\frac{\partial\left(\boldsymbol x^TA\boldsymbol x\right)}{\partial x_k}=\sum_{j=1}^p A_{kj}x_j+\sum_{i=1}^p A_{ik}x_i=(A\boldsymbol x)_k+(A^T\boldsymbol x)_k .
$$

Juntando las $p$ componentes en un vector,

$$
\frac{\partial\left(\boldsymbol x^TA\boldsymbol x\right)}{\partial\boldsymbol x}=\left(A+A^T\right)\boldsymbol x .
$$

Si además $A$ es simétrica ($A^T=A$),

$$
\frac{\partial\left(\boldsymbol x^TA\boldsymbol x\right)}{\partial\boldsymbol x}=2A\boldsymbol x .
$$

En ridge las matrices que aparecen son $Z^TZ$ y $Z^TZ+\lambda I$, ambas simétricas, porque $(Z^TZ)^T=Z^T(Z^T)^T=Z^TZ$ y la identidad es simétrica. Por eso en las secciones siguientes se usa directamente la regla $2A\boldsymbol x$.
