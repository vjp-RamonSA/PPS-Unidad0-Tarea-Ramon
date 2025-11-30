# Configuración de Workflow para MkDocs en GitHub Actions

## 1. Ruta de directorios

Crear la siguiente estructura de carpetas y archivo:

```
.github/
└── workflows/
    └── crearDocumentacion.yml
```

Ejecución del Workflow

El workflow se ejecutará automáticamente cada vez que se haga un push a la rama `main` o `master`.

## 2. Contenido del archivo crearDocumentacion.yml

```bash
name: Deploy MkDocs

on:
  push:
    branches:
      - main

permissions:
  contents: write
  pages: write
  id-token: write

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repo
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'

      - name: Install dependencies
        run: pip install mkdocs

      - name: Deploy docs
        run: mkdocs gh-deploy --force
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

 ![](gitActions/1.png)


## 3. Explicación paso a paso del Workflow MkDocs

## Nombre del workflow
- **`name: Deploy MkDocs`**  
  Define el nombre del workflow.

---

## 3.1. Evento que lo ejecuta
- **`on: push → branches: main`**  
  Indica que el workflow se ejecutará cuando haya un **push** en la rama `main`.

---

## 3.2. Permisos necesarios
Se otorgan permisos para que el workflow pueda desplegar la documentación:

- **`contents: write`** → Modificar contenido del repositorio.  
- **`pages: write`** → Desplegar en GitHub Pages.  
- **`id-token: write`** → Autenticación segura.

---

## 3.3. Job principal
- **`jobs → deploy → runs-on: ubuntu-latest`**  
  El job se ejecuta en un entorno **Ubuntu**.

---

## 3.4. Pasos del workflow
1. **Checkout Repo** → Clona el repositorio.  
1. **Set up Python** → Configura Python en la versión 3.x.  
1. **Install dependencies** → Instala MkDocs.  
1. **Deploy docs** → Publica la documentación en GitHub Pages usando `mkdocs gh-deploy`.


## 4. Conclusión

La configuración de GitHub Actions permitió automatizar el proceso de construcción y despliegue de la documentación.  

Gracias a este flujo de trabajo, cada vez que se actualiza el repositorio la documentación se genera y publica de forma automática en GitHub Pages, asegurando que esté siempre disponible y actualizada sin necesidad de intervención manual.
