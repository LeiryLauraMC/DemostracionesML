# Demostración de la ecuación 3.12

:::{admonition} Enunciado (propiedades del estimador ridge)
:class: note

El estimador ridge satisface las siguientes propiedades, que en las notas corresponden a la ecuación (3.12):

$$
\begin{aligned}
\mathbb E(\hat{\boldsymbol\beta}_R)&=\left(X^TX+\lambda I_p\right)^{-1}X^TX\boldsymbol\beta\\
\operatorname{Cov}(\hat{\boldsymbol\beta}_R)&=\sigma^2\left(X^TX+\lambda I_p\right)^{-1}X^TX\left(X^TX+\lambda I_p\right)^{-1}\\
\mathbb E(\hat{\boldsymbol\beta}_R-\boldsymbol\beta)&=-\lambda\left(X^TX+\lambda I_p\right)^{-1}\boldsymbol\beta\\
\mathbb E\left[(\hat{\boldsymbol\beta}_R-\boldsymbol\beta)^T(\hat{\boldsymbol\beta}_R-\boldsymbol\beta)\right]&=\sigma^2\sum_{j=1}^p\frac{l_j}{(l_j+\lambda)^2}+\lambda^2\boldsymbol\beta^T\left(X^TX+\lambda I_p\right)^{-2}\boldsymbol\beta ,
\end{aligned}
$$

donde $l_1,\dots,l_p$ son los autovalores de $X^TX$. El primer término de la última ecuación es la suma de las varianzas de los componentes del estimador ridge, y el segundo es el cuadrado del sesgo.
:::

## Supuestos y notación

- El modelo es $\boldsymbol y=X\boldsymbol\beta+\boldsymbol\varepsilon$, con $X$ **fija** (no aleatoria), $\mathbb E(\boldsymbol\varepsilon)=\mathbf 0$ y $\operatorname{Cov}(\boldsymbol\varepsilon)=\sigma^2I_n$.
- Como $X\boldsymbol\beta$ es constante, la aleatoriedad de $\boldsymbol y$ viene solo de $\boldsymbol\varepsilon$:

$$
\mathbb E(\boldsymbol y)=X\boldsymbol\beta+\mathbb E(\boldsymbol\varepsilon)=X\boldsymbol\beta,\qquad \operatorname{Cov}(\boldsymbol y)=\operatorname{Cov}(\boldsymbol\varepsilon)=\sigma^2 I_n .
$$

- Para no repetir la inversa en cada línea, definimos

$$
W=\left(X^TX+\lambda I_p\right)^{-1},
\qquad\text{de modo que}\qquad
\hat{\boldsymbol\beta}_R=WX^T\boldsymbol y .
$$

  $W$ es **simétrica**: la matriz $X^TX+\lambda I_p$ es simétrica, y la inversa de una matriz simétrica también lo es, porque $(M^{-1})^T=(M^T)^{-1}=M^{-1}$. Por lo tanto, $W^T=W$.

Usaremos dos propiedades de los vectores aleatorios. Si $A$ es una matriz constante,

$$
\mathbb E(A\boldsymbol y)=A\,\mathbb E(\boldsymbol y),\qquad \operatorname{Cov}(A\boldsymbol y)=A\operatorname{Cov}(\boldsymbol y)A^T .
$$

## Propiedad 1. Esperanza del estimador

El estimador $\hat{\boldsymbol\beta}_R=WX^T\boldsymbol y$ es lineal en $\boldsymbol y$, con matriz constante $A=WX^T$. Por la linealidad de la esperanza,

$$
\mathbb E(\hat{\boldsymbol\beta}_R)=WX^T\,\mathbb E(\boldsymbol y)=WX^TX\boldsymbol\beta=\left(X^TX+\lambda I_p\right)^{-1}X^TX\boldsymbol\beta .
$$

Si $\lambda>0$, la matriz $WX^TX$ no es la identidad, así que $\mathbb E(\hat{\boldsymbol\beta}_R)\neq\boldsymbol\beta$ en general: el estimador ridge es **sesgado**. En la Propiedad 3 calculamos exactamente cuánto.

## Propiedad 2. Matriz de covarianzas

Con $A=WX^T$, su transpuesta es $A^T=(WX^T)^T=(X^T)^TW^T=XW$, usando que $W^T=W$. Entonces

$$
\operatorname{Cov}(\hat{\boldsymbol\beta}_R)=A\operatorname{Cov}(\boldsymbol y)A^T=WX^T\left(\sigma^2I_n\right)XW .
$$

Como $\sigma^2$ es un escalar, sale del producto, y $X^TI_nX=X^TX$:

$$
\operatorname{Cov}(\hat{\boldsymbol\beta}_R)=\sigma^2\,WX^TXW=\sigma^2\left(X^TX+\lambda I_p\right)^{-1}X^TX\left(X^TX+\lambda I_p\right)^{-1}.
$$

:::{admonition} Observación
:class: warning

En las notas, esta propiedad aparece escrita como $\operatorname{Cov}(\boldsymbol\beta_R)$, sin el gorro. Como $\boldsymbol\beta$ es un parámetro fijo, su covarianza sería cero; la expresión se refiere al estimador $\hat{\boldsymbol\beta}_R$.
:::

## Propiedad 3. Sesgo

La idea es reescribir $X^TX$ en términos de $W^{-1}=X^TX+\lambda I_p$. Restando $\lambda I_p$ a ambos lados,

$$
X^TX=W^{-1}-\lambda I_p .
$$

Reemplazando en la Propiedad 1,

$$
\begin{aligned}
\mathbb E(\hat{\boldsymbol\beta}_R)-\boldsymbol\beta
&=WX^TX\boldsymbol\beta-\boldsymbol\beta
=W\left(W^{-1}-\lambda I_p\right)\boldsymbol\beta-\boldsymbol\beta\\
&=WW^{-1}\boldsymbol\beta-\lambda W\boldsymbol\beta-\boldsymbol\beta
=\boldsymbol\beta-\lambda W\boldsymbol\beta-\boldsymbol\beta\\
&=-\lambda\left(X^TX+\lambda I_p\right)^{-1}\boldsymbol\beta .
\end{aligned}
$$

Como $\boldsymbol\beta$ es constante, $\mathbb E(\hat{\boldsymbol\beta}_R-\boldsymbol\beta)=\mathbb E(\hat{\boldsymbol\beta}_R)-\boldsymbol\beta$, que es lo que se quería. En adelante llamamos $\boldsymbol b=-\lambda W\boldsymbol\beta$ a este sesgo. Cuando $\lambda\to0$, $\boldsymbol b\to\mathbf 0$ y se recupera la insesgadez del estimador de mínimos cuadrados.

## Propiedad 4. Error cuadrático medio

Queremos demostrar que

$$
\mathbb E\left[(\hat{\boldsymbol\beta}_R-\boldsymbol\beta)^T(\hat{\boldsymbol\beta}_R-\boldsymbol\beta)\right]=\sigma^2\sum_{j=1}^p\frac{l_j}{(l_j+\lambda)^2}+\lambda^2\boldsymbol\beta^T\left(X^TX+\lambda I_p\right)^{-2}\boldsymbol\beta .
$$

### Descomposición en varianza más sesgo al cuadrado

Sumamos y restamos $\mathbb E(\hat{\boldsymbol\beta}_R)$ dentro del error:

$$
\hat{\boldsymbol\beta}_R-\boldsymbol\beta=\underbrace{\left(\hat{\boldsymbol\beta}_R-\mathbb E\hat{\boldsymbol\beta}_R\right)}_{\boldsymbol u}+\underbrace{\left(\mathbb E\hat{\boldsymbol\beta}_R-\boldsymbol\beta\right)}_{\boldsymbol b} .
$$

El vector $\boldsymbol u$ es aleatorio y tiene esperanza cero, porque $\mathbb E(\boldsymbol u)=\mathbb E\hat{\boldsymbol\beta}_R-\mathbb E\hat{\boldsymbol\beta}_R=\mathbf 0$. El vector $\boldsymbol b$ es constante: es el sesgo de la Propiedad 3. Expandiendo el producto y usando que $\boldsymbol u^T\boldsymbol b=\boldsymbol b^T\boldsymbol u$ (es un escalar),

$$
(\boldsymbol u+\boldsymbol b)^T(\boldsymbol u+\boldsymbol b)=\boldsymbol u^T\boldsymbol u+2\boldsymbol b^T\boldsymbol u+\boldsymbol b^T\boldsymbol b .
$$

Tomando esperanza, y como $\boldsymbol b$ es constante,

$$
\mathbb E\left[(\boldsymbol u+\boldsymbol b)^T(\boldsymbol u+\boldsymbol b)\right]
=\mathbb E(\boldsymbol u^T\boldsymbol u)+2\,\boldsymbol b^T\,\mathbb E(\boldsymbol u)+\boldsymbol b^T\boldsymbol b
=\mathbb E(\boldsymbol u^T\boldsymbol u)+\boldsymbol b^T\boldsymbol b .
$$

Falta calcular cada uno de estos dos términos.

### Primer término: la suma de las varianzas

$\boldsymbol u^T\boldsymbol u$ es un escalar, y un escalar es igual a su traza. Además, la traza tiene la propiedad cíclica $\operatorname{tr}(AB)=\operatorname{tr}(BA)$, de modo que $\operatorname{tr}(\boldsymbol u^T\boldsymbol u)=\operatorname{tr}(\boldsymbol u\boldsymbol u^T)$. Como la traza es lineal, conmuta con la esperanza:

$$
\mathbb E(\boldsymbol u^T\boldsymbol u)=\mathbb E\left[\operatorname{tr}(\boldsymbol u\boldsymbol u^T)\right]=\operatorname{tr}\,\mathbb E(\boldsymbol u\boldsymbol u^T)=\operatorname{tr}\operatorname{Cov}(\hat{\boldsymbol\beta}_R)=\sum_{j=1}^p\operatorname{Var}(\hat\beta_{R,j}) .
$$

La penúltima igualdad usa la definición de covarianza, $\operatorname{Cov}(\hat{\boldsymbol\beta}_R)=\mathbb E\left[(\hat{\boldsymbol\beta}_R-\mathbb E\hat{\boldsymbol\beta}_R)(\hat{\boldsymbol\beta}_R-\mathbb E\hat{\boldsymbol\beta}_R)^T\right]=\mathbb E(\boldsymbol u\boldsymbol u^T)$, y la última que la diagonal de la matriz de covarianzas contiene las varianzas de cada componente. Esto confirma la interpretación de las notas: el primer término es la **suma de las varianzas de los componentes** del estimador.

Para calcular esa traza usamos la **descomposición espectral**. Como $X^TX$ es simétrica y semidefinida positiva, existe una matriz ortogonal $Q$ tal que

$$
X^TX=Q\Lambda Q^T,\qquad Q^TQ=QQ^T=I_p,\qquad \Lambda=\operatorname{diag}(l_1,\dots,l_p),\quad l_j\ge0 .
$$

Escribiendo $\lambda I_p=\lambda QQ^T$, la matriz que invertimos se diagonaliza con la misma $Q$:

$$
X^TX+\lambda I_p=Q\Lambda Q^T+\lambda QQ^T=Q(\Lambda+\lambda I_p)Q^T .
$$

La inversa de $QMQ^T$ es $QM^{-1}Q^T$ (basta multiplicarlas y usar $Q^TQ=I$), así que

$$
W=Q(\Lambda+\lambda I_p)^{-1}Q^T .
$$

Sustituyendo en la covarianza de la Propiedad 2,

$$
\begin{aligned}
WX^TXW&=Q(\Lambda+\lambda I_p)^{-1}Q^T\;Q\Lambda Q^T\;Q(\Lambda+\lambda I_p)^{-1}Q^T\\
&=Q(\Lambda+\lambda I_p)^{-1}\Lambda(\Lambda+\lambda I_p)^{-1}Q^T\\
&=Q\,\operatorname{diag}\!\left(\frac{l_j}{(l_j+\lambda)^2}\right)Q^T ,
\end{aligned}
$$

donde en la segunda línea se usó $Q^TQ=I_p$ dos veces, y en la tercera que el producto de matrices diagonales es diagonal, con entradas $\frac{1}{l_j+\lambda}\cdot l_j\cdot\frac{1}{l_j+\lambda}$.

Por la propiedad cíclica, la traza no cambia bajo este tipo de transformación: $\operatorname{tr}(QMQ^T)=\operatorname{tr}(MQ^TQ)=\operatorname{tr}(M)$. Por lo tanto,

$$
\operatorname{tr}\operatorname{Cov}(\hat{\boldsymbol\beta}_R)=\sigma^2\operatorname{tr}\left(WX^TXW\right)=\sigma^2\sum_{j=1}^p\frac{l_j}{(l_j+\lambda)^2}.
$$

### Segundo término: el sesgo al cuadrado

Con $\boldsymbol b=-\lambda W\boldsymbol\beta$, su transpuesta es $\boldsymbol b^T=-\lambda\boldsymbol\beta^TW^T=-\lambda\boldsymbol\beta^TW$. Entonces

$$
\boldsymbol b^T\boldsymbol b=(-\lambda)^2\,\boldsymbol\beta^TWW\boldsymbol\beta=\lambda^2\boldsymbol\beta^TW^2\boldsymbol\beta=\lambda^2\boldsymbol\beta^T\left(X^TX+\lambda I_p\right)^{-2}\boldsymbol\beta .
$$

### Conclusión

Sumando los dos términos,

$$
\mathbb E\left[(\hat{\boldsymbol\beta}_R-\boldsymbol\beta)^T(\hat{\boldsymbol\beta}_R-\boldsymbol\beta)\right]=\underbrace{\sigma^2\sum_{j=1}^p\frac{l_j}{(l_j+\lambda)^2}}_{\text{varianza total}}+\underbrace{\lambda^2\boldsymbol\beta^T\left(X^TX+\lambda I_p\right)^{-2}\boldsymbol\beta}_{\text{sesgo}^2} . \qquad\blacksquare
$$

## Interpretación: el compromiso entre sesgo y varianza

- **Comparación con mínimos cuadrados ($\lambda=0$).** Si $X^TX$ es invertible, el error cuadrático medio de mínimos cuadrados es $\sigma^2\sum_j\frac{1}{l_j}$, que se dispara si algún $l_j$ es cercano a cero, como ocurre con la matriz de Hilbert. Con ridge, cada término de la varianza cumple $\frac{l_j}{(l_j+\lambda)^2}<\frac{l_j}{l_j^2}=\frac{1}{l_j}$, porque $(l_j+\lambda)^2>l_j^2$; así que la varianza total siempre es menor.
- **Efecto de $\lambda$.** Al aumentar $\lambda$, el término de varianza disminuye y el sesgo al cuadrado aumenta desde cero.
- **Resultado de Hoerl y Kennard** {cite:p}`hoerl1970`. Siempre existe algún $\lambda>0$ para el cual el error cuadrático medio de ridge es estrictamente menor que el de mínimos cuadrados. Esta es la justificación teórica principal de la regresión ridge.
