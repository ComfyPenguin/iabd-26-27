---
title: MLflow Introductorio
---

# MLflow Introductorio

## Para que sirve

Cuando comparamos modelos, es muy fácil perder el hilo de:

- qué dataset hemos usado
- qué parámetros tenía cada prueba
- qué métrica ha dado mejor resultado

`MLflow` es una herramienta de seguimiento de experimentos.

Para alguien que empieza, se puede entender así: `MLflow` es una manera ordenada de no perder el rastro del que has probado.

No hay que pensarlo como una plataforma enorme. En este curso, el primer valor es muy simple: saber qué has comparado y por qué has elegido una prueba y no otra.

## Qué registra

- nombre del experimento
- parámetros
- métricas
- artefactos como gráficos o modelos guardados

En la práctica esto significa que, cuando vuelves el día siguiente, no tienes que confiar en la memoria para recordar qué prueba era mejor.

## Por qué es útil al curso

No hay que usarlo como una herramienta avanzada de producción para sacar provecho. Ya ayuda mucho si sirve para comparar pruebas de manera ordenada.

## Ejemplo guiado de clase

Situación:

en `Predicció Comercial`, un equipo prueba dos modelos y tres configuraciones, pero al jefe de dos días ya no recuerda qué combinación dio mejor resultado.

Sin seguimiento, suelen pasar cosas como:

- no saber qué dataset exacto se ha usado
- confundir métricas de pruebas diferentes
- repetir experimentos porque no han quedado registrados

Con una herramienta como `MLflow`, o con una disciplina parecida aunque sea manual, el equipo tendría que guardar:

- nombre de la prueba
- modelo utilizado
- parámetros principales
- métrica obtenida
- comentario corto sobre que se ha probado

Conclusión docente:

el valor no es solo técnico; es hacer que los experimentos sean comparables y defendibles.

## Ejemplo conceptual

Prueba A:

- modelo: `RandomForest`
- `max_depth = 5`
- `F1 = 0.74`

Prueba B:

- modelo: `RandomForest`
- `max_depth = 10`
- `F1 = 0.79`

Sin seguimiento, es fácil olvidar qué combinación exacta ha dado mejor resultado.

## Qué pasa si no lo registramos

Sin una disciplina de experimentos, es muy habitual:

- repetir pruebas ya hechas
- no poder justificar una elección ante el profesorado
- confundir una métrica de una prueba con otra
- enseñar el "mejor modelo" sin saber reproducirlo

Por eso, aunque la herramienta sea nueva, la idea de fondo es muy sencilla: cada prueba importante tiene que quedar identificada.

## Ejemplo muy simple de pensamiento experimental

Supone que pruebas:

- una baseline simple
- un árbol con profundidad pequeña
- un árbol con profundidad grande

El que quieres poder contestar después es:

- qué prueba era cada una
- con qué datos se hizo
- qué métrica dio
- cuál elegirías y por qué

`MLflow` ayuda exactamente a esto.

## Error habitual

Comparar modelos y después no saber reproducir la mejor prueba.

## Mini práctica

Pregunta:

Qué datos mínimos querrías guardar si compadres dos modelos para predecir ventas?

## Solución orientativa

Como mínimo:

- nombre del modelo
- parámetros principales
- métrica elegida
- versión o descripción del dataset

## Resumen final

`MLflow` ayuda a convertir pruebas dispersas en experimentos comparables y reproducibles.

## Cómo llevarlo a aula

- BiciTierra Market: usarlo como hábito de registro de experimentos aunque sea en nivel introductorio
- P5: usarlo si hay parte predictiva con varias iteraciones o modelos
- criterio docente: si el equipo no puede explicar qué prueba concreta está enseñando, el trabajo experimental todavía es débil

## Criterio docente ampliado

Si un equipo dice "este modelo es mejor" pero no puede decir qué ejecución, con qué parámetros y sobre qué dataset, todavía no ha cerrado bien la parte experimental.


## Para profundizar

[Obri la píndola ampliada, amb pràctiques i exercicis](../PINDOLES_AMPLIADES/docs/mlflow-intro/index.md).
