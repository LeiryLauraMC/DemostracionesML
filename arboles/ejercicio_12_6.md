# Corrección del ejercicio 12.6

:::{admonition} Enunciado (ejercicio 6 de la sección Random Forest y XGBoost)
:class: note

La siguiente tabla muestra 10 observaciones. Úsela para predecir si un individuo será astronauta (*Gonna Be an Astronaut*, variable binaria). Las variables explicativas son *Age* (numérica), *Like Dogs* y *Like Gravity* (binarias).

| Age | Like Dogs | Like Gravity | Gonna Be an Astronaut |
|:---:|:---:|:---:|:---:|
| 24 | 0 | 0 | 0 |
| 30 | 1 | 1 | 1 |
| 36 | 0 | 1 | 1 |
| 36 | 0 | 0 | 0 |
| 42 | 0 | 0 | 0 |
| 44 | 1 | 1 | 1 |
| 46 | 1 | 0 | 0 |
| 47 | 1 | 1 | 1 |
| 47 | 0 | 1 | 1 |
| 51 | 1 | 1 | 0 |
:::

En esta sección se construye el árbol de decisión paso a paso y se compara con la solución publicada en la sección 12 de las notas. El nodo raíz de esa solución es correcto, pero en el segundo nivel hay errores de cálculo que llevan a un árbol final que clasifica mal dos observaciones de entrenamiento.

## Criterio de división

Usamos las definiciones de las notas. Si un nodo tiene $N$ observaciones, de las cuales $n_1$ son de la clase SI (1) y $n_0$ de la clase NO (0), su **índice de Gini** es

$$
\text{Gini}(\text{nodo})=1-\left(\frac{n_1}{N}\right)^2-\left(\frac{n_0}{N}\right)^2 .
$$

Vale 0 si el nodo es puro (todas las observaciones son de la misma clase) y 0.5 si las dos clases están igualmente representadas. Al dividir un nodo en dos hijos con $N_1$ y $N_2$ observaciones, la impureza de la división es el promedio ponderado

$$
\text{Gini}_{\text{split}}=\frac{N_1}{N}\,\text{Gini}(\text{nodo}_1)+\frac{N_2}{N}\,\text{Gini}(\text{nodo}_2),
$$

y en cada nodo se escoge la división con **menor** $\text{Gini}_{\text{split}}$, que es la que produce la mayor disminución de impureza.

Para las variables numéricas, los umbrales candidatos son los puntos medios entre valores **distintos** consecutivos, y la división es "edad < umbral" frente a "edad ≥ umbral".

## Paso 1. Nodo raíz

El nodo raíz tiene las 10 observaciones: 5 SI y 5 NO. Su impureza es

$$
\text{Gini}(\text{raíz})=1-\left(\tfrac{5}{10}\right)^2-\left(\tfrac{5}{10}\right)^2=1-0.25-0.25=0.5 .
$$

### Variable *Like Gravity*

- *Like Gravity* = 1: edades 30, 36, 44, 47, 47, 51, con clases 1, 1, 1, 1, 1, 0. Son 6 observaciones: 5 SI y 1 NO.

$$
\text{Gini}=1-\left(\tfrac56\right)^2-\left(\tfrac16\right)^2=1-\tfrac{25}{36}-\tfrac{1}{36}=\tfrac{10}{36}\approx0.278 .
$$

- *Like Gravity* = 0: edades 24, 36, 42, 46, todas de clase 0. Son 4 observaciones: 0 SI y 4 NO, así que el nodo es puro y su Gini es 0.

$$
\text{Gini}_{\text{split}}=\tfrac{6}{10}(0.278)+\tfrac{4}{10}(0)\approx0.167 .
$$

### Variable *Like Dogs*

- *Like Dogs* = 1: edades 30, 44, 46, 47, 51, con clases 1, 1, 0, 1, 0. Son 5 observaciones: 3 SI y 2 NO, con $\text{Gini}=1-0.36-0.16=0.48$.
- *Like Dogs* = 0: edades 24, 36, 36, 42, 47, con clases 0, 1, 0, 0, 1. Son 5 observaciones: 2 SI y 3 NO, con $\text{Gini}=1-0.16-0.36=0.48$.

$$
\text{Gini}_{\text{split}}=\tfrac{5}{10}(0.48)+\tfrac{5}{10}(0.48)=0.48 .
$$

### Variable *Age*

Ordenando las observaciones por edad, las clases son:

| Age | 24 | 30 | 36 | 36 | 42 | 44 | 46 | 47 | 47 | 51 |
|:---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Clase | 0 | 1 | 1 | 0 | 0 | 1 | 0 | 1 | 1 | 0 |

Las edades distintas son 24, 30, 36, 42, 44, 46, 47 y 51, así que los umbrales candidatos son 27, 33, 39, 43, 45, 46.5 y 49. Para cada umbral contamos cuántas observaciones SI y NO quedan a cada lado:

| Umbral | Edad < umbral (SI, NO) | Gini | Edad ≥ umbral (SI, NO) | Gini | $\text{Gini}_{\text{split}}$ |
|:---:|:---:|:---:|:---:|:---:|:---:|
| 27 | (0, 1) | 0 | (5, 4) | 0.494 | **0.444** |
| 33 | (1, 1) | 0.5 | (4, 4) | 0.5 | 0.500 |
| 39 | (2, 2) | 0.5 | (3, 3) | 0.5 | 0.500 |
| 43 | (2, 3) | 0.48 | (3, 2) | 0.48 | 0.480 |
| 45 | (3, 3) | 0.5 | (2, 2) | 0.5 | 0.500 |
| 46.5 | (3, 4) | 0.490 | (2, 1) | 0.444 | 0.476 |
| 49 | (5, 4) | 0.494 | (0, 1) | 0 | **0.444** |

Por ejemplo, para el umbral 27 a la izquierda queda solo la persona de 24 años (clase 0), con Gini 0, y a la derecha quedan 9 observaciones con 5 SI y 4 NO, con $\text{Gini}=1-\frac{25}{81}-\frac{16}{81}=\frac{40}{81}\approx0.494$. Entonces $\text{Gini}_{\text{split}}=\frac{1}{10}(0)+\frac{9}{10}(0.494)\approx0.444$. Los demás umbrales se calculan igual.

El mejor umbral de *Age* da 0.444, empatado entre 27 y 49.

### Elección del nodo raíz

| Variable | Mejor $\text{Gini}_{\text{split}}$ | Disminución de impureza ($0.5-\text{Gini}_{\text{split}}$) |
|:---|:---:|:---:|
| **Like Gravity** | **0.167** | **0.333** |
| Age (umbral 27 o 49) | 0.444 | 0.056 |
| Like Dogs | 0.480 | 0.020 |

El nodo raíz es *Like Gravity*, y la rama *Like Gravity* = 0 es una hoja pura (las 4 observaciones son NO). **Hasta aquí la solución publicada es correcta.**

## Paso 2. Nodo *Like Gravity* = 1

Este nodo tiene 6 observaciones, 5 SI y 1 NO, con Gini 0.278:

| Age | 30 | 36 | 44 | 47 | 47 | 51 |
|:---|:---:|:---:|:---:|:---:|:---:|:---:|
| Like Dogs | 1 | 0 | 1 | 1 | 0 | 1 |
| Clase | 1 | 1 | 1 | 1 | 1 | 0 |

### Variable *Like Dogs*

- *Like Dogs* = 1: edades 30, 44, 47, 51, con clases 1, 1, 1, 0. Son 4 observaciones: 3 SI y 1 NO.

$$
\text{Gini}=1-\left(\tfrac34\right)^2-\left(\tfrac14\right)^2=1-\tfrac{9}{16}-\tfrac{1}{16}=\tfrac{6}{16}=0.375 .
$$

- *Like Dogs* = 0: edades 36 y 47, ambas de clase 1. El nodo es puro y su Gini es 0.

$$
\text{Gini}_{\text{split}}=\tfrac46(0.375)+\tfrac26(0)=0.25 .
$$

:::{admonition} Error 1 de la solución publicada
:class: warning

La solución publicada reporta un Gini de **0.17** para *Like Dogs* en este nodo. El valor correcto es **0.25**. El valor 0.17 coincide con el Gini de *Like Gravity* en el nodo raíz, así que probablemente se trasladó por error desde el Paso 1.
:::

### Variable *Age*

Las edades distintas en este nodo son 30, 36, 44, 47 y 51, así que los umbrales candidatos son 33, 40, 45.5 y 49:

| Umbral | Edad < umbral (SI, NO) | Gini | Edad ≥ umbral (SI, NO) | Gini | $\text{Gini}_{\text{split}}$ |
|:---:|:---:|:---:|:---:|:---:|:---:|
| 33 | (1, 0) | 0 | (4, 1) | 0.320 | 0.267 |
| 40 | (2, 0) | 0 | (3, 1) | 0.375 | 0.250 |
| 45.5 | (3, 0) | 0 | (2, 1) | 0.444 | 0.222 |
| **49** | **(5, 0)** | **0** | **(0, 1)** | **0** | **0** |

Detallamos los dos umbrales que importan para la corrección.

**Umbral 45.5**, el que escogió la solución publicada. A la izquierda quedan 30, 36 y 44 años (tres SI), con Gini 0. A la derecha quedan 47, 47 y 51 años (dos SI y un NO):

$$
\text{Gini}=1-\left(\tfrac23\right)^2-\left(\tfrac13\right)^2=1-\tfrac49-\tfrac19=\tfrac49\approx0.444 .
$$

Entonces

$$
\text{Gini}_{\text{split}}=\tfrac36(0)+\tfrac36(0.444)\approx0.222 .
$$

**Umbral 49** $=\frac{47+51}{2}$. A la izquierda quedan 30, 36, 44, 47 y 47 años, todos SI, con Gini 0. A la derecha queda solo la persona de 51 años, que es NO, con Gini 0:

$$
\text{Gini}_{\text{split}}=\tfrac56(0)+\tfrac16(0)=0 .
$$

:::{admonition} Error 2 de la solución publicada
:class: warning

La solución publicada afirma que el umbral con menor Gini es 45.5, con 0.22. Sin embargo, el umbral 49 da Gini **0**: separa perfectamente el nodo, porque todas las personas menores de 49 años son astronautas y la única de 51 no lo es. Como $0<0.222$ y $0<0.25$, la mejor división de este nodo es ***Age* < 49**, no *Like Dogs*.
:::

Con esa división, los dos nodos hijos son puros, así que el crecimiento del árbol se detiene aquí.

## El árbol correcto

```text
Like Gravity?
├── 0 (NO) ──────────────> NO astronauta     (4 de 4, Gini 0)
└── 1 (SI) ──> Age < 49?
               ├── SI ───> SÍ astronauta     (5 de 5, Gini 0)
               └── NO ───> NO astronauta     (1 de 1, Gini 0)
```

El árbol tiene profundidad 2 y tres hojas puras. La variable *Like Dogs* no aparece porque no se necesita.

**Verificación sobre los datos de entrenamiento.** Recorriendo el árbol con cada observación:

| Age | Like Dogs | Like Gravity | Real | Árbol correcto | Árbol publicado |
|:---:|:---:|:---:|:---:|:---:|:---:|
| 24 | 0 | 0 | 0 | 0 | 0 |
| 30 | 1 | 1 | 1 | 1 | 1 |
| 36 | 0 | 1 | 1 | 1 | 1 |
| 36 | 0 | 0 | 0 | 0 | 0 |
| 42 | 0 | 0 | 0 | 0 | 0 |
| 44 | 1 | 1 | 1 | 1 | 1 |
| 46 | 1 | 0 | 0 | 0 | 0 |
| 47 | 1 | 1 | 1 | 1 | 1 |
| 47 | 0 | 1 | 1 | 1 | **0** |
| 51 | 1 | 1 | 0 | 0 | **1** |

El árbol correcto clasifica bien las 10 observaciones. El árbol publicado (*Like Gravity*, luego *Like Dogs*, y en la rama *Like Dogs* = 0 el corte *Age* < 45.5) falla en dos.

:::{admonition} Error 3 de la solución publicada
:class: warning

Aun aceptando *Like Dogs* como segundo nodo, las ramas del árbol publicado quedaron intercambiadas:

- Se declara como hoja "*Like Dogs* = SI → astronauta", pero ese nodo **no es puro**: la persona de 51 años tiene *Like Dogs* = 1 y no es astronauta.
- La rama que sí es pura es *Like Dogs* = NO (36 y 47 años, ambos astronautas). Sin embargo, ahí se aplica el corte "edad ≥ 45.5 → NO", que clasifica mal a la persona de 47 años con *Like Dogs* = 0.

Un árbol que se hace crecer hasta que todas sus hojas son puras debe tener error de entrenamiento 0; el publicado tiene 2 errores de 10.
:::

:::{admonition} Detalles menores
:class: note

- En el Paso 1, la solución publicada incluye los umbrales 36 y 47, que son puntos medios de valores repetidos ($\frac{36+36}{2}$ y $\frac{47+47}{2}$). Según las notas, los umbrales deben tomarse entre valores **distintos** consecutivos. Esto no cambia el resultado, porque esas particiones coinciden con las de 33 y 46.5.
- La fórmula $\frac{x_i+x_{i+1}}{2}$ se describe como "vinculada a la mediana", pero en realidad es el punto medio entre dos valores consecutivos ordenados, no la mediana.
:::
