---
title: Clustering I Segmentación
---

# Clustering I Segmentación

## Idea clave

Segmentar es agrupar casos parecidos cuando no tenemos un hashtag previo.

En negocio, esto es útil para descubrir perfiles de clientes, patrones de uso o comportamientos parecidos.

## No es magia

Un algoritmo de `clustering` no "descubre la verdad". El que hace es agrupar según la representación de los datos que le damos.

Por eso el resultado depende de:

- las variables elegidas
- el escalado
- el algoritmo
- los parámetros

## Dos casos conocidos

### K-Means

Agrupa alrededor de centros.

Es útil cuando esperamos grupos relativamente compactos.

### DBSCAN

Agrupa por densidad.

Es útil cuando hay ruido o cuando no queremos forzar todos los puntos a pertenecer a un clúster.

## Error habitual

Pensar que el número de grupos sale automáticamente "bien" sin justificación.

## Ejemplo conceptual

Si segmentamos clientes con:

- `annual_spend`
- `avg_order_value`
- `orders_last_12m`

podemos encontrar perfiles como:

- clientes ocasionales de bajo valor
- clientes frecuentes de valor mediano
- clientes premium con gasto alto

## Ejemplo guiado de clase

Situación:

en `Predicció Comercial`, el equipo quiere segmentar clientes para decidir campañas y priorización comercial.

Variables iniciales razonables:

- `annual_spend`
- `avg_order_value`
- `orders_last_12m`
- `returns_ratio`

Que hay que hacer antes del algoritmo:

1. revisar si hay nulos o casos extraños
2. decidir qué variables explican mejor el valor del cliente
3. pensar qué perfiles comerciales esperaríamos encontrar

Que hay que hacer después:

1. mirar si los grupos tienen sentido legible
2. posarlos nombre de negocio, no solo número de clúster
3. decidir qué acción comercial haría cada equipo con cada segmento

Conclusión docente:

si el grupo no se puede interpretar ni convertir en decisión, la segmentación todavía no está muy cerrada.

## Mini práctica

Tenso tres clientes:

- A: `annual_spend=720`, `orders=7`
- B: `annual_spend=3560`, `orders=8`
- C: `annual_spend=4020`, `orders=8`

Pregunta:

Qué perfil parece más próximo a un segmento premium? Qué podría quedar separado del grupo de más valor?

## Solución orientativa

- `B` y `C` parecen candidatos a segmentos de alto valor
- `C` todavía más, porque el gasto es superior
- `A` quedaría previsiblemente en un segmento de valor bajo u ocasional

## Resumen final

El `clustering` es útil cuando ayuda a construir una lectura accionable del negocio, no cuando solo genera grupos difíciles de interpretar.

## Cómo llevarlo a aula

- BiciTierra Market: hacer escribir primero segmentos esperados y después compararlos con el resultado real
- criterio docente: el segmento tiene que acabar en una acción, una recomendación o una prioridad comercial


## Para profundizar

[Obri la píndola ampliada, amb pràctiques i exercicis](../PINDOLES_AMPLIADES/docs/clustering-i-segmentacio/index.md).
