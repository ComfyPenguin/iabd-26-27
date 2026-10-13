---
title: "Validación: mesurar aquello que realmente queremos predecir"
description: "Cimientos, ejemplos reproducibles y práctica progresiva: validación: mesurar aquello que realmente queremos predecir."
tags: [IABD, datos, formación]
icon: material/book-open-page-variante
status: ampliada
---

# 07 · Validación: mesurar aquello que realmente queremos predecir

[← Índex de les píndoles](../index.md)

!!! abstract "Qué aprenderás"
    - Seleccionar particiones según tiempos, grupos o independencia.
    - Evitar información futura en retrasos y transformaciones.
    - Interpretar métricas globales y por grupo.

**Conocimientos previos:** Pipeline supervisado y cálculo de MAE.

**Dedicación orientativa:** 4–5 h para lectura y laboratorio guiado; las extensiones y los ejercicios autónomos requieren tiempo adicional.

!!! tip "Dos recorridos posibles"
    **Esencial:** lee los cimientos, ejecuta el laboratorio y completa la práctica inicial. **Profundización:** continúa con las prácticas intermedia y avance y los ejercicios. El profesorado puede seleccionarlas según los conocimientos previos; no son todas obligatorias dentro del proyecto.

## Mapa de la unidad

Validar es simular una situación futura → Tres familias de separación → Fuga de información → Retrasos y ventanas → Backtesting y paneles → Errores en unidades y en proporción → Sobreajust, selección e incertidumbre → Lectura del laboratorio

## 1. Validar es simular una situación futura

La validación pregunta como se comportará el procedimiento ante datos que no ha utilizado para aprender ni para elegir decisiones. La dificultad principal es definir qué significa «nuevo»: otra fila, otra persona, otro centro o un momento posterior.

Si entrenamos con lecturas de una máquina y probamos con lecturas casi idénticas de la misma máquina, estamos mesurando una capacidad distinta de la que necesitaríamos en una máquina desconocida. La partición forma parte de la definición del problema.

Entrenamiento ajusta; validación selecciona; prueba estima el resultado final. En conjuntos pequeños, la validación cruzada aprovecha varias particiones, pero no autoriza a ignorar tiempos, grupos o disponibilidad de variables.

## 2. Tres familias de separación

| Diseño | Cuando tiene sentido | Riesgo si se aplica fuera de contexto |
|---|---|---|
| Aleatorio | Observaciones aproximadamente independientes y misma población de uso | Mezclar información de la misma entidad o del futuro |
| Por grupos | Pacientes, personas, centros o dispositivos que no tienen que compartirse | Mesurar memorización de entidades conocidas |
| Temporal | Predecir periodos posteriores con información anterior | Ajustar con futuro y probar con pasado |

Una estratificación mantiene proporciones de clases, pero no elimina dependencia entre observaciones. Si una persona aparece muchas veces, separar por clase no impide que la misma persona esté en entrenamiento y prueba.

La validación cruzada anidada separa selección de parámetros y estimación de rendimiento, pero también tiene que adaptar el diseño interno y externo a la dependencia de los datos. No es un remedio automático para un dataset mal definido.

## 3. Fuga de información

La fuga aparece cuando el modelo o el proceso de selección accede a información que no estaría disponible en el uso previsto. Puede ser directa, como un importe calculado a partir de la respuesta, o sutil, como escalar con el conjunto de prueba.

Ejemplos: predecir cancelación con la fecha de cancelación; predecir demora con el tiempo final de resolución; seleccionar variables con correlaciones calculadas sobre la prueba; usar una media móvil que incluye el mismo valor que se quiere anticipar.

Por cada predictor, anota el momento en que se conoce. Una columna presente en un extracto actual no era necesariamente conocida a comienzos de cada periodo histórico. El contrato temporal tiene que ser explícito.

## 4. Retrasos y ventanas

Un retraso de una fila solo equivale a un retraso de un periodo si la serie está ordenada y no faltan periodos. En un panel con muchas entidades, el desplazamiento se tiene que calcular dentro de cada grupo.

```python title="Un retard que no travessa edificis"
import pandas as pd

df = pd.DataFrame({"edifici":["A","A","B","B"],
                   "mes":[1,2,1,2],"consum":[100,120,80,90]})
df = df.sort_values(["edifici","mes"])
df["lag1"] = df.groupby("edifici")["consum"].shift(1)
print(df)
```

En predicción de un mes vista, el valor real del mes anterior se puede incorporar cuando ya ha sido observado. En una predicción de seis meses hecha hoy, los valores reales de los cinco meses intermedios todavía no existen: hace falta un esquema recursiu, directo u otro diseño justificado.

Por eso un buen resultado en evaluación mensual sucesiva no se puede presentar como una validación de seis meses al avance.

## 5. Backtesting y paneles

El **backtesting* repite el ajuste con varios puntos de corte históricos. Una ventana expansiva incorpora todo el pasado disponible; una ventana deslizante conserva solo un tramo reciente. La primera aprovecha más observaciones; la segunda puede adaptarse a cambios, a expensas de menos datos.

En un panel mensual, todas las filas de un mes tienen que ir al mismo bloque si la pregunta es anticipar un mes completo. Aplicar directamente un separador sobre índices de filas puede dividir un mismo mes entre entrenamiento y validación. La documentación de [TimeSeriesSplit](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.TimeSeriesSplit.html) describe el separador; nuestro laboratorio construye los cortes por fechas para respetar el panel.

Un intervalo de separación puede ser necesario si los hashtags llegan con retraso o las ventanas de características se solapan. Su longitud tiene que responder al proceso, no a una regla decorativa.

## 6. Errores en unidades y en proporción

MAE es la media de `|real − predicció|`. WAPE divide el error absoluto total por el total absoluto observado. MAE mantiene unidades; WAPE permite una lectura relativa del volumen conjunto, pero da más pes a los grupos grandes.

```python title="Mètriques amb casos concrets"
import numpy as np

real = np.array([10., 20., 0.])
pred = np.array([12., 18., 1.])
mae = np.abs(real-pred).mean()
wape = 100np.abs(real-pred).sum()/np.abs(real).sum()
print(mae, wape)  # 1.666... i 16.666...%
```

Si el denominador global es cero, WAPE no está definido. MAPE tiene problemas cuando hay valores reales cero o muy pequeños. No escondes estos casos añadiendo una constante arbitraria sin explicar como cambia la métrica.

Además de la media global, revisa grupos, cuantiles de error y casos de fallo. Un modelo aceptable en conjunto puede ser poco útil para un producto minoritario.

## 7. Sobreajust, selección e incertidumbre

Un resultado excelente en entrenamiento y peor en validación puede indicar sobreajust, pero también cambio de distribución o errores en el preprocessament. Diagnostica antes de ajustar parámetros.

Probar muchas alternativas y quedarse con la mejor métrica de validación introduce optimismo en la selección. Reserva una prueba final y documenta cuántas decisiones se han comparado. La variabilidad entre folds aporta información, pero no es automáticamente un intervalo de confianza de cualquier uso futuro.

En series dependientes, un bootstrap de filas independientes puede romper la estructura temporal. Si se quiere estimar incertidumbre con remostreig, se tiene que justificar la unidad de muestreo y la dependencia conservada.

## 8. Lectura del laboratorio

El ejemplo contiene 48 meses de dos edificios. El primer año prepara un retraso anual; dos cortes de 2025 sirven para observar estabilidad y 2026 queda reservado. El modelo usa retrasos, una tendencia y el edificio, con parámetros fijados antes de la prueba.

Las comprobaciones aseguran que ninguna fecha de entrenamiento aparece en validación. Esto no demuestra por si suele ausencia de todas las fugas: también hay que revisar como se han generado las variables. El propósito es convertir el diseño experimental en una parte visible del código.

## Laboratorio completo y reproducible

**Contexto:** trabajaremos con datos sintéticos creados por el mismo programa. No hay que descargar datasets ni usar los CSV de BiciTierra. Los valores sirven para aprender y comprobar procedimientos; no describen una población real.

**Materiales:** Python 3.10 o superior, un entorno virtual y los [requisits de la unitat](requirements.txt). Consulta la [preparació comuna](../index.md) antes de instalarlos.

Desde la carpeta de esta píldora, con el entorno activado:

```bash
python -m pip install -r requirements.txt
python codi/exemple.py
```

El programa crea `codi/eixides/`. Puedes examinar las salidas sin modificar el código original. Para resolver las variantes, trabaja sobre una copia y actualiza las comprobaciones cuando cambias deliberadamente los datos.

### Ejemplo comentado

[Obri o descarrega el programa complet](codi/exemple.py){ download="exemple.py" }. Cada comprobación `assert` expresa una propiedad esperada de los datos de demostración; si falla, investiga la causa antes de eliminarla.

```python title="codi/exemple.py" linenums="1"
"""Backtesting per dates completes d'un panell de dos edificis."""
from pathlib import Path
import numpy as np
import pandas as pd
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_absolute_error

def main():
    rng = np.random.default_rng(7)
    dates = pd.date_range('2023-01-01', periods=48, freq='MS')
    rows = []
    for (building, offset) in [('A', 0), ('B', 30)]:
        for (i, date) in enumerate(dates):
            rows.append({'data': date, 'edifici': building, 't': i, 'consum': 100 + offset + i * 0.8 + 12 * np.sin(2 * np.pi * i / 12) + rng.normal(0, 3)})
    df = pd.DataFrame(rows).sort_values(['edifici', 'data'])
    # Retards dins de cada edifici; les sèries són mensuals completes.
    df['lag1'] = df.groupby('edifici').consum.shift(1)
    df['lag12'] = df.groupby('edifici').consum.shift(12)
    df = df.dropna().copy()
    df['b'] = (df.edifici == 'B').astype(int)
    # lag1 pressuposa predicció successiva d’un mes, no sis mesos de colp.
    features = ['lag1', 'lag12', 't', 'b']
    # Els talls són per mesos complets, no per posició de fila.
    folds = [('2024-12-01', '2025-01-01', '2025-06-01'), ('2025-06-01', '2025-07-01', '2025-12-01')]
    results = []
    for (end, start, stop) in folds:
        tr = df[df.data <= end]
        va = df[df.data.between(start, stop)]
        assert tr.data.max() < va.data.min() and set(tr.data).isdisjoint(set(va.data))
        model = Ridge(alpha=1).fit(tr[features], tr.consum)
        results.append({'train_end': end, 'valid_start': start, 'files': len(va), 'mae_model': mean_absolute_error(va.consum, model.predict(va[features])), 'mae_lag12': mean_absolute_error(va.consum, va.lag12)})
    print(pd.DataFrame(results).round(3).to_string(index=False))
    # 2026 queda reservat: alpha s’ha fixat abans d’esta avaluació.
    train = df[df.data < '2026-01-01']
    test = df[df.data >= '2026-01-01']
    model = Ridge(alpha=1).fit(train[features], train.consum)
    pred = model.predict(test[features])
    mae = mean_absolute_error(test.consum, pred)
    wape = 100 * np.abs(test.consum - pred).sum() / np.abs(test.consum).sum()
    print(f'Test: MAE={mae:.3f}; WAPE={wape:.3f}%')
    out = Path(__file__).resolve().parent / 'eixides'
    out.mkdir(exist_ok=True)
    pd.DataFrame(results).to_csv(out / 'backtesting.csv', index=False)
    assert len(test) == 24 and len(results) == 2
if __name__ == '__main__':
    main()
```

## Prácticas guiadas

Cada práctica deja una evidencia pequeña: una salida comprobada y una explicación de la decisión. Los tiempos son orientativos y no incluyen instalación.

### Práctica 1 · Dibujar las particiones

**Nivel y tiempo:** Inicial · 20–30 min.

**Objetivo:** Verificar que una pregunta temporal se traduce en cortes por fechas.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Ejecuta el laboratorio y abre `backtesting.csv`.
2. Representa en una línea temporal entrenamiento y validación de los dos folds, más la prueba de 2026.
3. Comprueba que cada mes aporta dos filas y que no se divide entre bloques.
4. Anota cuántas filas quedan después de crear el retraso anual y por qué se pierde el primer año.

!!! success "Comprobación y evidencia esperada"
    Quedan 72 filas con retrasos; cada validación semestral tiene 12 filas y la prueba anual  tiene 24.

**Preguntas de reflexión:**

- Por qué 24 filas de prueba no equivalen a 24 meses?
- Qué cambiaría si faltara un mes en un edificio?
- Por qué no se tienen que mezclar aleatoriamente estas filas?

**Ampliación opcional:** Añade una comprobación de periodicidad por edificio antes de calcular `shift`.

### Práctica 2 · Comparar una referencia temporal

**Nivel y tiempo:** Intermedia · 30–45 min.

**Objetivo:** Valorar un modelo contra una regla disponible en el momento de predecir.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Compara el MAE de Ridge y del retraso anual en cada fold.
2. Calcula el error absoluto por fila y resúmelo por edificio.
3. Identifica si una mejora global esconde un grupo con peor resultado.
4. Redacta un criterio de selección que combino media, estabilidad y uso previsto, sin consultar nuevas alternativas sobre 2026.

!!! success "Comprobación y evidencia esperada"
    La comparación incluye la regla estacional y muestra resultados por corte y por grupo.

**Preguntas de reflexión:**

- Por qué un modelo sofisticado puede perder ante una regla estacional?
- Cómo influye la tendencia en una predicción basada en el año anterior?
- Qué métrica complementaría MAE si los volúmenes fueran muy diferentes?

**Ampliación opcional:** Añade como tercera referencia el mes anterior, manteniendo exactamente las mismas filas de evaluación.

### Práctica 3 · Cambiar el horizonte cambia el experimento

**Nivel y tiempo:** Avance · 45–60 min.

**Objetivo:** Distinguir predicciones sucesivas de un mes y una predicción conjunta de seis meses.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Elige el corte de diciembre de 2024 y enumera la información disponible en ese instante.
2. Marca qué valores de `lag1` de febrero a junio de 2025 todavía no se conocen en ese corte.
3. Explica por qué la evaluación del laboratorio es de un mes vista con observaciones sucesivas, aunque uso un bloque semestral.
4. Diseña en pseudocodi una alternativa recursiva o directa para seis meses y declara como se obtendrían los predictores.

!!! success "Comprobación y evidencia esperada"
    La propuesta no reutiliza valores reales intermedios como si fueron conocidos en diciembre.

**Preguntas de reflexión:**

- Qué fuga aparece si entregamos los seis meses como una previsión hecha al mismo tiempo?
- Cómo se pueden acumular errores en un esquema recursiu?
- Qué variables futuras podrían conocerse por calendario?

**Ampliación opcional:** Implementa dos pasos recursius y compáralos con dos pasos sucesivos; hashtag los resultados como experimentos diferentes.

## Ejercicios autónomos

Intenta resolver cada reto antes de desplegar la orientación. Se valora el razonamiento y las comprobaciones, no solo obtener una cifra.

### Reto 1 · Qué separador?

Elige diseño para lecturas futuras de una máquina, nuevos pacientes y viviendas independientes.

??? example "Solución orientativa y criterio de revisión"
    Temporal para el futuro de la máquina; por grupo de paciente cuando hay visitas repetidas; aleatorio si las viviendas son realmente independientes y representan la población de uso. Justifica posibles dependencias adicionales.

### Reto 2 · Errores con ceros

Calcula MAE y WAPE para reales [0, 0] y predicciones [1, 2].

??? example "Solución orientativa y criterio de revisión"
    MAE es 1,5. WAPE no está definido porque el denominador es cero. Informa del caso y no sustituyas el resultado por cero ni por un porcentaje arbitrario.

### Reto 3 · Calendario incompleto

Explica qué pasa con shift(12) si falta un mes dentro de una serie.

??? example "Solución orientativa y criterio de revisión"
    Desplaza doce filas, no necesariamente doce meses. Reindexa con un calendario completo por entidad, conserva ausencias y decide como gestionarlas antes de crear retrasos.

## Errores habituales y diagnóstico

| Síntoma | Causa que conviene investigar | Comprobación o corrección |
|---|---|---|
| Resultado demasiado bueno en futuro | Retrasos o agregados con datos todavía desconocidos. | Anota disponibilidad de cada predictor al momento de corte. |
| Un mes aparece en dos bloques | Separación por filas en un panel. | Construye los bloques por fechas completas. |
| Porcentaje infinito o ausente | Denominador cero en métrica relativa. | Informa del caso y complementa con errores absolutos. |

## Autoevaluación y evidencias

Antes de dar la unidad por trabajada, comprueba estos puntos y escribe una frase de evidencia para cada uno:

- [ ] Puedo seleccionar particiones según tiempos, grupos o independencia.
- [ ] Puedo evitar información futura en retrasos y transformaciones.
- [ ] Puedo interpretar métricas globales y por grupo.
- [ ] He ejecutado el laboratorio y he contrastado al menos un resultado independientemente.
- [ ] Puedo explicar una limitación y un caso en que el procedimiento requeriría cambios.

**Entregable de la píldora:** copia de trabajo reproducible, resultados de la práctica seleccionada y un texto breve que indico pregunta, decisión, comprobación y limitación. Si se usa dentro de BiciTierra Market, integra esta evidencia en el entregable semanal correspondiente; no hay que crear una entrega duplicada.

## Fuentes para profundizar

- [Documentació oficial de referència](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.TimeSeriesSplit.html). Consulta especialmente los conceptos y los supuestos descritos en la unidad.

Las versiones ejecutadas y los límites de la comprobación están en el [registre de validació](../VALIDACIO.md). Los ejemplos son originales y los datos son sintéticos.

## Aplicación final a BiciTierra Market

**Momento orientativo:** semana 2, 3 y 5. Esta correspondencia ayuda a elegir materiales y no sustituye el documento de trabajo del alumnado.

Separa meses completos de salas_history.csv y reserva un tramo final antes de seleccionar modelos. Declara el horizonte y comprueba disponibilidad de retrasos por combinación. salas_forecast_input.csv es entrada para una previsión futura, no una prueba con resultados reales conocidos.

**Transferencia:** identifica qué concepto acabas de practicar, qué dato del proyecto lo exige y que tienes que cambiar respecto del laboratorio. Justifica esa adaptación antes de copiar código.
