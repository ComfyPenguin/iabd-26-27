---
title: Scikit-Learn Introductorio
---

# Scikit-Learn Introductorio

## Qué aporta de verdad

`scikit-learn` permite montar un pipeline de modelado clásico de manera rápida y bastante coherente.

Es especialmente útil para:

- preprocessament
- partición train/maceta
- modelos baseline
- métricas
- comparación de alternativas

La gran virtud de `scikit-learn` en contexto docente es que obliga a pensar el modelado como un proceso ordenado y no como una sucesión improvisada de pruebas.

## Pipeline mínimo que hay que entender

1. preparar datos
2. separar `features` y `target`
3. hacer `train_test_split`
4. entrenar un modelo base
5. evaluarlo

Este pipeline es más importante que cualquier modelo concreto. Si el alumnado entiende bien esta secuencia, después podrá cambiar de modelo con mucho más criterio.

## Qué es realmente una baseline

Una baseline no es "el modelo final pero más simple".

Es una referencia inicial que nos permite responder:

- estamos mejorando o no?
- el problema parece abordable con esta información?
- los datos tienen una estructura aprovechable?

Por eso una baseline modesta pero muy leída es mucho más valiosa que un modelo más complejo mal justificado.

## Separar datos y evitar autoengaño

La división `train/test` no es un trámite técnico. Es una manera de evitar que el modelo se juzgue sobre los mismos ejemplos con que ha aprendido.

Sin esta separación, el modelo puede paréixer muy bueno y ser en realidad muy frágil fuera del notebook.

## Ejemplo corto

```python
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

model = RandomForestClassifier(random_state=42)
model.fit(X_train, y_train)

pred = model.predict(X_test)
print(classification_report(y_test, pred))
```

La lectura docente de este ejemplo no es "ya sabemos usar `RandomForest`". La lectura es:

- hemos separado datos
- hemos entrenado un modelo reproducible
- hemos obtenido una salida de evaluación legible

Es una puerta de entrada clara al trabajo serio.

## Qué tiene que entender el alumnado

- que la baseline es un punto de partida, no una excusa para cerrar el problema demasiado pronto
- que una métrica suelta no basta
- que sin buen preprocessament es fácil tener resultados engañosos

También conviene que entienda:

- que una mejora pequeña pero coherente puede ser más valiosa que una gran mejora dudosa
- que el modelo no se juzga solo por acierto, sino por lectura, estabilidad y utilidad

## Métricas: no todas cuentan la misma historia

Según el problema, no es igual mirar:

- `accuracy`
- `precision`
- `recall`
- `F1`

En algunos contextos, fallar una alerta grave es peor que lanzar alguna falsa alarma. En otros, saturar de alertas también es un problema.

Por eso la métrica tiene que conectar con el impacto del caso de uso, no solo con la facilidad de cálculo.

## Cuando encaja muy bien al curso

- clasificación de tickets o incidencias
- predicción comercial
- detección simple de riesgo
- segmentación inicial

## Error habitual

Cambiar de modelo una y otra vez sin controlar primero la calidad de los datos, el corte train/maceta o la lectura de las métricas.

También es habitual:

- comparar modelos entrenados sobre preparaciones diferentes sin documentarlo bien
- presentar el mejor resultado sin explicar por qué es mejor
- confundir segmentación no supervisada con clasificación supervisada

## Ejemplo guiado de clase

Situación:

en `Predicció Comercial`, dos grupos tienen modelos parecidos en `accuracy`, pero uno detecta mejor los casos que realmente interesan a negocio.

La pregunta docente no es solo qué número es más alto.

La pregunta es:

- qué error hace cada modelo?
- qué error es más costoso?
- qué métrica lo refleja mejor?

Aquí es donde `scikit-learn` ayuda no solo a entrenar, sino a estructurar la comparación.

## Práctica guiada sugerida

1. haced una baseline simple
2. guardáis la métrica principal
3. probáis un modelo alternativo
4. comparáis no solo el resultado, sino también el comportamiento de los errores

La conclusión final tendría que tener una frase así:

`preferim este model perquè...`

Si el alumnado no puede completar bien esta frase, todavía no está comparando con criterio suficiente.

## Mini práctica

Pregunta:

por qué conviene hacer `train_test_split` antes de entrenar?

## Solución orientativa

Porque necesitamos comprobar si el modelo generaliza sobre datos no vistos y no solo si memoriza el conjunto de entrenamiento.

## Resumen final

`scikit-learn` es la vía más clara para enseñar ML clásico de manera ordenada y transferible a proyectos reales.

El valor no está solo en las librerías que ofrece, sino en el hábito mental que impone: preparar, separar, entrenar, evaluar y justificar.

## Cómo usarla en estos proyectos

- P1: baseline de clasificación de tickets
- BiciTierra Market: comparativa de modelos y justificación
- P5: lectura predictiva simple antes de escenarios más integrados

## Ved también

- `Python i Entorns`
- `Python per a Dades I`
- `Validació de models`
- `XAI amb SHAP i LIME`


## Para profundizar

[Obri la píndola ampliada, amb pràctiques i exercicis](../PINDOLES_AMPLIADES/docs/scikit-learn-intro/index.md).
