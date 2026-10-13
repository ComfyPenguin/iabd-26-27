---
title: Píldoras ampliadas · IABD
description: Biblioteca de cimientos, laboratorios y actividades progresivas para el proyecto 2 y otros contextos.
icon: material/bookshelf
---

# Píldoras ampliadas · IABD

Esta biblioteca contiene once píldoras con teoría, ejemplos comentados, prácticas graduadas, comprobaciones y actividades autónomas. Trabajaremos con casos de sensores, agua, energía, incidencias y bibliotecas. La aplicación a BiciTierra Market aparece al final de cada unidad.

Las píldoras breves ofrecen una consulta rápida; las ampliadas desarrollan los conceptos y las actividades. El documento de trabajo indica qué necesitas en cada semana.

## Acceso para el alumnado

1. Localiza el tema que necesitas en la mesa.
2. Comprueba los conocimientos previos y lee los cimientos.
3. Ejecuta el laboratorio y completa la práctica que indico el profesorado.
4. Usa los retos y las soluciones orientativas para comprobar comprensión.
5. Vuelve al último apartado para adaptar el que has aprendido al proyecto.

No es necesario instalar todas las herramientas ni hacer todas las prácticas. El recorrido esencial es lectura, laboratorio y práctica inicial; las otras actividades permiten profundizar o atender diferentes ritmos.

## Acceso para el profesorado

Cada unidad incluye objetivos, prerrequisitos, tiempos orientativos, tres prácticas con evidencia esperada, preguntas de reflexión y ampliaciones, tres retos con solución orientativa y errores habituales. Las soluciones están en el mismo material para facilitar autoevaluación; no son un solucionario reservado ni una propuesta de examen.

Selecciona actividades según las necesidades del grupo. La suma de todas las extensiones excede el tiempo ordinario de seis semanas. MLflow es una ampliación opcional. Para valorar el trabajo, pide que el alumnado explique la pregunta, justifique la decisión, compruebe el resultado y reconozca una limitación.

## Catálogo y conexión con las semanas

La numeración ordena los temas, no las semanas. Los momentos de uso son orientativos y se tienen que leer junto con el documento de trabajo del proyecto.

| Píldora | Momento en BiciTierra Market | Laboratorio general |
|---|---|---|
| [01 · Python i entorns de treball](python-i-entorns/index.md) | 1 | Validación de lecturas de un sensor |
| [02 · Jupyter: notebooks que es poden reproduir](jupyter-notebooks/index.md) | 1 | Consumo de agua y ejecución desde cero |
| [03 · NumPy i pandas: de les dades a les taules](python-per-a-dades-i/index.md) | 1–2 | Incidencias y catálogo de servicios |
| [04 · Exploració i qualitat de dades](eda-i-qualitat-de-dades/index.md) | 1–2 | Auditoría de sensores |
| [05 · Visualització amb Matplotlib i seaborn](visualitzacio-amb-matplotlib-i-seaborn/index.md) | 2, 4 y 6 | Consumo energético de tres edificios |
| [06 · Aprenentatge supervisat amb scikit-learn](scikit-learn-intro/index.md) | 3 | Regresión sobre viviendas sintéticas |
| [07 · Validació: mesurar allò que realment volem predir](validacio-de-models/index.md) | 2, 3 y 5 | Backtesting mensual de dos edificios |
| [08 · Clustering i interpretació de perfils](clustering-i-segmentacio/index.md) | 4 | Perfiles de uso de una biblioteca |
| [09 · Interpretabilitat: permutació, SHAP i LIME](xai-shap-lime/index.md) | 5 | Explicaciones de un modelo de consumo |
| [10 · Dashboards i modelatge amb Power BI](dashboards-i-powerbi/index.md) | 6 | Indicadores de un servicio técnico |
| [11 · MLflow: experiments traçables i comparables](mlflow-intro/index.md) | 3–6, ampliación | Comparación local de regresiones |

## Preparación común

Los programas crean sus propios datos y escriben solo dentro de `codi/eixides/` de la píldora. No modifican los CSV de BiciTierra. Trabaja sobre copias cuando resuelvas variantes.

Desde la carpeta `PINDOLES_AMPLIADES`, crea un entorno con Python 3.10 o superior:

```bash title="GNU/Linux o macOS"
python3 -m venv .venv
source .venv/bin/activate
```

En Windows, con PowerShell:

```powershell
py -m venv .venv
.venv\Scripts\Activate.ps1
```

Si la activación no está disponible en el equipo del centro, usa directamente el intérprete del entorno (`.venv/bin/python` o `.venv\Scripts\python.exe`) o la configuración indicada por el profesorado.

Después entra en la carpeta de la unidad que quieras trabajar e instala sus requisitos. Por ejemplo:

```bash
cd docs/python-per-a-dades-i
python -m pip install -r requirements.txt
python codi/exemple.py
```

La primera píldora usa solo Python estándar. Los requisitos de cada tema evitan instalar SHAP o MLflow cuando todavía no se necesitan. Para preparar un en torno a profesorado con todas las demostraciones, hay también [requisits complets](requirements-complets.txt). Las versiones se han fijado según la comprobación con Python 3.10; consulta el [registre de validació](VALIDACIO.md).

La interfaz de Jupyter es opcional: se puede instalar con `python -m pip install "notebook>=7,<8"`. El ejemplo de notebooks crea y ejecuta el fichero también sin abrirla. Power BI Desktop es necesario para construir y verificar el dashboard; Python genera los CSV y los totales de control.

## Cómo está organizada cada carpeta?

Las once unidades están dentro de `docs/`, una carpeta por tema. El índice general y el registro de validación también están dentro de `docs/`.

- `index.md`: unidad completa de trabajo, incluyendo ejemplos y actividades.
- `codi/exemple.py`: demostración ejecutable y comentada.
- `requirements.txt`: dependencias de la unidad.
- `codi/eixides/`: resultados que aparecen cuando ejecutas el programa; no son datos de entrada que tengas que buscar.

La unidad de visualización incluye una figura ya generada. Las actividades no requieren servicios en linea ni cuentas externas, salvo descargar inicialmente las dependencias si no están instaladas.

## Lectura y presentación

Puedes abrir los Markdown en un editor. Para mostrar desplegables, avisos, mesas y diagramas con el mismo estilo de la biblioteca de referencia, incluimos una configuración de MkDocs Material:

```bash
python -m pip install -r docs/requirements-presentacio.txt
mkdocs serve
```

Ejecuta estos mandos desde `PINDOLES_AMPLIADES` y abre la dirección local indicada. La presentación es opcional; los programas y los contenidos se pueden usar sin crear ningún web. No hay publicación automática.
