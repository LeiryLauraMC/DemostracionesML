# Demostraciones de Machine Learning

Leiry Laura Mares Cure. Maestría en Ingeniería Biomédica, Universidad del Norte.

Libro en formato Jupyter Book con las demostraciones desarrolladas a partir de las notas de clase del curso de Machine Learning.

## Compilar localmente

```bash
pip install -r requirements.txt
jupyter-book build .
```

El sitio queda en `_build/html/index.html`.

## Publicar en GitHub Pages

1. Crear un repositorio en GitHub y subir el contenido de esta carpeta a la rama `main`.
2. En el repositorio, ir a **Settings → Pages** y, en **Build and deployment → Source**, escoger **GitHub Actions**.
3. Cada vez que se haga `push` a `main`, el flujo `.github/workflows/deploy.yml` compila el libro y lo publica en `https://<usuario>.github.io/<repositorio>/`.

## Estructura

```text
intro.md                       Portada y presentación
notacion.md                    Notación y convenciones
knn/                           Capítulo 1. Teoría del aprendizaje y k-NN
ridge/                         Capítulo 2. Regresión Ridge y Lasso
arboles/                       Capítulo 3. Árboles de decisión y boosting
referencias.md, references.bib Bibliografía
_config.yml, _toc.yml          Configuración e índice del libro
```
