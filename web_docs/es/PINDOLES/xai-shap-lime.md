---
title: CORDERO Con SHAP I LIME
---

# CORDERO Con SHAP I LIME

## Por que importa

No basta con decir que un modelo acierta. Hay que poder explicar, al menos de forma aproximada, por qué variables ha tomado una decisio.

En entornos empresariales, una predicción sin explicación genera dos problemas:

- cuesta confiar en el modelo
- cuesta detectar si se esta equivocando por una razón relevante

Por eso la `XAI` no es solo un extra académico. Es una ayuda para leer el comportamiento del modelo y discutirlo con criterio.

Para quien empieza, la idea central es esta: una predicción sola dice "qué ha pasado"; una explicación intenta decir "por qué parece que ha pasado".

No es exactamente una ventana perfecta en el interior del modelo, pero sí una pista muy útil para revisar si el comportamiento parece coherente.

## Qué buscamos

- entender qué variables pesan mes
- detectar patrones sospechosos
- justificar decisiones ante dirección o cliente

En otras palabras, no queremos solo ver una gráfica bonita. Queremos poder discutir si el modelo está basándose en factores que tienen sentido.

## Explicación global y explicación local

No todas las explicaciones responden la misma pregunta.

Esta diferencia es muy importante porque el alumnado a menudo confunde las dos cosas.

Una explicación global habla del modelo en general.

Una explicación local habla de un caso concreto.

Si no distinguimos esto, podemos extraer conclusiones erróneas a partir de una sola predicción o, a la inversa, querer justificar un caso concreto solo con una importancia general de variables.

### Explicacio global

Intenta responder:

"En general, qué variables son más importantes en este modelo?"

Ejemplo:

- en un modelo de ventas, pueden pesar mucho `marketing_spend`, `avg_discount` y `website_sessions`

### Explicacio local

Intenta responder:

"Por qué este caso concreto ha recibido esta predicción?"

Ejemplo:

- un cliente concreto queda en un segmento premium porque tiene `annual_spend` alto, `returns_ratio` bajo y `orders_last_12m` elevado

## Dos enfoques conocidos

- `LIME`: explica predicciones locales aproximando el comportamiento del modelo.
- `SHAP`: reparte contribuciones de las variables a una predicción.

## Como entender LIME

`LIME` mira un caso concreto y construye una aproximación simple del modelo a su alrededor.

La idea no es describir todo el modelo, sino explicar aquella predicción concreta.

Es útil cuando queremos entender por qué el modelo ha clasificado un cliente, un ticket o una incidencia de una determinada manera.

## Como entender SHAP

`SHAP` reparte la predicción entre las variables de entrada, como si cada variable aportara una parte del resultado final.

En la práctica, esto permite decir cosas como:

- esta venta se predice alta porque hay mucho tráfico web y descuento moderado
- esta incidencia se considera grave por la cola elevada y la baja disponibilidad de grúas

La utilidad didáctica es que obliga a traducir el modelo a lenguaje legible.

No basta con decir "la gráfica muestra importancias". Hay que poder decir:

- qué variable empuja hacia arriba
- cuál frena la predicción
- si esto encaja con el negocio o con la operación

## Ejemplos conceptuales

Supone un modelo que predice ventas mensuales.

Una predicción concreta podría ser `71 unitats`.

Lectura tipo `SHAP`:

- `website_sessions` aporta `+9`
- `marketing_spend` aporta `+6`
- `stock_units` aporta `-3`
- `avg_discount` aporta `+2`

No quiere decir que el modelo "hace exactamente esa suma" de forma humana, pero sí que nos da una lectura útil de hacia donde empuja cada variable.

## Aplicacio al curso

- predicción comercial
- clasificación de tickets
- proyecto integrador final

## Ejemplo guiado de clase

Situación:

en `Predicció Comercial`, dos equipos tienen modelos pareguts en métrica, pero solo uno sabe explicar por qué predice mejor ventas altas.

Lectura posible con `SHAP`:

- `website_sessions` empuja hacia arriba
- `marketing_spend` empuja hacia arriba
- `stock_units` limita la predicción

Que tiene que hacer el alumnado:

1. mirar si las variables importantes tienen sentido de negocio
2. detectar si hay alguna sospechosa o sorprendente
3. escribir una conclusión corta: "el modelo parece confiar sobre todo en..."

Conclusión docente:

no queremos que memoricen la librería, sino que sepan leer si el modelo está razonando de forma creíble.

## Qué nos ayuda a detectar

- variables que aparentemente no tendrían que pesar tanto
- patrones posibles al bies
- comportamientos incoherentes con el negocio
- diferencias entre aquello que esperábamos y aquello que realmente usa el modelo

## Ejemplo teórico corto de lectura incorrecta y correcta

Lectura incorrecta:

- `website_sessions` es la variable más importante, por lo tanto siempre explica todas las ventas

Por qué es incorrecta:

- una importancia global no explica necesariamente todos los casos

Lectura más correcta:

- `website_sessions` parece ser una de las variables que más peso tienen en general, pero hay que mirar también casos concretos para ver como actúa junto con otros factores

Esta diferencia pare pequeña, pero es clave por no simplificar demasiado el modelo.

## Error habitual

Pensar que si una herramienta de `XAI` peine un gráfico bonito, ya hemos entendido el modelo.

Hay que interpretar la explicación dentro del contexto del problema.

## Mini práctica

Imaginamos un modelo de segmentación o clasificación de clientes con estas variables:

- `annual_spend`
- `orders_last_12m`
- `returns_ratio`

Caso concreto:

- `annual_spend = 4020`
- `orders_last_12m = 8`
- `returns_ratio = 0.00`

Pregunta:

Qué variables esperaríais que empujaron el modelo hacia un segmento de cliente de alto valor?

## Solución orientativa

- `annual_spend` alto: tendría que empujar claramente hacia valor alto
- `orders_last_12m` elevado: refuerza la idea de cliente de activo
- `returns_ratio` nulo: ayuda positivamente porque no hay señal de comportamiento problemático

Conclusión:

Si una explicación real mostrara que `returns_ratio` es casi el único factor relevante y `annual_spend` pesa poco, habría que revisar el modelo o los datos.

## Práctica guiada larga

### Objetivo

Aprender a leer una explicación de modelo sin confundir visualización con comprensión real.

### Situación

En `Predicció Comercial`, dos equipos tienen modelos con métricas paregudes. Ahora hay que decidir qué modelo es más creíble para presentarlo a dirección.

###  1. Empezar por la pregunta correcta

Antes de mirar ninguna gráfica, hay que preguntar:

- qué intenta predecir el modelo?
- qué variables esperaríamos que influyeron?
- hay factores que serían sospechosos si pesaron demasiado?

Por ejemplo, si predim ventas, sería lógico esperar pes en:

- `website_sessions`
- `marketing_spend`
- `stock_units`
- `avg_discount`

###  2. Distinguir lectura global y local

Ahora hacéis esta separación:

- lectura global: qué variables pesan más en general?
- lectura local: por qué este caso concreto ha recibido esta predicción?

El alumnado tiene que practicar las dos cosas por separado.

###  3. Simular una lectura global

Suponéis que una salida global dice:

- `website_sessions` pesa mucho
- `marketing_spend` pesa bastante
- `stock_units` pesa de manera moderada

Preguntas:

- esto parece coherente con el negocio?
- hay alguna variable que sorprende demasiado?
- falta alguna variable que esperabais importante?

###  4. Simular una lectura local

Ahora suponéis un caso concreto con predicción alta.

Lectura local:

- `website_sessions` empuja hacia arriba
- `marketing_spend` empuja hacia arriba
- `stock_units` frena un poco

El alumnado tendría que escribir una frase como esta:

`La predicció és alta sobretot perquè hi ha molt trànsit i inversió comercial, tot i que l'estoc limita una part del potencial.`

###  5. Detectar lectura mala

Ahora hacéis el ejercicio contrario.

Si el modelo muestra que la variable más importante es una que no tiene muy sentido de negocio, como por ejemplo un identificador o un campo sospechoso, qué podría estar pasando?

Posibles respuestas:

- datos mal preparados
- fuga de información
- modelo que está aprovechando un patrón accidental

###  6. Cerrar con una conclusión defendible

El alumnado tendría que acabar escribiendo:

- una conclusión global sobre el modelo
- una lectura de un caso concreto
- una duda o riesgo que convendría revisar

### Qué tendría que aprender el alumnado

- que `XAI` no es decorar el modelo con una gráfica
- que una explicación sirve para discutir credibilidad
- que hay que pasar de la visualización al lenguaje de negocio o de operación

### Criterio docente

Si el alumnado puede traducir una salida tipo `SHAP` o `LIME` a una explicación verbal creíble, está dando el paso importante.

## Riesgo

Confundir "explicación aproximada" con "prueba absoluta" del funcionamiento interno.

También es un riesgo pensar que `XAI` arregla un modelo malo. No lo hace.

Si los datos están mal, si hay fuga de información o si el modelo está mal planteado, la explicación puede continuar existiendo, pero no convertirá mágicamente la solución en fiable.

## Resumen final

`XAI` no elimina la complejidad del modelo, pero la hace discutible. Nos permite pasar de "el modelo dice esto" a "el modelo dice esto por estas razones aproximadas, y ahora podemos valorar si tienen sentido".

## Cómo llevarlo a aula

- BiciTierra Market: pedir siempre una lectura en lenguaje de negocio, no solo la gráfica
- P5: usarla para discutir si un indicador de riesgo tiene sentido operativo
- criterio docente: si el alumnado no sabe pasar de la visualización a una explicación verbal defendible, todavía no ha interiorizado la píldora

## Cómo llevarlo a aula

- BiciTierra Market: exigir al menos una explicación global y una lectura corta de un caso concreto
- P5: usar `XAI` para justificar indicadores de riesgo o cuellos de botella
- criterio docente: si el alumnado solo enseña la gráfica y no lo interpreta en lenguaje de negocio, la píldora no está muy aprovechada


## Para profundizar

[Obri la píndola ampliada, amb pràctiques i exercicis](../PINDOLES_AMPLIADES/docs/xai-shap-lime/index.md).
