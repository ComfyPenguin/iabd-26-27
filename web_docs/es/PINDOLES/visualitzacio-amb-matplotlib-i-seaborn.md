---
title: Visualitzacio Con Matplotlib I Seaborn
---

# Visualitzacio Con Matplotlib I Seaborn

## Para que sirve esta capa

Antes de explicar un modelo, a menudo necesitamos ver el comportamiento de los datos.

`Matplotlib` y `Seaborn` no son solo para hacer gráficos bonitos. Sirven para detectar patrones, anomalías y relaciones que después condicionan el resto del trabajo.

También tienen un valor más profundo: obligan a mirar los datos antes de opinar sobre ellas. Muchas conclusiones apresuradas caen cuando una distribución, un outlier o una separación de grupos se hacen visibles.

## Como repartir los papeles

- `Matplotlib`: base general para dibujar
- `Seaborn`: gráficos estadísticos más rápidos y legibles

La idea no es enfrentarlas. De hecho, muchas veces conviven:

- `Seaborn` para sacar una primera lectura clara
- `Matplotlib` para ajustar títulos, ejes, tamaño o composición final

## Visualizar es leer, no decorar

En contexto docente es importante repetirlo porque hay un error muy habitual: interpretar el gráfico como una ilustración final del trabajo.

En realidad, la visualización entra mucho antes:

- para detectar errores de calidad
- para ver patrones inesperados
- para decidir si hay que transformar variables
- para justificar una decisión analítica ante el profesorado o de una dirección ficticia

## Por qué un gráfico puede ser mejor que una mesa

Una mesa puede contener la información, pero no siempre muestra bien:

- dispersión
- patrones
- outliers
- comparaciones rápidas entre grupos

Por eso la visualización no es adorno. Es un instrumento de diagnóstico.

También es una herramienta de comunicación, pero conviene que la comunicación llego después del diagnóstico y no a la inversa.

## Ejemplo mínimo

```python
import matplotlib.pyplot as plt
import seaborn as sns

sns.boxplot(data=df, x="canal", y="vendes")
plt.xticks(rotation=45)
plt.show()
```

## Gráficos que más rendimiento dan al curso

- histograma
- boxplot
- countplot
- scatterplot
- heatmap de correlación

No porque sean los únicos posibles, sino porque dan mucha información con una curva de aprendizaje razonable.

## Antes de dibujar: qué pregunta quieres responder?

Este paso parece obvio, pero es el que más falta hace en aula.

Preguntas tipo:

- cómo se distribuye esta variable?
- hay grupos muy diferentes?
- hay valores extremos?
- dos variables paren relacionadas?
- hay demasiadas categorías porque el gráfico sea legible?

Si el alumnado no puede formular la pregunta, el gráfico suele acabar siendo ruido visual.

## Qué pregunta responde cada uno

- histograma: cómo se distribuye una variable?
- boxplot: hay outliers o diferencias entre grupos?
- scatterplot: dos variables se mueven juntas?
- heatmap: qué variables parecen relacionadas?

Aquí hay una lección importante: un buen gráfico no es lo más vistoso, sino el que responde una pregunta concreta.

## Lectura responsable de un gráfico

También conviene enseñar que NO se puede concluir demasiado rápido.

Por ejemplo:

- un `scatterplot` sugiere relación, pero no prueba causalidad
- una correlación alta no significa que una variable explico por sí sola la otra
- un boxplot con diferencias visibles no evita la necesidad de contexto o calidad de datos

## Ejemplo teórico corto: mismo dataset, preguntas diferentes

Con un mismo conjunto de ventas podemos querer saber:

- cómo se distribuye `vendes`
- si hay outliers por `canal`
- si `descompte` y `marge` parecen relacionados

Ninguno de estas preguntas no pide exactamente el mismo gráfico.

Por eso no conviene elegir visuales "por costumbre", sino según la lectura que queremos hacer.

## Cuando `Seaborn` ayuda mucho y cuando hay que bajar a `Matplotlib`

`Seaborn` ayuda mucho cuando queremos:

- salir rápidamente con una visual estadística clara
- trabajar con `DataFrame`
- comparar grupos sin demasiada configuración inicial

`Matplotlib` suele ser más útil cuando necesitamos:

- ajustar detalle fino
- combinar varios gráficos
- controlar mejor esos, anotaciones o formato final

Esta distinción es útil porque evita que el alumnado vea las dos librerías como competidoras estrictas.

## Ejemplo guiado de clase

Situación:

en `Predicció Comercial`, un equipo quiere saber si el descuento está relacionado con un margen peor.

Un `scatterplot` o un `boxplot` por tramos de descuento puede dar una lectura inicial mucho mejor que una intuición verbal.

También puede permitir una conversación más madura:

- el margen cae siempre o solo en ciertos tramos?
- hay canales donde el patrón es diferente?
- tenemos outliers que pueden distorsionar la lectura?

En una buena sesión de clase, el gráfico no se cierra con `ací està la figura`, sino con una frase de lectura:

`pareix que a partir d'un cert tram de descompte el marge es torna molt més inestable, especialment en certs canals.`

## Error habitual

Llenar el notebook de gráficos sin decir qué está mirando cada uno ni qué decisión puede afectar.

También es habitual:

- no etiquetar bien los ejes
- posar demasiadas categorías juntas y perder legibilidad
- interpretar una correlación visual como si ya fuera causalidad

También es habitual:

- elegir un tipo de gráfico inadecuado por la pregunta
- usar colores o formatos que dificultan la lectura
- mostrar el mismo patrón varias veces sin añadir información nueva

## Práctica guiada sugerida

Cogéis una variable numérica y una categórica de vuestro proyecto.

1. haced un histograma de la variable numérica
2. haced un boxplot por categoría
3. escribís qué aporta el segundo gráfico que no veis tan bien al primero

Después, añadís una segunda variable numérica y feudo un `scatterplot`.

El objetivo no es acumular gráficos, sino aprender a justificar por qué cada visual aporta una lectura distinta.

## Práctica guiada larga

### Objetivo

Aprender a usar `Matplotlib` y `Seaborn` como herramientas de lectura y justificación analítica, no solo de presentación final.

### Situación

Suponéis un notebook de `Predicció Comercial` con variables de ventas, canal, descuento, margen y devoluciones.

###  1. Empezar por una variable numérica

Elegís una variable como `vendes` o `marge` y feudo un histograma.

La pregunta docente es:

- la distribución pare simétrica?
- hay cola larga?
- hay concentración en tramos concretos?

###  2. Pasar a comparación entre grupos

Ahora hacéis un `boxplot` por `canal` o por segmento.

Explicáis:

- si hay grupos diferentes
- si hay outliers
- si la dispersión es parecida o muy desigual

###  3. Probar una relación entre variables

Haced un `scatterplot` entre dos variables como `descompte` y `marge`.

No cerráis la lectura con una afirmación demasiado fuerte. Usáis una formulación prudente:

- `pareix que...`
- `es veu una possible relació...`
- `caldria revisar si...`

###  4. Pensar en qué visuales no merece la pena insistir

Aquí hay una parte muy madura:

- qué gráficos no aportan casi nada nuevo?
- qué solo repiten una lectura ya hecha?
- cuáles serían demasiado ruidosos para un dashboard?

###  5. Cerrar con una decisión analítica

El equipo tendría que acabar con 2 o 3 frases como:

- `cal revisar outliers en este canal`
- `pareix que el descompte excessiu empitjora el marge en certs casos`
- `esta variable necessita transformació o segmentació`

### Criterio docente

La práctica está bien resuelta si cada gráfico acaba en una lectura útil y si el alumnado sabe explicar por qué lo ha elegido y que  concluye con prudencia.

## Mini práctica

Pregunta:

si quieres ver rápidamente si una variable tiene valores extremos, qué gráfico elegirías primero?

## Solución orientativa

Un `boxplot`, porque ayuda a detectar dispersión y posibles outliers de manera muy directa.

## Resumen final

Visualizar no es decorar. Es una manera de pensar mejor antes de transformar o modelar.

Si el alumnado aprende a mirar bien una distribución, un outlier o una relación sospechosa, después entenderá mucho mejor por qué modela de una forma o de otra.

## Cómo usarla en estos proyectos

- BiciTierra Market: EDA y justificación de decisiones de limpieza
- P3: series simples y evolución de lecturas
- P5: análisis exploratorio antes de una lectura ejecutiva o predictiva

## Ved también

- `Python per a Dades I`
- `EDA i Qualitat de Dades`
- `Dashboards i PowerBI`


## Para profundizar

[Obri la píndola ampliada, amb pràctiques i exercicis](../PINDOLES_AMPLIADES/docs/visualitzacio-amb-matplotlib-i-seaborn/index.md).
