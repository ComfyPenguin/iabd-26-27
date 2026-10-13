---
title: EDA I Calidad De Datos
---

# EDA I Calidad De Datos

## Qué es el EDA

`EDA` significa `Exploratory Data Analysis`. Es el análisis exploratorio inicial que basura antes de modelar.

El objetivo no es hacer gráficos para decorar, sino entender:

- qué tenemos
- que falta
- qué está mal
- qué puede explicar el problema de negocio

## Preguntas básicas

Antes de modelar conviene responder:

- qué variables hay?
- qué tipo de datos son?
- hay valores nulos?
- hay categorías extrañas?
- hay valores extremos?
- hay duplicados?

## Calidad de datos

La calidad de datos no es solo una cuestión técnica. Si los datos están mal, las decisiones de negocio también pueden estarlo.

Problemas habituales:

- campos vacíos
- formatos inconsistentes
- duplicados
- categorías mal escritas
- valores imposibles

## Ejemplo corto

Supone un dataset de clientes con estos casos:

- un cliente con `annual_spend` vacío
- dos filas con perfiles casi iguales
- `returns_ratio` muy alto en un solo caso

El EDA tendría que llevaros a preguntar:

- es un error de captura o un dato real?
- hay que imputar, eliminar o marcar el caso?
- ese extremo es informativo o es ruido?

## Ejemplo guiado de clase

Situación:

en `Predicció Comercial`, antes de modelar encontramos:

- un cliente con `annual_spend` vacío
- valores de retorno muy diferentes entre clientes
- canales y ciudades con comportamientos desiguales

Ruta razonable:

1. detectar qué campos tienen nulos
2. localizar si el problema es puntual o repetido
3. decidir si el dato se puede imputar, marcar o excluir
4. escribir el impacto que tendría ignorarlo

Conclusión docente:

una `EDA` buena no acaba en "ya hemos mirado los datos", sino en decisiones explícitas sobre que se considera fiable y que no.

## Mini práctica

Observa este fragmento:

```text
customer_id,annual_spend,orders_last_12m,returns_ratio
C018,,8,0.11
C020,3560,8,0.01
```

Pregunta:

Qué problemas detectas y qué decisiones podrías tomar antes de modelar?

## Solución orientativa

Problema principal:

- `annual_spend` está vacío a `C018`

Posibles decisiones:

- imputar si hay criterio razonable
- eliminar el caso si es residual y crítico para el modelo
- mantenerlo pero crear una bandera de valor ausente

La decisión correcta depende del contexto y del volumen de datos, pero el que no se puede hacer es ignorar el problema.

## Segunda mini practica

Tenso estas dos filas:

```text
C007,34,Oliva,ecommerce,3560,445,8,0.01,electric,medium
C020,34,Alzira,store,3560,445,8,0.01,electric,medium
```

Pregunta:

Por qué merece la pena revisar si hay duplicado funcional o si simplemente hay dos perfiles mucho pareguts?

## Solución orientativa

Porque si realmente es un duplicado, puede distorsionar el modelo o la segmentación. Si no lo es, puede ser solo una coincidencia de perfil.

El importante es no asumir automáticamente ni una cosa ni la otra sin revisar más contexto.

## Resumen final

Una buena `EDA` transforma un dataset desconocido en un conjunto de decisiones conscientes sobre calidad, estructura y riesgo.

## Cómo llevarlo a aula

- BiciTierra Market: exigir que cada problema detectado acabe en una decisión escrita
- P1: reaprofitar la misma lógica para ruido textual, casi duplicados y categorías confusas
- criterio docente: si hay gráficos o mesas pero no hay decisiones, la EDA todavía es superficial


## Para profundizar

[Obri la píndola ampliada, amb pràctiques i exercicis](../PINDOLES_AMPLIADES/docs/eda-i-qualitat-de-dades/index.md).
