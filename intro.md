# Demostraciones de Machine Learning

**Estudiante:** Leiry Laura Mares Cure

**Programa:** Maestría en Ingeniería Biomédica, Universidad del Norte

**Asignatura:** Machine Learning

**Profesor:** Lihki Rubio, Ph.D.

**Fecha:** octubre de 2026

---

## Presentación

Este libro reúne las demostraciones matemáticas desarrolladas a partir de las notas de clase del curso de Machine Learning {cite:p}`rubio_notas`. Los enunciados se toman tal como aparecen en las notas, con su misma numeración, para que cada resultado se pueda ubicar fácilmente en el material del curso.

En cada sección se sigue la misma estructura:

1. Se presenta el **enunciado** que se quiere demostrar o verificar.
2. Se fijan la **notación** y los **supuestos** que se van a usar.
3. Se desarrolla la demostración **paso a paso**, justificando cada igualdad o desigualdad.
4. Cuando aplica, se cierra con una **interpretación** del resultado.

A lo largo del trabajo aparecieron algunas inconsistencias menores en las notas (subíndices, paréntesis o pasos intermedios). Cuando eso ocurre, se señala en un recuadro de **observación** junto con la corrección propuesta, sin que esto afecte la validez del resultado final.

## Organización

El contenido sigue el orden de los capítulos del curso:

- **Capítulo 1. Teoría del aprendizaje y k-NN.** Riesgo verdadero y empírico (ecuación 2.2), su relación con el clasificador de Bayes, y una desigualdad que se usa en la cota del error de 1-NN.
- **Capítulo 2. Regresión Ridge y Lasso.** Desarrollo de la función objetivo de ridge, el estimador ridge (ecuaciones 3.7 y 3.11), sus propiedades estadísticas (ecuación 3.12) y la estimación de los parámetros lasso.
- **Capítulo 3. Árboles de decisión y boosting.** Corrección del ejercicio 12.6 sobre la construcción de un árbol con el índice de Gini, y la deducción del clasificador AdaBoost (ecuaciones 5.2 y 5.3) a partir de su algoritmo.

Antes de empezar, la sección {doc}`notacion` resume los símbolos que se usan en todo el libro.
