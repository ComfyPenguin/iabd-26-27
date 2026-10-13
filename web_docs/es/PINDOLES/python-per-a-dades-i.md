---
title: Python Para Datos I
---

# Python Para Datos I

## Objetivo de la píldora

Aquí bajamos a uso real de `NumPy` y `Pandas`, que son la base de casi todo el trabajo de datos del curso.

La idea no es memorizar todas las funciones, sino entender qué resuelve cada librería.

## Como repartir los papeles

- `NumPy`: arrays y cálculo numérico
- `Pandas`: mesas, columnas, limpieza y transformación de datos

La distinción es importante porque a principiantes los dos paquetes los pueden paréixer "librerías para datos" sin más. Pero no resuelven exactamente el mismo problema.

- `NumPy` es especialmente fuerte cuando trabajamos con estructura numérica homogénea y cálculo eficiente
- `Pandas` es especialmente fuerte cuando trabajamos con mesas reales, columnas heterogéneas, nulos y preguntas de negocio

## Pensar `NumPy`

Un array de `NumPy` es útil cuando trabajamos con valores numéricos y queremos hacer cálculo vectoritzat.

```python
import numpy as np

lectures = np.array([18.2, 19.1, 21.4])
mitjana = lectures.mean()
```

La idea de fondo es que `NumPy` permite operar sobre colecciones de valores de una forma más próxima a las matemáticas y menos a un bucle manual línea por línea.

No siempre el alumnado lo verá de manera explícita, pero muchas herramientas de datos y ML se apoyan en esta lógica matricial.

## Pensar `Pandas`

`Pandas` es la herramienta central cuando tenemos ficheros tabulares, registros o datasets de negocio.

```python
import pandas as pd

df = pd.read_csv("sales_history.csv")
df["marge"] = df["ingressos"] - df["cost"]
```

La potencia real de `Pandas` es que permite pensar una mesa no solo como un fichero, sino como un espacio de trabajo donde podemos:

- describir calidad
- transformar columnas
- resumir patrones
- preparar la base para modelar o visualizar

## Por qué esta capa es tan central al curso

Muchas veces el alumnado piensa que el centro del trabajo está en el modelo. Pero en la práctica, la mayor parte del tiempo se invierte en:

- entender datos
- limpiar valores
- revisar tipo
- crear variables útiles
- resumir patrones

Por eso `NumPy` y `Pandas` no son un prólogo. Son una parte muy grande del trabajo real.

## Operaciones mínimas que sí tenemos que saber hacer

- cargar un CSV
- mirar columnas y tipos
- detectar nulos
- filtrar filas
- crear una columna derivada
- agrupar y resumir

También conviene llegar a entender qué significa cada una.

- cargar un CSV: pasar de un fichero a una mesa manipulable
- mirar tipo: detectar si hay números, textos o campos mal interpretados
- detectar nulos: localizar pérdidas de calidad
- filtrar filas: quedarse con una parte relevante del problema
- crear una columna derivada: transformar datos crudos en información más útil
- agrupar y resumir: empezar a leer patrones colectivos y no solo casos individuales

## Ejemplo teórico corto: dato crudo vs dato treballable

Imaginamos una tabla de ventas con estas columnas:

- `ingressos`
- `cost`
- `canal`
- `devolucions`

El dato crudo solo informa que esos campos existen.

Cuando añadimos:

- `marge = ingressos - cost`
- una agrupación por `canal`
- una lectura de devoluciones medianas

ya estamos convirtiendo dato en estructura de decisión.

## Ejemplo guiado de clase

Situación:

en `Predicció Comercial`, queremos saber qué canales tienen más devoluciones.

```python
resum = (
    df.groupby("canal")["devolucions"]
    .mean()
    .sort_values(ascending=False)
)
```

Aquí el alumnado tiene que entender dos cosas:

- que el dato todavía se está leyendo, no juzgando
- que una mesa resumen puede orientar mucho antes de entrenar ningún modelo

Y todavía una tercera:

- que una buena lectura agregada ya puede dar valor de negocio aunque no haya ningún modelo predictivo detrás

## Error habitual

Saltar demasiado rápido al modelado sin revisar antes tipo, nulos o columnas mal codificadas.

También es habitual:

- encadenar muchas transformaciones sin entender cada paso
- no conservar una columna original antes de transformarla
- mezclar limpieza, cálculo y conclusiones sin orden

Por eso conviene que el notebook haga visible el hilo:

1. qué tenemos
2. qué está mal o es dudoso
3. qué transformamos
4. qué aprendemos después de transformar

## Práctica guiada sugerida

Situación:

cogéis un dataset de ventas o de incidentes y feudo cuatro bloques explícitos:

1. tipo y valores nulos
2. correcciones o exclusiones
3. columna derivada útil
4. resumen agrupado que permita una lectura inicial

Al final, el alumnado tendría que escribir una conclusión corta:

`abans de modelar, ja hem vist que...`

Esta frase es muy valiosa porque obliga a entender que el trabajo de datos no empieza en el modelo, sino antes.

## Mini práctica

Pregunta:

si quieres calcular la media de ventas por segmento y compararla, qué paquete te ayuda más directamente?

## Solución orientativa

`Pandas`, porque estamos trabajando con datos tabulares y agrupaciones por columna.

## Resumen final

`NumPy` y `Pandas` no son un extra. Son la capa operativa que conecta ficheros, exploración, modelado y explicación.

Cuando esta capa es sólida, el alumnado deja de depender tanto de recetas y empieza a entender de verdad que está haciendo con los datos.

## Cómo usarla en estos proyectos

- BiciTierra Market: lectura, limpieza, agrupación y preparación del dataset
- P3: validación simple de lecturas y agregaciones temporales básicas
- P5: consolidación de datos antes de pasar a analítica o integración

## Ved también

- `Python i Entorns`
- `EDA i Qualitat de Dades`
- `Visualitzacio amb Matplotlib i Seaborn`
- `Scikit-Learn Introductori`


## Para profundizar

[Obri la píndola ampliada, amb pràctiques i exercicis](../PINDOLES_AMPLIADES/docs/python-per-a-dades-i/index.md).
