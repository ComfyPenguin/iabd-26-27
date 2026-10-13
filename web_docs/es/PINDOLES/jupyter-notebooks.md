---
title: Jupyter Notebooks
---

# Jupyter Notebooks

## Para que sirven

Un `Jupyter Notebook` es muy útil cuando queremos combinar en un mismo espacio:

- explicación
- código
- pruebas rápidas
- resultados
- gráficos

Dedo de una manera muy simple: un notebook es como una libreta técnica donde podemos contar qué estamos haciendo, ejecutarlo y ver el resultado sin salir del mismo lugar.

Por eso es tan útil cuando todavía estamos pensando, probando o justificando decisiones.

## Qué veréis realmente dentro de un notebook

Normalmente hay dos tipos de celdas:

- celdas de texto o `markdown`, donde explicamos qué queremos hacer
- celdas de código, donde cargamos datos, hacemos cálculos o probamos modelos

La idea buena no es alternar texto y código porque quedo bonito, sino porque cualquier persona pueda seguir el hilo del trabajo.

En este curso son especialmente útiles para:

- EDA
- limpieza de datos
- pruebas de clasificación
- validación de modelos
- experimentos rápidos con texto o series temporales

## Cuando es mejor un notebook

- cuando estamos explorando
- cuando queremos justificar una decisión con pasos visibles
- cuando todavía no tenemos una librería o aplicación cerrada

## Cuando no es el mejor formato

- cuando ya tenemos lógica estable y reusable
- cuando el código tiene que pasar a producción
- cuando necesitamos mucha modularitat

Un error muy común es pensar que, si un notebook ya funciona, ya es la solución final. No es así.

Un notebook suele ser un espacio de prueba, exploración o demostración. Si la lógica se vuelve estable, después conviene pasarla a scripts, módulos o aplicaciones más limpias.

## Buenas prácticas mínimas

- posar títulos y subtítulos claros
- separar exploración, transformación y conclusiones
- no dejar celdas rotas
- documentar qué se quiere demostrar con cada bloque

## Estructura recomendada

1. objetivo del notebook
2. carga de datos
3. calidad y EDA
4. tratamiento de datos
5. prueba o modelo
6. conclusiones

## Ejemplo guiado de clase

Situación:

en `Predicció Comercial`, queremos saber si las ventas parecen depender del canal, del descuento y de las sesiones web.

Un notebook razonable podría tener:

1. una celda que explico la pregunta de negocio
2. una celda que cargo `sales_history.csv`
3. una celda que reviso nulos y tipos de dato
4. una celda que comparo ventas por canal
5. una celda final con una conclusión corta sobre que parece importante

El valor del notebook no es solo el código. Es que permite ver el camino entre la pregunta inicial y la conclusión.

## Como saber si un notebook está muy hecho

Si otra persona lo abre, tendría que poder entender:

- qué pregunta intenta responder
- qué datos está usando
- qué ha encontrado
- y qué decisión o siguiente paso propone

Si solo ve código suelto y mesas sin contexto, el notebook todavía es débil.

## Error habitual

Convertir el notebook en una sucesión de pruebas inalámbrica conductor.

## Mini práctica

Pregunta:

Si tienes un notebook para predicción comercial, qué dos secciones no tendrían que faltar nunca antes del modelado?

## Solución orientativa

- una sección de carga y comprensión de datos
- una sección de calidad/EDA antes de modelar

## Resumen final

El notebook es una herramienta de pensamiento visible. Tiene mucho valor si ayuda a entender el camino que puerta a una decisión técnica.

## Cómo usarlo en estos proyectos

- P1: explorar tickets y probar una baseline antes del `RAG`
- BiciTierra Market: separar EDA, modelado y segmentación con conclusiones cortas al final de cada bloque
- P3: validar rangos y alertas antes de desplegar demasiada infraestructura
- P4: comprobar reglas de validación sobre la cola de entrada antes de automatizar
- P5: probar una lectura de riesgo o una analítica puntual antes de la integración global

## Criterio docente rapid

Si el profesorado puede leer el notebook de arriba abajo y entender el problema, la prueba y la conclusión sin tener que adivinar el hilo, el notebook ya está haciendo bien su función.


## Para profundizar

[Obri la píndola ampliada, amb pràctiques i exercicis](../PINDOLES_AMPLIADES/docs/jupyter-notebooks/index.md).
