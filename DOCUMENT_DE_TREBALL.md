# BiciTierra Market *· Documento de trabajo

**Proyecto inicial del curso *· Propuesta de seis semanas.** Este es el documento que tenéis que seguir. Las píldoras y los ficheros prácticos están a banda y se enlazan en la semana en que los necesitáis. El profesorado concretará las fechas según el calendario y el nivel del grupo.

**Equipo:** … *· **Miembros:** … *· **Repositorio:** …

| Semana | Fechas acordadas | Fecha de revisión o entrega |
|---|---|---|
| 1 | … | … |
| 2 | … | … |
| 3 | … | … |
| 4 | … | … |
| 5 | … | … |
| 6 | … | … |

## Como utilizar este documento

1. Localizáis la semana actual en el [mapa del projecte](#mapa-setmanal).
2. Leéis el objetivo y seguís las tareas en orden. Consultáis cada píldora cuando la tarea lo requiera.
3. Trabajáis con los materiales indicados y guardáis los resultados al repositorio del equipo.
4. Completáis la entrega semanal y el [registre de seguiment](#registre). Anotáis la revisión docente y el próximo paso.

Los notebooks son una base que tenéis que ejecutar, entender y adaptar. **No ejecutáis todo el notebook de modelado en la semana 2:** la prueba final se reserva para la semana 5, después de elegir el modelo. Las fechas de los CSV pertenecen al caso simulado; no son las fechas del calendario de clase.

## El reto de la empresa

BiciTierra Market combina tienda física y comercio electrónico. Quiere mejorar las decisiones sobre ventas, stock y campañas. La dirección plantea tres preguntas:

- Podemos anticipar las ventas mensuales con suficiente fiabilidad para planificar el stock?
- Qué perfiles de cliente tenemos y qué acciones comerciales tienen sentido para cada uno?
- Qué decisiones podemos justificar con los resultados y qué límites tienen?

**Producto final:** análisis de datos, predicción mensual justificada, segmentación interpretable, dashboard ejecutivo, memoria breve y defensa de **8–10 minutos**.

El objetivo predictivo de partida es `sales_units`: unidades vendidas durante un mes por canal, línea de producto y región. Otra variable objetivo se tiene que acordar con el profesorado y tiene que mantener una separación temporal correcta.

Tenéis que comparar al menos una referencia sencilla con un modelo alternativo, justificar una métrica principal, aplicar una técnica de segmentación y transformar los resultados en recomendaciones. Un gráfico o una métrica sin interpretación no cierra una tarea.

## Los datos y las reglas del caso

| Material | Qué contiene | Cuando se utiliza |
|---|---|---|
| [Històric de vendes](MATERIALS_PRACTICS/dades/sales_history.csv) | 1.188 filas, 18 series mensuales completas, enero de 2021–junio de 2026 | Semanas 1–3 y 5–6 |
| [Clients](MATERIALS_PRACTICS/dades/customers.csv) | 2.000 clientes, fotografía a 30/06/2026 | Exploración en semana 1; segmentación en semana 4 |
| [Entrada de predicció](MATERIALS_PRACTICS/dades/sales_forecast_input.csv) | 18 combinaciones de julio de 2026 sin resultado real | Semana 5, después de validar |
| [Diccionari](MATERIALS_PRACTICS/dades/DICCIONARI_DADES.md) | Camps, unidades, significado y disponibilidad | Antes de elegir variables y cuando haya dudas |
| [Referència tècnica de les dades](MATERIALS_PRACTICS/dades/README.md) | Generación sintética, cobertura, nulos y uso de los ficheros | Consulta durante el proyecto |

Todos los datos son **sintéticas para docencia**. Los clientes son una muestra independiente de las ventas agregadas: no hay una clave transaccional que permita unir las dos mesas hila a fila. No se espera que el gasto de los clientes sume los ingresos de ventas.

**Información disponible antes del mes:** canal, producto, región, calendario, presupuesto de marketing, descuento previsto, stock inicial y precio de tarifa. También se pueden usar retrasos de meses anteriores de la misma serie.

**Información conocida después del mes:** ventas, ingresos, visitas web, gasto real de marketing, descuento real, stock final y ruptura de stock. No usáis estos valores del mismo mes para anticipar las ventas.

| Partición | Periodo | Función |
|---|---|---|
| Histórico inicial | 2021 | Construir el retraso de 12 meses cuando se utilice |
| Entrenamiento | Hasta diciembre de 2024; efectivamente 2022–2024 con `lag_12` | Ajustar modelos y transformaciones |
| Validación | Gener–diciembre de 2025 | Comparar modelos y elegir parámetros |
| Prueba final | Gener–junio de 2026 | Evaluar después de fijar las decisiones; semana 5 |
| Predicción nueva | Julio de 2026 | Aplicar el modelo sin disponer todavía del resultado |

Separáis meses completos, sin mezclar filas aleatoriamente. Ajustáis la imputación y la escala solo con entrenamiento. La prueba representa predicciones sucesivas de un mes vista: para predecir junio, mayo ya se ha observado. No es una predicción simultánea de seis meses hecha en diciembre.

<a id="mapa-semanal"></a>
## Mapa del proyecto por semanas

| Semana | Foco | Material práctico principal | Evidencia de cierre |
|---|---|---|---|
| [1](#setmana-1) | Entorno, pregunta de negocio y EDA | Notebook 01 + histórico + clientes | Pregunta, dos hipótesis y diagnóstico inicial *· **CP1** |
| [2](#setmana-2) | Limpieza, partición temporal y referencias | Notebook 02, apartados 1–2 | Datos preparados, predictores justificados y baseline *· **CP2** |
| [3](#setmana-3) | Entrenamiento y comparación | Notebook 02, apartados 3–4 | Tabla de validación y elección provisional de modelo |
| [4](#setmana-4) | Segmentación y perfiles | Notebook 03 + clientes | Perfiles, acciones comerciales y modelo comparado *· **CP3** |
| [5](#setmana-5) | Prueba final, errores y explicación | Notebook 02, apartados 5–6 | Resultado de prueba reservada, límites y previsión de julio |
| [6](#setmana-6) | Dashboard, memoria y defensa | Materiales de dashboard + resultados propios | Paquete final y defensa *· **CP4** |

Cada semana tiene cuatro momentos: **preparar → desarrollar → comprobar → entregar**. Distribuidlos entre las sesiones reales con el profesorado. Las ampliaciones son opcionales y se hacen después del mínimo de la semana.

<a id="semana-1"></a>
## Semana 1 *· Entender el reto y explorar

**Objetivo:** formular una pregunta de negocio concreta y comprobar qué permiten responder los datos.

### Píldoras, en orden de uso

1. [Python i entorns](PINDOLES/python-i-entorns.md): preparación del entorno; refuerzo según el nivel inicial.
2. [Jupyter Notebooks](PINDOLES/jupyter-notebooks.md): abrir, ejecutar celdas y escribir conclusiones.
3. [Python per a dades](PINDOLES/python-per-a-dades-i.md): cargar CSV, seleccionar campos y resumir datos.
4. [EDA i qualitat](PINDOLES/eda-i-qualitat-de-dades.md): decidir qué revisar antes de modelar.

### Materiales que tenéis que abrir

- [Notebook 01 · EDA](MATERIALS_PRACTICS/notebooks/01_eda.ipynb).
- [Històric](MATERIALS_PRACTICS/dades/sales_history.csv), [clients](MATERIALS_PRACTICS/dades/customers.csv) y [diccionari](MATERIALS_PRACTICS/dades/DICCIONARI_DADES.md).

### Tareas

1. Acordáis el equipo y creáis el repositorio con el [estructura mínima](#repositori).
2. Escribís una pregunta principal y una de secundaria. Concretad qué decisión tomaría la dirección con la respuesta.
3. Preparáis Python, Jupyter, pandas y NumPy. Desde la carpeta `MATERIALS_PRACTICS/notebooks/`, ejecutáis la lectura inicial de los CSV.
4. Comprobáis qué representa cada fila, las claves, las fechas, los tipos y los nulos. Diferenciáis falta de dato y valor cero.
5. Usáis los patrones de entrenamiento para explorar diferencias entre canales, productos, regiones y meses. No usáis la prueba de 2026 para orientar decisiones de modelo.
6. Escribís dos hipótesis y ejemplos concretos que las motivan. Anotáis también que todavía no podéis concluir.

### Entrega y comprobación *· CP1

Completáis esta ficha en el repositorio:

| Apartado | Respondida del equipo |
|---|---|
| Pregunta principal y decisión de negocio | … |
| Pregunta secundaria | … |
| Variable objetivo y unidad de observación | … |
| Predictores iniciales y momento en que se conocen | … |
| Problemas de calidad detectados y ejemplos | … |
| Baseline, modelo alternativo y métrica que proponemos | … |
| Variables y técnica inicial de segmentación | … |
| Hipótesis 1 e hipótesis 2 | … |

Adjuntáis el notebook ejecutado con comentarios. Podéis avanzar cuando cualquier miembro explico la pregunta, la clave de una fila y al menos un problema de calidad.

<a id="semana-2"></a>
## Semana 2 *· Preparar los datos y establecer una referencia

**Objetivo:** tener una preparación reproducible y una primera predicción con qué comparar los modelos.

### Píldoras

- [Python per a dades](PINDOLES/python-per-a-dades-i.md): tratamiento de nulos, transformaciones y agrupaciones.
- [Visualització](PINDOLES/visualitzacio-amb-matplotlib-i-seaborn.md): representar distribuciones y justificar decisiones.
- [Validació de models](PINDOLES/validacio-de-models.md): métricas y separación de datos. En este proyecto prevalece el corte temporal indicado aquí sobre ejemplos genéricos de partición aleatoria.

### Materiales

- [Notebook 01](MATERIALS_PRACTICS/notebooks/01_eda.ipynb), para documentar calidad y decisiones.
- [Notebook 02 · Modelatge](MATERIALS_PRACTICS/notebooks/02_modelatge_baseline.ipynb): carga y **apartados 1–2**, incluida la comprobación de referencias de semana 2.
- [Diccionari](MATERIALS_PRACTICS/dades/DICCIONARI_DADES.md).

### Tareas

1. Por cada problema detectado, decidís conservar, corregir, imputar o excluir y escribís el motivo. Mantenéis los CSV originales intactos.
2. Ordenáis por serie y mes; comprobáis la cobertura antes de crear retrasos de uno y doce meses.
3. Separáis entrenamiento, validación y prueba. Guardáis fechas y recuento de filas de cada partición.
4. Elaboráis una tabla de variables admitidas y excluidas segundos si se conocen antes del mes. Justificáis especialmente la exclusión de ingresos y sesiones del mismo mes.
5. Calculáis la media histórica de grupo solo con entrenamiento y comparadla con la referencia del mismo mes del año anterior.
6. Mesuráis el error en validación: ***MAE en unidades** y ***WAPE en porcentaje**. Explicáis qué implica el error para el negocio.

### Entrega y comprobación *· CP2

- Notebook con preparación reproducible y decisiones de calidad.
- Tabla de particiones, lista de predictores y resultados de las referencias.
- Un ejemplo de como una mala imputación o una variable posterior podría falsear el resultado.

Paraos antes del apartado 3 del notebook 02. El conjunto de prueba queda reservado.

<a id="semana-3"></a>
## Semana 3 *· Comparar modelos

**Objetivo:** seleccionar una alternativa con evidencias de validación, sin mirar el resultado de la prueba final.

### Píldoras

- [Introducció a scikit-learn](PINDOLES/scikit-learn-intro.md): conceptos de ajuste, predicción y pipeline. El notebook base calcula ridge con NumPy; trasladarlo a scikit-learn es una ampliación.
- [Validació de models](PINDOLES/validacio-de-models.md): comparación justa, sobreajust y lectura de métricas.
- **Ampliación:** [MLflow](PINDOLES/mlflow-intro.md), para registrar experimentos si la comparación básica ya funciona.

### Materiales

- [Notebook 02](MATERIALS_PRACTICS/notebooks/02_modelatge_baseline.ipynb), **apartados 3–4**. Ejecutáis antes las celdas de preparación de semana 2 si reiniciáis el entorno.
- Resultados de las referencias de semana 2.

### Tareas

1. Explicáis qué aporta un modelo lineal regularizado respecto de las referencias simples.
2. Entrenáis con los predictores disponibles antes del mes y con transformaciones ajustadas solo en entrenamiento.
3. Comparáis, como mínimo, una referencia y un modelo alternativo con las mismas filas de validación y métricas.
4. Registráis modelo, variables, parámetros, MAE, WAPE y observaciones. Mostráis casos en que la predicción se aleje del resultado.
5. Elegís provisionalmente el modelo con datos de 2025 y justificáis si la mejora compensa la complejidad.

### Entrega y comprobación

Mesa comparativa, elección argumentada y dos casos de error en validación. Todos los miembros tienen que distinguir entrenamiento, validación y prueba.

**Paraos antes del apartado 5.** La selección se revisa en CP3 junto con la segmentación; la prueba final llegará en semana 5.

<a id="semana-4"></a>
## Semana 4 *· Segmentar clientes y proponer acciones

**Objetivo:** describir perfiles de cliente que ayudan a decidir campañas, servicios o prioridades comerciales.

### Píldoras

- [Clustering i segmentació](PINDOLES/clustering-i-segmentacio.md): elegir variables, escalar e interpretar grupos.
- [Visualització](PINDOLES/visualitzacio-amb-matplotlib-i-seaborn.md): comparar perfiles sin quedarse en hashtags numéricos.
- [Scikit-learn](PINDOLES/scikit-learn-intro.md): apoyo si se implementa K-Means u otro algoritmo.

### Materiales

- [Notebook 03 · Segmentació](MATERIALS_PRACTICS/notebooks/03_segmentacio.ipynb).
- [Clients](MATERIALS_PRACTICS/dades/customers.csv) y su definición en el [diccionari](MATERIALS_PRACTICS/dades/DICCIONARI_DADES.md).

### Tareas

1. Revisáis nulos, valores extremos, recència, frecuencia, gasto y devoluciones. Excluis identificador y fecha de fotografía como variables de clustering.
2. Justificáis las variables elegidas. El gasto anual es ticket mediano × pedidos: usar las tres puede dar demasiadas pes al mismo comportamiento.
3. Ejecutáis las reglas del notebook como primera referencia. El notebook incluye una segmentación heurística; no contiene un clustering completo resuelto.
4. Desarrolláis y justificáis la técnica acordada con el profesorado. Para practicar clustering, usáis la píldora de K-Means, ajustáis la escala y argumentáis el número de grupos. Una técnica más compleja no es mejor solo por el nombre.
5. Describís al menos dos perfiles útiles: medida, comportamiento, diferencias y casos fronterizos.
6. Proponéis una acción por perfil y explicáis qué tendría que comprobar la empresa antes de aplicarla.

### Entrega y comprobación *· CP3

- Notebook con preparación, técnica y perfiles interpretados.
- Tabla de segmentos y acciones comerciales.
- Comparativa de modelos de semana 3 y decisión final antes de abrir la maceta.

No hay un hashtag de segmento «correcta». Hay que justificar utilidad y límites. No unís esta fotografía de clientes con ventas históricas como si fueron las mismas transacciones.

<a id="semana-5"></a>
## Semana 5 *· Hacer la prueba final y explicar el resultado

**Objetivo:** comprobar el modelo en datos reservados y comunicar sus errores y límites.

### Píldoras

- [Validació de models](PINDOLES/validacio-de-models.md): lectura crítica de la prueba y diferencia con validación.
- [Explicabilitat amb SHAP i LIME](PINDOLES/xai-shap-lime.md): lectura conceptual de factores y explicaciones. Aplicar las librerías es una ampliación; el mínimo es explicar decisiones y casos concretos con evidencias.

### Materiales

- [Notebook 02](MATERIALS_PRACTICS/notebooks/02_modelatge_baseline.ipynb), **apartados 5–6**, después de recuperar el ajuste y la selección ya fijados.
- [Entrada de juliol](MATERIALS_PRACTICS/dades/sales_forecast_input.csv).
- Comparativa y decisiones congeladas en CP3.

### Tareas

1. Dejáis por escrito el modelo, los predictores y los parámetros antes de ejecutar la prueba.
2. Evaluáis enero–junio de 2026 una vez. No cambiáis el modelo para mejorar esta métrica y continuar presentándola como prueba independiente.
3. Comparáis el resultado con validación y analizáis al menos dos casos de error. Consideráis stock insuficiente, diferencias de grupo e información no observada.
4. Explicáis factores que influyen en el modelo y límites de la interpretación. En datos sintéticos, una relación aprendida no demuestra una causa en el mercado real.
5. Aplicáis el modelo a julio y guardáis las 18 predicciones con la clave completa. Diferenciáis predicción y venta real.
6. Redactáis una recomendación de stock y una de campaña, con el grado de confianza que permiten las evidencias.

### Entrega y comprobación

Métricas finales, análisis de errores, fichero de predicciones y límites documentados. La venta real de julio no está incluida: no hay una métrica validada para ese mes.

Esta entrega prepara CP4. Las métricas no se evalúan para superar un umbral mágico: importa que el proceso sea correcto y que sepáis explicar el resultado.

<a id="semana-6"></a>
## Semana 6 *· Construir el dashboard y defender decisiones

**Objetivo:** presentar una propuesta que la dirección pueda entender y utilizar.

### Píldoras

- [Dashboards i Power BI](PINDOLES/dashboards-i-powerbi.md): elegir KPIs y visuales según la pregunta.
- [Visualització](PINDOLES/visualitzacio-amb-matplotlib-i-seaborn.md): legibilidad y comunicación de resultados, si se necesita refuerzo.

### Materiales

- [Briefing del panell](MATERIALS_PRACTICS/dashboards/01_briefing_powerbi.md).
- [KPIs i visuals](MATERIALS_PRACTICS/dashboards/02_kpis_i_visuals.md).
- [Model de taules](MATERIALS_PRACTICS/dashboards/03_model_taules.md).
- Resultados propios de ventas, validación, segmentos y predicciones.

### Tareas

1. Diseñáis el panel en papel antes de construirlo. Asignáis una pregunta o decisión a cada visual.
2. Incluis una lectura de ventas o ingresos, comparación por canal/producto, perfiles de cliente y conclusiones. Separáis claramente previsión y datos observados.
3. Evitáis duplicar ventas con uniones incorrectas. No sumáis precios ni porcentajes; justificáis las agregaciones.
4. Preparáis la [memòria](#memoria) y el repositorio reproducible. Mostráis de qué resultados sale cada recomendación.
5. Preparáis la defensa de 8–10 minutos: problema, datos, comparación, segmentos, demo del panel, decisiones y límites.
6. Completáis el [autoavaluació](#autoavaluacio) y revisáis la [rúbrica](#avaluacio).

### Entrega final *· CP4

- Notebooks ejecutados con conclusiones e instrucciones para repetirlos.
- Comparativa en validación, decisión de modelo y métricas de prueba reservada.
- Segmentación y al menos dos interpretaciones comerciales útiles.
- Predicciones de julio identificadas como previsión.
- Dashboard o visual equivalente, fichero o enlace accesible.
- Memoria con recomendaciones, limitaciones y reparto de trabajo.
- Defensa y autoevaluación.

**Comprobación mínima de defensa:** una comparativa entre modelos, dos perfiles útiles y una recomendación concreta que la dirección podría considerar. Cada miembro tiene que poder explicar su aportación y el hilo general del proyecto.

<a id="registro"></a>
## Registro de trabajo y revisiones

Mantenéis este registro en la copia de trabajo del equipo o en el repositorio. No hay que crear otra guía del proyecto.

| Semana | Evidencia: fichero o enlace | Que funciona / qué falta | Decisión y motivo | Revisión docente y próximo paso |
|---|---|---|---|---|
| 1 *· CP1 | … | … | … | … |
| 2 *· CP2 | … | … | … | … |
| 3 | … | … | … | … |
| 4 *· CP3 | … | … | … | … |
| 5 | … | … | … | … |
| 6 *· CP4 | … | … | … | … |

En cada checkpoint indicáis **equipo, fecha, objetivo, trabajo acabado, bloqueo, pruebas hechas, decisión técnica, alternativas descartadas, feedback y acción siguiente**. El profesorado puede valorar comprensión, autonomía y calidad técnica con nivel alto, mediano o bajo para orientar el progreso; esto no sustituye la rúbrica final.

<a id="repositorio"></a>
## Organización del trabajo del equipo

```text
equip_bicitierra/
  README.md
  docs/          memòria, decisions i seguiment
  data/          originals conservats i dades derivades diferenciades
  notebooks/     exploració, modelatge i segmentació
  src/           funcions reutilitzables, si en creeu
  tests/         comprovacions de dades o codi, si escau
  evidencies/    mètriques, prediccions, captures i resultats del panell
```

El README del equipo tiene que indicar objetivo, miembros, estructura, dependencias, orden de ejecución y limitaciones. Si trasladáis los notebooks a vuestro repositorio, adaptáis las rutas de datos. En el paquete librado, los notebooks encuentran los CSV en `../dades/` respecto de su carpeta.

<a id="memoria"></a>
## Estructura de la memoria final

1. **Resumen ejecutivo:** problema, propuesta y valor para la empresa.
2. **Datos y flujo de trabajo:** fuentes sintéticas, granularidad, calidad, transformaciones, herramientas y dependencias.
3. **Predicción:** variable objetivo, predictores, fechas de las particiones, modelos, métricas y elección.
4. **Segmentación:** variables, técnica, perfiles, interpretación y acciones.
5. **Pruebas y resultados:** errores, resultados reservados, dashboard y recomendaciones.
6. **Límites y mejoras:** que no se puede concluir, riesgos y siguientes pasos.
7. **Reparto del trabajo:** aportaciones, decisiones compartidas y dificultades resueltas.

<a id="autoavaluacio"></a>
## Autoevaluación del equipo

- Quién ha asumido el análisis, los datos, el modelado, la segmentación, la visualización y la documentación?
- Qué ha sido la mejor decisión y qué evidencia la sustenta?
- Qué problema ha costado más y como se ha resuelto?
- Qué cambiaríais si volvisteis a empezar?
- Qué nivel de autonomía habéis logrado y en que todavía necesitáis apoyo?
- Cada miembro explica una aportación concreta y un aspecto que necesita reforzar.

<a id="avaluacio"></a>
## Evaluación y evidencias

Se aplica la escala existente: **1 muy insuficiente *· 2 insuficiente *· 3 aceptable *· 4 notable *· 5 excelente**. Los pesos orientativos del proyecto continúan siendo **20% por criterio**; el profesorado confirmará la aplicación en la programación del módulo. La rúbrica de defensa de más abajo orienta la observación de la presentación y no añade una segunda ponderación automática.

### 1. EDA y calidad de datos *· 20%

| Nivel | Descripción |
|---|---|
| 1 | No hay EDA útil o se ignoran problemas básicos de calidad. |
| 2 | Exploración superficial, sin decisiones claras sobre nulos, extremos o incoherencias. |
| 3 | EDA correcta y decisiones mínimas justificadas sobre calidad. |
| 4 | El EDA orienta el modelado y detecta problemas relevantes con buen criterio. |
| 5 | Conecta calidad, riesgo e impacto de negocio de manera clara y sólida. |

### 2. Modelado y validación *· 20%

| Nivel | Descripción |
|---|---|
| 1 | No hay modelo usable o no está validado. |
| 2 | Hay modelo, pero sin baseline clara o métrica adecuada. |
| 3 | Hay baseline y comparación básica con otro modelo. |
| 4 | Validación coherente, métricas muy elegidas y lectura de los errores. |
| 5 | Comparación clara, lectura crítica y justificación sólida de la elección final. |

### 3. Segmentación y lectura de perfiles *· 20%

| Nivel | Descripción |
|---|---|
| 1 | No hay segmentación útil o no se puede interpretar. |
| 2 | Hay agrupaciones o hashtags, sin lectura comercial clara. |
| 3 | Segmentación aceptable y perfiles básicos descritos. |
| 4 | Perfiles claros y conectados con decisiones comerciales plausibles. |
| 5 | Perfiles muy descritos que se traducen en prioridades o acciones concretas. |

### 4. Traducción a negocio y visualización *· 20%

| Nivel | Descripción |
|---|---|
| 1 | El trabajo técnico no se traduce en decisiones de negocio. |
| 2 | Visualización o conclusiones genéricas y desconectadas del problema. |
| 3 | Dashboard y conclusiones que permiten una lectura básica del negocio. |
| 4 | Dashboard y recomendaciones que ayudan a tomar decisiones. |
| 5 | Visualización y recomendaciones claras, priorizadas y accionables. |

### 5. Criterio técnico y defensa global *· 20%

| Nivel | Descripción |
|---|---|
| 1 | No se justifican decisiones ni se reconocen límites. |
| 2 | Justificación parcial y poca conciencia de riesgos o límites. |
| 3 | Defensa coherente en el esencial, con límites principales reconocidos. |
| 4 | Explican bien modelos, segmentos, visuales y mejoras posibles. |
| 5 | Defensa madura que conecta datos, métricas, negocio, límites y siguientes pasos. |

### Hoja de valoración

| Criterio | Peso orientativo | Nivel 1–5 | Evidencia y observaciones |
|---|---:|---|---|
| EDA y calidad | 20% | … | … |
| Modelado y validación | 20% | … | … |
| Segmentación y perfiles | 20% | … | … |
| Negocio y visualización | 20% | … | … |
| Criterio técnico y defensa | 20% | … | … |

### Observación de la defensa oral

| Aspecto | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| Comprensión del problema | No lo explican o lo confunden | Explicación parcial con errores | Esencial correcto | Conectan problema, solución y contexto | Explicación clara con visión crítica |
| Justificación técnica | Decisiones arbitrarias | Justificación débil | Decisiones principales coherentes | Justificación sólida vinculada al proyecto | Comparación madura y conciencia de límites |
| Demo funcional | No funciona o no se puede seguir | Parcial y poco clara | Funciona en la esencial | Muestra bien las partes importantes | Muestra con claridad funcionamiento, límites y casos especiales |
| Comunicación | Confusa | Entendedora con vacíos | Clara en general | Ordenada y profesional | Sintética, segura y adaptada al público |
| Límites y mejoras | No los detectan | Los enumeran sin explicar | Explican límites y una mejora | Conectan riesgos y mejoras | Lectura crítica sólida del sistema y su evolución |

Anotáis el nivel y una evidencia por aspecto, una fortaleza técnica, un aspecto a mejorar y una recomendación para la siguiente iteración.

## Checklist antes de la defensa

- [ ] La pregunta de negocio es concreta y las conclusiones  responden.
- [ ] El proceso de datos es reproducible y las transformaciones están justificadas.
- [ ] Ningún predictor anticipado incorpora información posterior del mismo mes.
- [ ] Hay una comparación justa y una prueba temporal reservada.
- [ ] Los perfiles de cliente tienen una interpretación y una acción propuesta.
- [ ] Lo dashboard diferencia real y previsión y usa agregaciones correctas.
- [ ] La memoria, las evidencias y las instrucciones de ejecución están disponibles.
- [ ] Todos los miembros pueden explicar decisiones, aportaciones y límites.
- [ ] La defensa dura 8–10 minutos y muestra una recomendación concreta.

## Índice de consulta de píldoras

| Píldora | Momento principal |
|---|---|
| [Python i entorns](PINDOLES/python-i-entorns.md) | Semana 1, refuerzo inicial |
| [Jupyter](PINDOLES/jupyter-notebooks.md) | Semana 1 |
| [Python per a dades](PINDOLES/python-per-a-dades-i.md) | Semanas 1–2 |
| [EDA i qualitat](PINDOLES/eda-i-qualitat-de-dades.md) | Semana 1 |
| [Visualització](PINDOLES/visualitzacio-amb-matplotlib-i-seaborn.md) | Semanas 2, 4 y 6 |
| [Scikit-learn](PINDOLES/scikit-learn-intro.md) | Semanas 3–4 |
| [Validació](PINDOLES/validacio-de-models.md) | Semanas 2, 3 y 5 |
| [Clustering](PINDOLES/clustering-i-segmentacio.md) | Semana 4 |
| [XAI](PINDOLES/xai-shap-lime.md) | Semana 5; implementación avanzada opcional |
| [Dashboards](PINDOLES/dashboards-i-powerbi.md) | Semana 6 |
| [MLflow](PINDOLES/mlflow-intro.md) | Ampliación opcional desde semana 3 |
