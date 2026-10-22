# Estimación de los parámetros lasso

:::{admonition} Enunciado (ejercicio para el lector)
:class: note

Encontrar los parámetros de estimación lasso, tal como se realizó en el caso de la regresión ridge. Se sugiere investigar sobre el algoritmo LARS (*Least Angle Regression*) de Efron et al. (2004).
:::

La diferencia fundamental con ridge es que **lasso no tiene, en general, una fórmula cerrada** para el estimador. Por eso, "encontrar los parámetros" significa aquí:

1. plantear el problema y obtener el intercepto;
2. deducir las condiciones que caracterizan la solución;
3. resolverlo de forma cerrada en el caso de predictores ortogonales;
4. presentar los algoritmos que lo resuelven en general: descenso por coordenadas y LARS.

## Planteamiento

Con la misma notación de ridge (ecuación 3.7), lasso {cite:p}`tibshirani1996` reemplaza la penalización $L^2$ por una penalización $L^1$:

$$
S_\lambda(\beta_0^\star,\boldsymbol\beta_s)=(\boldsymbol y-\beta_0^\star\mathbf 1-Z\boldsymbol\beta_s)^T(\boldsymbol y-\beta_0^\star\mathbf 1-Z\boldsymbol\beta_s)+\lambda\sum_{j=1}^p|\beta_{s,j}|,\qquad\lambda>0 .
$$

**El intercepto.** La penalización no contiene a $\beta_0^\star$, y $S_\lambda$ sí es derivable respecto a él. La derivada es exactamente la misma que en ridge, porque la expansión de la parte cuadrática no cambia (ver {doc}`desarrollo_3_7_y_3_8`):

$$
\frac{\partial S_\lambda}{\partial\beta_0^\star}=-2n\bar y+2n\beta_0^\star=0\qquad\Longrightarrow\qquad \hat\beta_0^\star=\bar y .
$$

Con el mismo argumento de {doc}`ecuacion_3_11` (respuesta centrada y predictores estandarizados), podemos trabajar sin pérdida de generalidad con

$$
S_\lambda(\boldsymbol\beta)=(\boldsymbol y-X\boldsymbol\beta)^T(\boldsymbol y-X\boldsymbol\beta)+\lambda\|\boldsymbol\beta\|_1,\qquad \|\boldsymbol\beta\|_1=\sum_{j=1}^p|\beta_j|,
$$

donde $\sum_i y_i=0$, $\sum_i x_{ij}=0$ y $\sum_i x_{ij}^2=n$. La última igualdad dice que cada columna $X_j$ de $X$ cumple $X_j^TX_j=n$, y la usaremos varias veces.

**Por qué no hay fórmula cerrada.** La función $|\beta_j|$ no es derivable en $\beta_j=0$ (tiene un "pico"), así que no podemos simplemente igualar el gradiente a cero y despejar, como hicimos en ridge. Aun así, $S_\lambda$ es **convexa**: es la suma de una función cuadrática convexa y una norma, que también es convexa. En una función convexa todo mínimo local es global, y el mínimo se puede caracterizar con **subgradientes**, que generalizan la derivada a funciones con picos.

## Condiciones de optimalidad

### El subgradiente del valor absoluto

Para una función convexa $g$, un número $c$ es un **subgradiente** de $g$ en $\beta_0$ si la recta de pendiente $c$ que pasa por $(\beta_0,g(\beta_0))$ queda por debajo de la gráfica: $g(\beta)\ge g(\beta_0)+c(\beta-\beta_0)$ para todo $\beta$. Donde $g$ es derivable, el único subgradiente es la derivada. Para el valor absoluto:

- si $\beta\neq0$, la función es derivable y su derivada es $\operatorname{sign}(\beta)$;
- si $\beta=0$, cualquier recta de pendiente entre $-1$ y $1$ que pase por el origen queda por debajo de $|\beta|$.

Por lo tanto, el conjunto de subgradientes (el **subdiferencial**) es

$$
\partial|\beta|=\begin{cases}\{\operatorname{sign}(\beta)\},&\beta\neq0\\[2pt] [-1,\,1],&\beta=0 .\end{cases}
$$

### La condición de optimalidad

En una función convexa, $\hat{\boldsymbol\beta}$ es un mínimo si y solo si el vector cero pertenece a su subdiferencial; es la versión general de "la derivada se anula". La parte cuadrática es derivable, con gradiente $-2X^T(\boldsymbol y-X\boldsymbol\beta)$, igual que en ridge. Para cada coordenada $j$, la condición queda

$$
0\in -2X_j^T(\boldsymbol y-X\hat{\boldsymbol\beta})+\lambda\,\partial|\hat\beta_j| .
$$

Analizamos los dos casos posibles:

- **Si $\hat\beta_j\neq0$**, el subdiferencial es solo $\{\operatorname{sign}(\hat\beta_j)\}$, y la condición es una igualdad: $2X_j^T(\boldsymbol y-X\hat{\boldsymbol\beta})=\lambda\operatorname{sign}(\hat\beta_j)$.
- **Si $\hat\beta_j=0$**, necesitamos que $2X_j^T(\boldsymbol y-X\hat{\boldsymbol\beta})=\lambda c$ para algún $c\in[-1,1]$, lo que equivale a $\left|2X_j^T(\boldsymbol y-X\hat{\boldsymbol\beta})\right|\le\lambda$.

Dividiendo entre 2, obtenemos las **condiciones de Karush-Kuhn-Tucker (KKT)** de lasso: $\hat{\boldsymbol\beta}$ es el estimador lasso si y solo si, para cada $j$,

$$
\boxed{\ X_j^T(\boldsymbol y-X\hat{\boldsymbol\beta})=\frac\lambda2\operatorname{sign}(\hat\beta_j)\ \ \text{si }\hat\beta_j\neq0,\qquad \left|X_j^T(\boldsymbol y-X\hat{\boldsymbol\beta})\right|\le\frac\lambda2\ \ \text{si }\hat\beta_j=0\ }
$$

:::{admonition} Interpretación
:class: tip

$X_j^T(\boldsymbol y-X\hat{\boldsymbol\beta})$ mide (sin normalizar) la correlación entre el predictor $j$ y el residuo. Las condiciones dicen que:

- todas las variables **activas** ($\hat\beta_j\neq0$) tienen la misma correlación absoluta con el residuo, igual a $\lambda/2$;
- las variables **inactivas** tienen una correlación menor o igual a $\lambda/2$: no aportan lo suficiente como para "pagar" la penalización.

De aquí sale la **selección de variables** de lasso: algunos coeficientes son exactamente cero, algo que en ridge no ocurre.
:::

## Caso ortogonal: solución cerrada por umbralización suave

Supongamos que los predictores estandarizados son ortogonales entre sí, es decir, $X^TX=nI$ (los productos entre columnas distintas son cero y cada $X_j^TX_j=n$). Definimos el estimador de mínimos cuadrados

$$
\boldsymbol b=\hat{\boldsymbol\beta}^{\text{MC}}=(X^TX)^{-1}X^T\boldsymbol y=\frac1nX^T\boldsymbol y ,
\qquad\text{de modo que}\qquad X^T\boldsymbol y=n\boldsymbol b .
$$

### Paso 1. El problema se separa en coordenadas

Expandiendo la parte cuadrática como en ridge, $S_\lambda(\boldsymbol\beta)=\boldsymbol y^T\boldsymbol y-2\boldsymbol\beta^TX^T\boldsymbol y+\boldsymbol\beta^TX^TX\boldsymbol\beta+\lambda\sum_j|\beta_j|$. Reemplazando $X^TX=nI$ y $X^T\boldsymbol y=n\boldsymbol b$,

$$
S_\lambda(\boldsymbol\beta)=\boldsymbol y^T\boldsymbol y-2n\boldsymbol\beta^T\boldsymbol b+n\boldsymbol\beta^T\boldsymbol\beta+\lambda\sum_{j=1}^p|\beta_j| .
$$

Escribiendo los productos como sumas, $\boldsymbol\beta^T\boldsymbol b=\sum_j\beta_jb_j$ y $\boldsymbol\beta^T\boldsymbol\beta=\sum_j\beta_j^2$, todo se agrupa por coordenadas:

$$
S_\lambda(\boldsymbol\beta)=\boldsymbol y^T\boldsymbol y+\sum_{j=1}^p\underbrace{\left(n\beta_j^2-2nb_j\beta_j+\lambda|\beta_j|\right)}_{g_j(\beta_j)} .
$$

Cada $g_j$ depende de un solo coeficiente, así que minimizar $S_\lambda$ equivale a minimizar cada $g_j$ por separado.

### Paso 2. Minimizar $g(\beta)=n\beta^2-2nb\beta+\lambda|\beta|$

Quitamos el valor absoluto separando según el signo de $\beta$:

- **Si $\beta>0$:** $|\beta|=\beta$ y $g$ es derivable. Igualando $g'(\beta)=2n\beta-2nb+\lambda=0$ se obtiene $\beta=b-\frac{\lambda}{2n}$. Este valor es aceptable solo si es positivo, es decir, si $b>\frac{\lambda}{2n}$.
- **Si $\beta<0$:** $|\beta|=-\beta$ y $g'(\beta)=2n\beta-2nb-\lambda=0$, de donde $\beta=b+\frac{\lambda}{2n}$. Es aceptable solo si es negativo, es decir, si $b<-\frac{\lambda}{2n}$.
- **Si $\beta=0$:** usamos la condición del subgradiente. La parte derivable, $n\beta^2-2nb\beta$, tiene derivada $-2nb$ en $\beta=0$, y el subdiferencial de $\lambda|\beta|$ en cero es $\lambda[-1,1]$. La condición es $0\in-2nb+\lambda[-1,1]$, que equivale a $|2nb|\le\lambda$, o sea, $|b|\le\frac{\lambda}{2n}$.

Los tres casos cubren todos los valores posibles de $b$ sin solaparse, así que para cada $b$ hay exactamente una solución. Juntándolos en una sola fórmula:

$$
\boxed{\ \hat\beta_j^{\text{lasso}}=\operatorname{sign}(b_j)\left(|b_j|-\frac{\lambda}{2n}\right)_+\ }
\qquad\text{donde }(t)_+=\max\{t,0\}.
$$

Este operador se llama **umbralización suave** (*soft thresholding*), y se denota $\mathcal S_t(b)=\operatorname{sign}(b)\,(|b|-t)_+$. En palabras: si el coeficiente de mínimos cuadrados es pequeño (en valor absoluto, menor que $\frac{\lambda}{2n}$), lasso lo vuelve cero; si es grande, lo acerca a cero restándole $\frac{\lambda}{2n}$.

### Comparación con ridge en el mismo caso

Con $X^TX=nI$, la fórmula de ridge se simplifica a

$$
\hat{\boldsymbol\beta}^{\text{ridge}}=(nI+\lambda I)^{-1}X^T\boldsymbol y=\frac{1}{n+\lambda}\,n\boldsymbol b
\qquad\Longrightarrow\qquad
\hat\beta_j^{\text{ridge}}=\frac{n}{n+\lambda}\,b_j .
$$

| | Ridge | Lasso |
|---|---|---|
| Cómo encoge | Multiplica por un factor menor que 1 | Resta una cantidad fija, $\frac{\lambda}{2n}$ |
| ¿Puede volver un coeficiente exactamente cero? | No, salvo que $b_j=0$ | Sí, cuando $\lvert b_j\rvert\le\frac{\lambda}{2n}$ |

## Caso general: descenso por coordenadas

Cuando $X^TX$ no es diagonal, el problema ya no se separa. Sin embargo, si fijamos todos los coeficientes menos uno, el problema en ese coeficiente sí se reduce al caso anterior. Esa es la idea del **descenso por coordenadas** {cite:p}`friedman2010`.

Fijemos $\beta_k$ para todo $k\neq j$ y definamos el **residuo parcial**, es decir, lo que queda por explicar sin la variable $j$:

$$
\boldsymbol r^{(j)}=\boldsymbol y-\sum_{k\neq j}X_k\beta_k .
$$

Así, $\boldsymbol y-X\boldsymbol\beta=\boldsymbol r^{(j)}-X_j\beta_j$, y la función a minimizar en $\beta_j$ es

$$
\|\boldsymbol r^{(j)}-X_j\beta_j\|^2+\lambda|\beta_j|+\text{(términos que no dependen de }\beta_j).
$$

Expandiendo la norma y usando $X_j^TX_j=n$,

$$
\|\boldsymbol r^{(j)}-X_j\beta_j\|^2=\|\boldsymbol r^{(j)}\|^2-2\beta_jX_j^T\boldsymbol r^{(j)}+n\beta_j^2 ,
$$

de modo que, salvo constantes, la función es $n\beta_j^2-2\beta_jX_j^T\boldsymbol r^{(j)}+\lambda|\beta_j|$. Es exactamente la función $g$ del caso ortogonal, con $b=\frac1nX_j^T\boldsymbol r^{(j)}$. Por lo tanto, la actualización de cada coordenada es

$$
\boxed{\ \beta_j\ \leftarrow\ \mathcal S_{\lambda/(2n)}\!\left(\frac1nX_j^T\boldsymbol r^{(j)}\right),\qquad j=1,\dots,p\ }
$$

y se repite el ciclo sobre todas las coordenadas hasta que los coeficientes dejen de cambiar.

Como $S_\lambda$ es convexa y su parte no derivable es una suma de funciones de una sola coordenada, este método converge al mínimo global {cite:p}`tseng2001`. Es el algoritmo que usan `glmnet` en R y `Lasso` en scikit-learn.

## El algoritmo LARS

El algoritmo LARS {cite:p}`efron2004` no resuelve el problema para un solo $\lambda$, sino que calcula el **camino completo** de soluciones $\hat{\boldsymbol\beta}(\lambda)$ para todos los valores de $\lambda$ a la vez. Se apoya directamente en las condiciones KKT.

### Punto de partida

$\hat{\boldsymbol\beta}=\mathbf 0$ es solución si y solo si cumple las KKT con todas las variables inactivas. Con $\hat{\boldsymbol\beta}=\mathbf 0$ el residuo es $\boldsymbol y$, así que la condición es $|X_j^T\boldsymbol y|\le\lambda/2$ para todo $j$. Es decir, todos los coeficientes son cero mientras

$$
\lambda\ge\lambda_{\max}=2\max_j\left|X_j^T\boldsymbol y\right| .
$$

### El camino es lineal por tramos

Llamemos $\mathcal A$ al conjunto de variables activas, $X_{\mathcal A}$ a las columnas de $X$ correspondientes y $\boldsymbol s_{\mathcal A}$ al vector de signos de sus coeficientes. Mientras $\mathcal A$ y $\boldsymbol s_{\mathcal A}$ no cambien, las KKT de las variables activas forman un sistema de ecuaciones:

$$
X_{\mathcal A}^T\left(\boldsymbol y-X_{\mathcal A}\boldsymbol\beta_{\mathcal A}\right)=\frac\lambda2\boldsymbol s_{\mathcal A}.
$$

Distribuyendo, $X_{\mathcal A}^T\boldsymbol y-X_{\mathcal A}^TX_{\mathcal A}\boldsymbol\beta_{\mathcal A}=\frac\lambda2\boldsymbol s_{\mathcal A}$, y despejando (suponiendo que las columnas activas son linealmente independientes),

$$
\boxed{\ \hat{\boldsymbol\beta}_{\mathcal A}(\lambda)=\left(X_{\mathcal A}^TX_{\mathcal A}\right)^{-1}\left(X_{\mathcal A}^T\boldsymbol y-\frac\lambda2\boldsymbol s_{\mathcal A}\right)\ }
$$

Esta expresión es **lineal en $\lambda$**. Por lo tanto, el camino de soluciones está formado por segmentos de recta, y solo cambia de dirección cuando una variable entra o sale del conjunto activo. Basta con calcular esos puntos de quiebre.

### Pasos del algoritmo

1. Empezar con $\hat{\boldsymbol\beta}=\mathbf 0$ y residuo $\boldsymbol r=\boldsymbol y$. Activar la variable más correlacionada con $\boldsymbol r$.
2. Mover los coeficientes activos en la dirección dada por la fórmula anterior, llamada dirección **equiangular**: al avanzar así, todas las variables activas mantienen la misma correlación absoluta con el residuo, y esa correlación va disminuyendo.
3. Detenerse cuando alguna variable inactiva alcance esa misma correlación, y agregarla al conjunto activo.
4. **Modificación para lasso:** si antes de eso algún coeficiente activo llega a cero, sacarlo del conjunto activo y recalcular la dirección. Sin esta regla se obtiene el LARS original, que no siempre coincide con lasso.
5. Repetir hasta llegar a $\lambda=0$ o hasta haber incluido todas las variables. Con $\lambda=0$ y $X$ de rango completo, se llega a la solución de mínimos cuadrados.

El costo total del algoritmo es del mismo orden que un solo ajuste de mínimos cuadrados, lo que lo hace mucho más eficiente que resolver el problema por separado para cada valor de $\lambda$, y facilita escoger $\lambda$ por validación cruzada.

## Verificación numérica

Las fórmulas anteriores se pueden comprobar en Python. El siguiente código implementa el descenso por coordenadas con umbralización suave, lo compara con `Lasso` de scikit-learn y verifica las condiciones KKT y la fórmula lineal por tramos del conjunto activo.

```python
import numpy as np
from sklearn.linear_model import Lasso

rng = np.random.default_rng(0)
n, p = 50, 6
X = rng.normal(size=(n, p))
X = (X - X.mean(0)) / X.std(0)          # suma 0 y suma de cuadrados n
y = X @ np.array([3, 0, -2, 0, 0, 1.]) + rng.normal(size=n)
y = y - y.mean()                         # respuesta centrada
lam = 40.0

def S(z, t):                             # umbralización suave
    return np.sign(z) * max(abs(z) - t, 0)

b = np.zeros(p)
for _ in range(500):                     # descenso por coordenadas
    for j in range(p):
        r = y - X @ b + X[:, j] * b[j]   # residuo parcial
        b[j] = S(X[:, j] @ r / n, lam / (2 * n))

sk = Lasso(alpha=lam / (2 * n), fit_intercept=False,
           tol=1e-12, max_iter=100000).fit(X, y).coef_
print(np.round(b, 5))
print(np.round(sk, 5))

g = X.T @ (y - X @ b)                    # condiciones KKT
print("KKT activas:", np.round(g[b != 0] / (lam / 2), 5))
print("KKT inactivas |.|:", np.round(abs(g[b == 0]) / (lam / 2), 3))

A = b != 0                               # fórmula del conjunto activo
s, XA = np.sign(b[A]), X[:, A]
print(np.round(np.linalg.solve(XA.T @ XA, XA.T @ y - lam / 2 * s), 5))
print("lambda_max:", 2 * abs(X.T @ y).max())
```

```text
[ 2.48791 -0.      -1.39324 -0.       0.       0.73979]
[ 2.48791 -0.      -1.39324 -0.       0.       0.73979]
KKT activas: [ 1. -1.  1.]
KKT inactivas |.|: [0.384 0.582 0.273]
[ 2.48791 -1.39324  0.73979]
lambda_max: 275.0074100476726
```

Los resultados confirman lo demostrado:

- el descenso por coordenadas y scikit-learn dan los mismos coeficientes, y tres de ellos son exactamente cero;
- en las variables activas, $X_j^T(\boldsymbol y-X\hat{\boldsymbol\beta})/(\lambda/2)$ vale exactamente $\operatorname{sign}(\hat\beta_j)$, y en las inactivas es menor que 1 en valor absoluto;
- la fórmula del conjunto activo reproduce los coeficientes no nulos.

:::{admonition} Relación con el parámetro de scikit-learn
:class: tip

`sklearn.linear_model.Lasso` minimiza $\frac{1}{2n}\|\boldsymbol y-X\boldsymbol\beta\|^2+\alpha\|\boldsymbol\beta\|_1$. Multiplicando esta función por $2n$ se obtiene la de esta sección, así que ambos problemas tienen la misma solución cuando

$$
\alpha=\frac{\lambda}{2n} .
$$

Por eso en el código se usó `alpha = lam / (2 * n)`, y por eso el umbral de la umbralización suave coincide con $\alpha$.
:::

Finalmente, a diferencia de ridge (ecuación 3.12), para lasso no existen fórmulas cerradas de $\mathbb E(\hat{\boldsymbol\beta}^{\text{lasso}})$ ni de $\operatorname{Cov}(\hat{\boldsymbol\beta}^{\text{lasso}})$, porque el estimador no es una función lineal de $\boldsymbol y$.
