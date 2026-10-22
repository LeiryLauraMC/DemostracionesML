# Verificación del centrado de la respuesta

:::{admonition} Enunciado
:class: note

Para obtener la estimación ridge del vector de coeficientes $\boldsymbol\beta_s=(\hat\beta_1,\dots,\hat\beta_p)$, las notas observan que, al reemplazar $\hat\beta_0^\star=\bar y$ en la ecuación (3.6), se tiene

$$
\boldsymbol y=\bar y\mathbf 1+Z\boldsymbol\beta_s+\boldsymbol\varepsilon ,
$$

y entonces $\boldsymbol y-\bar y\mathbf 1$ está centrando los datos con respecto a la variable respuesta.
:::

Recordemos el modelo (3.6):

$$
\boldsymbol y=\beta_0^\star\mathbf 1+Z\boldsymbol\beta_s+\boldsymbol\varepsilon ,
$$

con $Z$ la matriz de predictores estandarizados, que cumple $Z^T\mathbf 1=\mathbf 0$ (ver {doc}`desarrollo_3_7_y_3_8`).

## Sustitución directa

En la ecuación (3.8) se obtuvo $\hat\beta_0^\star=\bar y$. Si en (3.6) reemplazamos $\beta_0^\star$ por ese valor,

$$
\boldsymbol y=\bar y\,\mathbf 1+Z\boldsymbol\beta_s+\boldsymbol\varepsilon ,
$$

y pasando $\bar y\mathbf 1$ al lado izquierdo,

$$
\boldsymbol y-\bar y\,\mathbf 1=Z\boldsymbol\beta_s+\boldsymbol\varepsilon .
$$

## $\boldsymbol y-\bar y\mathbf 1$ es la respuesta centrada

El vector $\bar y\mathbf 1$ tiene todas sus componentes iguales a $\bar y$, así que la componente $i$ de $\boldsymbol y-\bar y\mathbf 1$ es $y_i-\bar y$: a cada respuesta se le resta la media. Veamos que el resultado tiene media cero. Su suma es

$$
\mathbf 1^T\left(\boldsymbol y-\bar y\mathbf 1\right)=\mathbf 1^T\boldsymbol y-\bar y\,\mathbf 1^T\mathbf 1=\sum_{i=1}^n y_i-n\bar y .
$$

Como $n\bar y=\sum_{i=1}^n y_i$, la suma es cero, y por lo tanto también su media. Esto es justamente lo que significa "centrar los datos con respecto a la variable respuesta".

## Los dos lados de la ecuación tienen media cero

Si la igualdad $\boldsymbol y-\bar y\mathbf 1=Z\boldsymbol\beta_s+\boldsymbol\varepsilon$ es coherente, el lado derecho también debería sumar cero. Para la parte $Z\boldsymbol\beta_s$ esto se cumple, porque $\mathbf 1^TZ=\mathbf 0^T$:

$$
\mathbf 1^T\left(Z\boldsymbol\beta_s\right)=\left(\mathbf 1^TZ\right)\boldsymbol\beta_s=\mathbf 0^T\boldsymbol\beta_s=0 .
$$

Sin embargo, la suma de los errores $\mathbf 1^T\boldsymbol\varepsilon$ no tiene por qué ser cero en una muestra concreta. Esto nos lleva a precisar el término de error.

## Precisión sobre el término de error

En (3.6), $\beta_0^\star$ es el parámetro **verdadero**, mientras que $\bar y$ es su **estimador**; en general no son iguales. Para ver cuál es la diferencia, multiplicamos (3.6) por $\frac1n\mathbf 1^T$:

$$
\bar y=\frac1n\mathbf 1^T\boldsymbol y=\frac1n\left(\beta_0^\star\,\mathbf 1^T\mathbf 1+\mathbf 1^TZ\boldsymbol\beta_s+\mathbf 1^T\boldsymbol\varepsilon\right)=\frac1n\left(n\beta_0^\star+0+\sum_{i=1}^n\varepsilon_i\right)=\beta_0^\star+\bar\varepsilon ,
$$

donde $\bar\varepsilon=\frac1n\sum_{i=1}^n\varepsilon_i$ es el promedio de los errores. Ahora restamos $\bar y\mathbf 1=(\beta_0^\star+\bar\varepsilon)\mathbf 1$ a ambos lados de (3.6):

$$
\boldsymbol y-\bar y\mathbf 1=\beta_0^\star\mathbf 1+Z\boldsymbol\beta_s+\boldsymbol\varepsilon-\beta_0^\star\mathbf 1-\bar\varepsilon\,\mathbf 1
=Z\boldsymbol\beta_s+\left(\boldsymbol\varepsilon-\bar\varepsilon\,\mathbf 1\right).
$$

En sentido estricto, entonces, el error del modelo centrado no es $\boldsymbol\varepsilon$ sino $\boldsymbol\varepsilon-\bar\varepsilon\mathbf 1$, es decir, el error centrado (que sí suma cero, como debía ser). Su matriz de covarianzas es $\sigma^2\left(I-\frac1n\mathbf 1\mathbf 1^T\right)$ en lugar de $\sigma^2I$. Escribir simplemente $+\boldsymbol\varepsilon$, como en las notas, es una simplificación.

## Por qué la simplificación no afecta al estimador ridge

El estimador ridge, $\left(Z^TZ+\lambda I\right)^{-1}Z^T\boldsymbol y$, solo depende de la respuesta a través del producto $Z^T\boldsymbol y$. Al multiplicar el error centrado por $Z^T$ y usar $Z^T\mathbf 1=\mathbf 0$,

$$
Z^T\left(\boldsymbol\varepsilon-\bar\varepsilon\mathbf 1\right)=Z^T\boldsymbol\varepsilon-\bar\varepsilon\,Z^T\mathbf 1=Z^T\boldsymbol\varepsilon .
$$

Es decir, para el estimador es exactamente lo mismo trabajar con $\boldsymbol\varepsilon$ que con $\boldsymbol\varepsilon-\bar\varepsilon\mathbf 1$. Por eso, como dicen las notas más adelante, se puede considerar el modelo centrado "sin pérdida de generalidad".

## Conexión con la ecuación (3.11)

Una vez centrada, la nueva respuesta cumple $\sum_i y_i=0$, que es la primera de las igualdades (3.11). En consecuencia, si se vuelve a estimar el intercepto con los datos centrados, se obtiene $\hat\beta_0^\star=0$. Esto se desarrolla en la siguiente sección.
