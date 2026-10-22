# Demostración de la desigualdad 2η(1−η) ≤ 2·mín{η, 1−η} (caso 1−η)

:::{admonition} Enunciado (cota del error esperado de 1-NN)
:class: note

En las notas, para acotar el error esperado de la regla del vecino más cercano, se usa que para todo $\boldsymbol x$

$$
2\eta(\boldsymbol x)\left(1-\eta(\boldsymbol x)\right)\le 2\min\{\eta(\boldsymbol x),\,1-\eta(\boldsymbol x)\},
$$

y de ahí se concluye

$$
\underset{S\sim\mathcal D^m}{\mathbb E}\left[L_{\mathcal D}(h_S)\right]\le 2L_{\mathcal D}(h^\star)+c\underset{\boldsymbol x\sim\mathcal D_{\boldsymbol x},\ \mathcal D_{\boldsymbol x}\sim\mathcal D^m_{\boldsymbol x}}{\mathbb E}\left(\|\boldsymbol x-\boldsymbol x'\|\right).
$$

En clase se demostró el caso en que el mínimo es $\eta(\boldsymbol x)$. Aquí se demuestra el caso en que el mínimo es $1-\eta(\boldsymbol x)$.
:::

Recordemos que $\eta(\boldsymbol x)=\mathbb P[y=1\mid\boldsymbol x]$ es una probabilidad, así que $\eta(\boldsymbol x)\in[0,1]$. Para que la escritura sea más liviana, en lo que sigue escribimos simplemente $\eta$ en lugar de $\eta(\boldsymbol x)$; todo el argumento se hace con $\boldsymbol x$ fijo.

## Qué significa estar en este caso

Estamos suponiendo que

$$
\min\{\eta,\,1-\eta\}=1-\eta .
$$

Que el mínimo sea $1-\eta$ quiere decir que $1-\eta$ es el menor de los dos números:

$$
1-\eta\le\eta .
$$

Sumando $\eta$ a ambos lados queda $1\le 2\eta$, y dividiendo entre 2,

$$
\eta\ge\frac12 .
$$

Es decir, este caso corresponde a los puntos $\boldsymbol x$ donde la clase 1 es al menos tan probable como la clase 0.

Además, como $\eta\in[0,1]$, se cumplen dos hechos que vamos a usar:

$$
\eta\le 1 \qquad\text{y}\qquad 1-\eta\ge 0 .
$$

## La desigualdad

Partimos de $\eta\le 1$ y multiplicamos ambos lados por $2(1-\eta)$. Como acabamos de ver que $1-\eta\ge0$, el número $2(1-\eta)$ no es negativo, y multiplicar por un número no negativo **no cambia el sentido** de la desigualdad:

$$
\eta\cdot 2(1-\eta)\le 1\cdot 2(1-\eta),
$$

es decir,

$$
2\eta(1-\eta)\le 2(1-\eta).
$$

Finalmente, como en este caso $1-\eta=\min\{\eta,1-\eta\}$, podemos reemplazar el lado derecho:

$$
2\eta(1-\eta)\le 2(1-\eta)=2\min\{\eta,\,1-\eta\}.
$$

Esto prueba la desigualdad para el caso $\min\{\eta,1-\eta\}=1-\eta$. Junto con el caso visto en clase ($\min\{\eta,1-\eta\}=\eta$), queda demostrada para todo $\eta\in[0,1]$, es decir, para todo $\boldsymbol x$. $\blacksquare$

## Un argumento que cubre los dos casos a la vez

Los números $\eta$ y $1-\eta$ forman un par: uno es el menor y el otro el mayor. El producto de dos números no cambia si se escribe como "el menor por el mayor", así que

$$
\eta(1-\eta)=\min\{\eta,1-\eta\}\cdot\max\{\eta,1-\eta\}.
$$

Como ambos están en $[0,1]$, el mayor es a lo sumo 1, y el menor no es negativo. Por eso

$$
\min\{\eta,1-\eta\}\cdot\max\{\eta,1-\eta\}\le\min\{\eta,1-\eta\}\cdot 1 .
$$

Multiplicando por 2 se obtiene $2\eta(1-\eta)\le 2\min\{\eta,1-\eta\}$, sin necesidad de separar en casos.

## Cómo se usa en la cota de 1-NN

En la sección anterior vimos que el error del clasificador de Bayes $h^\star$ es

$$
L_{\mathcal D}(h^\star)=\mathbb E_{\boldsymbol x}\left[\min\{\eta(\boldsymbol x),1-\eta(\boldsymbol x)\}\right].
$$

Como la desigualdad vale en cada punto $\boldsymbol x$, también vale al tomar la esperanza:

$$
\mathbb E_{\boldsymbol x}\left[2\eta(\boldsymbol x)\left(1-\eta(\boldsymbol x)\right)\right]\le 2\,\mathbb E_{\boldsymbol x}\left[\min\{\eta(\boldsymbol x),1-\eta(\boldsymbol x)\}\right]=2L_{\mathcal D}(h^\star).
$$

Este es el paso que permite pasar del término $2\eta(1-\eta)$ que aparece en la demostración del teorema al término $2L_{\mathcal D}(h^\star)$ de la cota final: el error esperado de 1-NN es, a lo sumo, el doble del error de Bayes más un término que depende de qué tan cerca están los vecinos.

:::{admonition} Observación sobre el término intermedio de las notas
:class: warning

Para el caso $\min\{\eta,1-\eta\}=\eta$, las notas escriben la cadena

$$
2\eta(1-\eta)\le 2(1-\eta)^2\le 2\min\{\eta,1-\eta\}.
$$

La primera desigualdad es correcta (se obtiene multiplicando $\eta\le1-\eta$ por $2(1-\eta)\ge0$), pero **la segunda no se cumple en general**. Por ejemplo, con $\eta=0.1$, que pertenece a ese caso porque $0.1\le0.9$:

$$
2(1-\eta)^2=2(0.9)^2=1.62 ,
$$

mientras que

$$
2\min\{\eta,1-\eta\}=2(0.1)=0.2 .
$$

El resultado final sí es cierto, pero el camino correcto es el análogo al de esta sección: como $1-\eta\le1$ y $\eta\ge0$,

$$
2\eta(1-\eta)\le 2\eta=2\min\{\eta,1-\eta\}.
$$

Por esa razón, para el caso $1-\eta$ no se siguió la estructura de la cadena de las notas: su versión simétrica, $2\eta(1-\eta)\le2\eta^2\le2(1-\eta)$, falla también en el segundo paso (con $\eta=1$ quedaría $2\le0$).
:::
