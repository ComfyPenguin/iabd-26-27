---
title: Validación De Modelos
---

# Validación De Modelos

## Idea clave

Un modelo no es bueno porque "funciono" con los datos de entrenamiento. Hay que comprobar si generaliza.

Si un modelo memoriza demasiado el conjunto de entrenamiento, puede dar resultados aparentemente muy buenos dentro de clase pero fallar cuando recibe datos nuevos. Validar es comprobar si aquello que hemos aprendido sirve fuera de los datos que ya coneixiem.

## Minims que siempre tenéis que hacer

- separar `train` y `test`
- usar una métrica coherente con el problema
- comparar al menos un baseline con una opción mejorada

## Qué es separar `train` y `test`

El conjunto `train` sirve para aprender.

El conjunto `test` sirve para comprobar si el modelo generaliza.

Si mezclamos los dos papeles, la evaluación deja de ser fiable.

## Baseline

Un `baseline` es una referencia simple. Nos ayuda a responder una pregunta básica:

"El modelo realmente aporta valor o solo parece sofisticado?"

Por ejemplo:

- en clasificación, un baseline podría ser la clase mayoritaria
- en regresión, una predicción fija como la media

Si el modelo no mejora claramente el baseline, hay que revisar el problema o los datos.

## Metriques habituales

- regresión: `MAE`, `RMSE`
- clasificación: `accuracy`, `precision`, `recall`, `F1`

## Como elegir métrica

No hay una métrica universal.

En clasificación:

- `accuracy` puede servir si las clases están equilibradas
- `precision` importa si no queremos demasiado falsos positivos
- `recall` importa si no queremos dejar casos importantes sin detectar
- `F1` combina `precision` y `recall`

En regresión:

- `MAE` es fácil de interpretar
- `RMSE` penaliza más los errores grandes

## Sobreajust y infraajust

`Overfitting` o sobreajust:

- el modelo aprende demasiado bien los datos de entrenamiento
- pero falla con datos nuevos

`Underfitting` o infraajust:

- el modelo es demasiado simple
- ni siquiera aprende correctamente el patrón principal

## Riesgos habituales

- fuga de información
- sobreajust
- datos mucho desbalancejades

## Fuga de información

Hay fuga de información cuando el modelo recibe, de una forma u otra, información que en la realidad no tendría disponible en el momento de predecir.

Ejemplos:

- crear variables con datos futuros
- normalizar con todo el dataset antes de separar `train` y `test`
- usar campos que son casi equivalentes a la respuesta

## Datos desbalancejades

Si una clase es mucho más frecuente que otra, el modelo puede dar una `accuracy` alta y todavía así ser inútil.

Por ejemplo, si el 95% de los casos son normales, un modelo que siempre diga "normal" puede parecer bueno, pero no detectará incidencias.

## Cross-validation

Cuando hay pocos datos, conviene usar `cross-validation` para comprobar si el comportamiento del modelo es estable y no depende demasiado de una única partición.

## Ejemplo guiado de clase

Situación:

en `Predicció Comercial`, un equipo presenta un modelo con muy buen resultado pero solo enseña una ejecución y ninguna comparación con baseline.

Que tendría que pasar:

1. separar claramente entrenamiento y prueba
2. construir una referencia simple, aunque sea la media o una regla básica
3. comparar el modelo nuevo contra esa referencia
4. mirar si la ganancia es real o solo aparente

Conclusión docente:

si no hay baseline ni maceta separada, todavía no hay evidencia suficiente para confiar en el modelo.

## Ejemplo de lectura de métrica

Supone estos resultados en regresión:

- baseline: `MAE = 14.8`
- modelo A: `MAE = 9.2`
- modelo B: `MAE = 9.0`

Lectura razonable:

- A y B mejoran claramente la baseline
- la diferencia entre A y B es pequeña
- si B es mucho más complejo, quizás A ya es bastante buena opción

El mejor modelo no es siempre el más sofisticado, sino el que mejora suficiente y se puede defender con criterio.

## Mini práctica

- Coge dos modelos.
- Compara los resultados sobre el mismo conjunto de maceta.
- Explica cuál elegirías y por que.

## Segunda mini práctica

Tenso un problema de clasificación con clases desbalancejades.

Resultados:

- modelo 1: `accuracy = 0.95`, `recall` de la clase crítica = `0.20`
- modelo 2: `accuracy = 0.89`, `recall` de la clase crítica = `0.81`

Pregunta:

Qué modelo parece más útil si el coste de no detectar la clase crítica es alto?

## Solución orientativa

El modelo 2 parece más útil, porque detecta mucho mejor la clase crítica aunque el `accuracy` global sea menor.

La lectura importante es esta:

- no todos los errores cuestan igual
- la métrica correcta depende del riesgo del problema

## Resumen final

Validar no es un trámite. Es el proceso que separa una demo aparentemente convincente de una solución técnica mínimamente fiable.

## Cómo llevarlo a aula

- BiciTierra Market: exigir siempre baseline, métrica coherente y lectura final de los errores
- P1: reaprofitar esta píldora cuando se compare una regla simple contra una clasificación más robusta
- P5: usarla si hay parte predictiva y hay que justificar por qué una alerta o riesgo es creíble


## Para profundizar

[Obri la píndola ampliada, amb pràctiques i exercicis](../PINDOLES_AMPLIADES/docs/validacio-de-models/index.md).
