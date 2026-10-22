# Demostración de la ecuación 3.11

:::{admonition} Enunciado
:class: note

Estandarizando las variables predictoras y la respuesta en el modelo inicial (3.3), mediante $y_i-\bar y$ y $(x_{ij}-\bar x_j)/s_j$, se tienen las siguientes igualdades:

$$
\sum_{i=1}^n y_i=0,\quad \sum_{i=1}^n x_{ij}=0,\ j=1,\dots,p,\quad \sum_{i=1}^n x_{ij}^2=n. \tag{3.11}
$$

Entonces $\beta_0^\star=\bar y=\frac1n\sum_{i=1}^n y_i=0$. En virtud de estas igualdades y de la ecuación (3.5), se puede considerar sin pérdida de generalidad el modelo $\boldsymbol y=X\boldsymbol\beta+\boldsymbol\varepsilon$, con $X\in\mathbb R^{n\times p}$, $\boldsymbol\beta\in\mathbb R^p$, $E(\boldsymbol\varepsilon)=\mathbf 0$ y $\operatorname{Cov}(\boldsymbol\varepsilon)=\sigma^2I$. Minimizando $S_\lambda(\boldsymbol\beta)=(\boldsymbol y-X\boldsymbol\beta)^T(\boldsymbol y-X\boldsymbol\beta)+\lambda\boldsymbol\beta^T\boldsymbol\beta$ de forma análoga a (3.7), se obtiene el estimador ridge

$$
\hat{\boldsymbol\beta}_R=\left(X^TX+\lambda I_p\right)^{-1}X^T\boldsymbol y,\qquad \lambda>0 .
$$
:::

## Variables transformadas

Partimos de los datos originales y definimos

$$
\tilde y_i=y_i-\bar y,\qquad \tilde x_{ij}=\frac{x_{ij}-\bar x_j}{s_j},
\qquad\text{con}\quad \bar y=\frac1n\sum_{i=1}^n y_i,\quad \bar x_j=\frac1n\sum_{i=1}^n x_{ij}.
$$

Las igualdades (3.11) se refieren a las variables ya transformadas. Para verificarlas, las escribimos en términos de los datos originales.

## Primera igualdad: la respuesta centrada suma cero

$$
\sum_{i=1}^n\tilde y_i=\sum_{i=1}^n (y_i-\bar y)=\sum_{i=1}^n y_i-\sum_{i=1}^n\bar y=\sum_{i=1}^n y_i-n\bar y .
$$

Como $\bar y$ no depende de $i$, sumarlo $n$ veces da $n\bar y$, y por la definición de la media, $n\bar y=n\cdot\frac1n\sum_{i=1}^n y_i=\sum_{i=1}^n y_i$. Entonces

$$
\sum_{i=1}^n\tilde y_i=\sum_{i=1}^n y_i-\sum_{i=1}^n y_i=0 .
$$

## Segunda igualdad: cada predictor estandarizado suma cero

Para cada $j=1,\dots,p$, el factor $\frac1{s_j}$ no depende de $i$, así que sale de la suma:

$$
\sum_{i=1}^n\tilde x_{ij}=\sum_{i=1}^n \frac{x_{ij}-\bar x_j}{s_j}
=\frac{1}{s_j}\left(\sum_{i=1}^n x_{ij}-n\bar x_j\right)
=\frac{1}{s_j}\left(\sum_{i=1}^n x_{ij}-\sum_{i=1}^n x_{ij}\right)=0 .
$$

Es el mismo argumento de la primera igualdad, aplicado a cada columna. Esta es también la propiedad $Z^T\mathbf 1=\mathbf 0$ verificada en {doc}`desarrollo_3_7_y_3_8`.

## Tercera igualdad: la suma de cuadrados de cada predictor es $n$

Elevando al cuadrado y sacando $\frac1{s_j^2}$ de la suma,

$$
\sum_{i=1}^n\tilde x_{ij}^2=\sum_{i=1}^n\left(\frac{x_{ij}-\bar x_j}{s_j}\right)^2
=\frac{1}{s_j^2}\sum_{i=1}^n (x_{ij}-\bar x_j)^2 .
$$

El resultado depende de cómo se defina la desviación estándar $s_j$.

**Si $s_j$ se define con divisor $n$** (desviación estándar poblacional),

$$
s_j^2=\frac1n\sum_{i=1}^n (x_{ij}-\bar x_j)^2
\qquad\Longrightarrow\qquad
\sum_{i=1}^n (x_{ij}-\bar x_j)^2=n\,s_j^2 ,
$$

y reemplazando,

$$
\sum_{i=1}^n\tilde x_{ij}^2=\frac{n\,s_j^2}{s_j^2}=n .
$$

**Si $s_j$ se define con divisor $n-1$** (desviación estándar muestral), el mismo cálculo da $\sum_i\tilde x_{ij}^2=\frac{(n-1)s_j^2}{s_j^2}=n-1$.

:::{admonition} Observación
:class: warning

La tercera igualdad de (3.11) es cierta **solo si $s_j$ se define con divisor $n$**. Conviene tenerlo presente al programar, porque funciones como `sd()` en R o `std(ddof=1)` en Python usan $n-1$. Esto no cambia la forma del estimador ridge; solo cambia la escala en la que se interpreta $\lambda$.
:::

**Interpretación matricial.** Si llamamos $X$ a la matriz de predictores estandarizados, la segunda y la tercera igualdad dicen que

$$
\mathbf 1^TX=\mathbf 0^T\qquad\text{y}\qquad \left(X^TX\right)_{jj}=\sum_{i=1}^n x_{ij}^2=n\quad\text{para todo } j .
$$

En consecuencia, $\frac1n X^TX$ tiene unos en la diagonal y, fuera de ella, las covarianzas entre predictores estandarizados: es la **matriz de correlaciones** de los predictores.

## Consecuencia: el intercepto es cero

En {doc}`ecuacion_3_7` (Paso 2) se demostró que, cuando los predictores están centrados, el estimador del intercepto es

$$
\hat\beta_0^\star=\bar y=\frac1n\sum_{i=1}^n y_i .
$$

Si ahora la respuesta también está centrada, la primera igualdad de (3.11) dice que su suma es cero, y entonces

$$
\hat\beta_0^\star=\frac1n\cdot 0=0 .
$$

## Por qué se puede usar $\boldsymbol y=X\boldsymbol\beta+\boldsymbol\varepsilon$ sin pérdida de generalidad

Partimos del modelo (3.6), $\boldsymbol y=\beta_0^\star\mathbf 1+Z\boldsymbol\beta_s+\boldsymbol\varepsilon$, donde $Z$ ya es la matriz de predictores estandarizados. En {doc}`centrado_respuesta` vimos que al centrar la respuesta se obtiene

$$
\boldsymbol y-\bar y\mathbf 1=Z\boldsymbol\beta_s+\left(\boldsymbol\varepsilon-\bar\varepsilon\mathbf 1\right),
$$

y que, para efectos del estimador, el error centrado se comporta igual que $\boldsymbol\varepsilon$. El intercepto desaparece. Si ahora renombramos

$$
\boldsymbol y\leftarrow\boldsymbol y-\bar y\mathbf 1,\qquad X\leftarrow Z,\qquad \boldsymbol\beta\leftarrow\boldsymbol\beta_s ,
$$

el modelo queda exactamente $\boldsymbol y=X\boldsymbol\beta+\boldsymbol\varepsilon$, sin intercepto. No se ha perdido información: es el mismo modelo, escrito en variables estandarizadas. Una vez estimado $\hat{\boldsymbol\beta}_s$, los coeficientes en la escala original se recuperan deshaciendo las definiciones de (3.5), $\beta_j^\star=s_j\beta_j$ y $\beta_0^\star=\beta_0+\sum_j\beta_j\bar x_j$, con $\hat\beta_0^\star=\bar y$ calculado con la respuesta original:

$$
\hat\beta_j=\frac{\hat\beta_j^\star}{s_j},\qquad \hat\beta_0=\bar y-\sum_{j=1}^p\hat\beta_j\,\bar x_j .
$$

:::{admonition} Por qué estandarizar no es solo una comodidad
:class: tip

La penalización $\lambda\|\boldsymbol\beta\|^2$ **no es invariante a la escala** de los predictores. Si un predictor estuviera medido en milímetros y otro en kilómetros, sus coeficientes tendrían tamaños muy distintos y se penalizarían de forma muy desigual. Al estandarizar, todos los coeficientes quedan en la misma escala y la penalización los trata por igual.
:::

## Derivación del estimador $\hat{\boldsymbol\beta}_R$

Se minimiza

$$
S_\lambda(\boldsymbol\beta)=(\boldsymbol y-X\boldsymbol\beta)^T(\boldsymbol y-X\boldsymbol\beta)+\lambda\boldsymbol\beta^T\boldsymbol\beta .
$$

**Expansión.** Desarrollando el producto,

$$
S_\lambda(\boldsymbol\beta)=\boldsymbol y^T\boldsymbol y-\boldsymbol y^TX\boldsymbol\beta-\boldsymbol\beta^TX^T\boldsymbol y+\boldsymbol\beta^TX^TX\boldsymbol\beta+\lambda\boldsymbol\beta^T\boldsymbol\beta .
$$

Como $\boldsymbol y^TX\boldsymbol\beta$ es un escalar, es igual a su transpuesta $\boldsymbol\beta^TX^T\boldsymbol y$, y los dos términos del medio se suman. Escribiendo $\lambda\boldsymbol\beta^T\boldsymbol\beta=\boldsymbol\beta^T(\lambda I_p)\boldsymbol\beta$,

$$
S_\lambda(\boldsymbol\beta)=\boldsymbol y^T\boldsymbol y-2\boldsymbol\beta^TX^T\boldsymbol y+\boldsymbol\beta^T\left(X^TX+\lambda I_p\right)\boldsymbol\beta .
$$

**Condición de primer orden.** El primer término no depende de $\boldsymbol\beta$. El segundo es lineal, con $\boldsymbol a=X^T\boldsymbol y$, y su derivada es $-2X^T\boldsymbol y$. El tercero es una forma cuadrática con la matriz simétrica $X^TX+\lambda I_p$, y su derivada es $2(X^TX+\lambda I_p)\boldsymbol\beta$. Igualando a cero,

$$
\nabla S_\lambda(\boldsymbol\beta)=-2X^T\boldsymbol y+2\left(X^TX+\lambda I_p\right)\boldsymbol\beta=\mathbf 0 ,
$$

de donde

$$
\left(X^TX+\lambda I_p\right)\boldsymbol\beta=X^T\boldsymbol y .
$$

**Invertibilidad.** Para cualquier $\boldsymbol v$, $\boldsymbol v^TX^TX\boldsymbol v=\|X\boldsymbol v\|^2\ge0$, así que $X^TX$ es semidefinida positiva y sus valores propios cumplen $d_j^2\ge0$. Los valores propios de $X^TX+\lambda I_p$ son $d_j^2+\lambda\ge\lambda>0$ (el argumento completo está en {doc}`ecuacion_3_7`, Paso 4). Luego la matriz es definida positiva e invertible para todo $\lambda>0$, **incluso si $X^TX$ es singular**, como ocurre cuando $p>n$ o hay multicolinealidad exacta. Despejando,

$$
\boxed{\ \hat{\boldsymbol\beta}_R=\left(X^TX+\lambda I_p\right)^{-1}X^T\boldsymbol y,\qquad \lambda>0\ }
$$

**Condición de segundo orden.** La Hessiana es

$$
\nabla^2 S_\lambda=2\left(X^TX+\lambda I_p\right),
$$

que es definida positiva. Por lo tanto, $S_\lambda$ es estrictamente convexa y $\hat{\boldsymbol\beta}_R$ es su **único mínimo global**. $\blacksquare$
