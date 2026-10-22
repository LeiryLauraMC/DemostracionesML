# Notación y convenciones

En todo el libro se usan las siguientes convenciones.

## Vectores y matrices

- Los vectores se escriben en negrita y se entienden como vectores **columna**: $\boldsymbol y\in\mathbb R^n$, $\boldsymbol\beta\in\mathbb R^p$.
- $\mathbf 1\in\mathbb R^n$ es el vector de unos y $\mathbf 0$ el vector de ceros.
- $I_p$ (o simplemente $I$) es la matriz identidad de tamaño $p\times p$.
- $A^T$ es la transpuesta de $A$. Se usa con frecuencia que $(AB)^T=B^TA^T$.
- $\|\boldsymbol v\|^2=\boldsymbol v^T\boldsymbol v=\sum_i v_i^2$ es la norma euclidiana al cuadrado, y $\|\boldsymbol v\|_1=\sum_i|v_i|$ es la norma $L^1$.
- $\operatorname{tr}(A)$ es la traza de $A$, es decir, la suma de su diagonal.

**Un escalar es igual a su transpuesta.** Si $\boldsymbol u,\boldsymbol v\in\mathbb R^n$, el producto $\boldsymbol u^T\boldsymbol v$ es un número ($1\times1$), y por lo tanto

$$
\boldsymbol v^T\boldsymbol u=\left(\boldsymbol v^T\boldsymbol u\right)^T=\boldsymbol u^T\boldsymbol v .
$$

Esta propiedad se usa muchas veces para agrupar términos cruzados al expandir productos.

## Datos y modelos

- $n$ (o $m$, o $N$, según el capítulo) es el número de observaciones y $p$ el número de predictores.
- $\bar y=\frac1n\sum_{i=1}^n y_i$ y $\bar x_j=\frac1n\sum_{i=1}^n x_{ij}$ son las medias muestrales.
- $s_j$ es la desviación estándar del predictor $j$.

## Probabilidad

- $\mathcal D$ es la distribución que genera los datos, y $S\sim\mathcal D^m$ indica una muestra de $m$ observaciones independientes con esa distribución.
- $\mathbb E[\cdot]$, $\operatorname{Var}(\cdot)$ y $\operatorname{Cov}(\cdot)$ son la esperanza, la varianza y la matriz de covarianzas.
- $\mathbb 1[A]$ es la función indicadora: vale 1 si $A$ ocurre y 0 si no.
- $\operatorname{sgn}(t)$ es la función signo: $+1$ si $t>0$ y $-1$ si $t<0$.

## Cierre de las demostraciones

El símbolo $\blacksquare$ indica el final de una demostración.
