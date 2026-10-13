---
title: "Interpretabilitat: permutación, SHAP y LIME"
description: "Cimientos, ejemplos reproducibles y práctica progresiva: interpretabilitat: permutación, shap y lime."
tags: [IABD, datos, formación]
icon: material/book-open-page-variante
status: ampliada
---

# 09 · Interpretabilitat: permutación, SHAP y LIME

[← Índex de les píndoles](../index.md)

!!! abstract "Qué aprenderás"
    - Distinguir explicaciones globales y locales.
    - Reconstruir una predicción a partir de contribuciones SHAP.
    - Comprobar la fidelidad y los supuestos de una explicación LIME.

**Conocimientos previos:** Modelo supervisado ya validado y distinción entre correlación y causalidad.

**Dedicación orientativa:** 4–5 h para lectura y laboratorio guiado; las extensiones y los ejercicios autónomos requieren tiempo adicional.

!!! tip "Dos recorridos posibles"
    **Esencial:** lee los cimientos, ejecuta el laboratorio y completa la práctica inicial. **Profundización:** continúa con las prácticas intermedia y avance y los ejercicios. El profesorado puede seleccionarlas según los conocimientos previos; no son todas obligatorias dentro del proyecto.

## Mapa de la unidad

Qué queremos explicar? → Empieza con una referencia interpretable → Importancia por permutación → SHAP: referencia y contribuciones → LIME: aproximar un en torno → Explicaciones globales, locales y contradictorias → Como redactar una interpretación defendible → Lectura del laboratorio

## 1. Qué queremos explicar?

Un modelo puede producir una predicción útil sin que sea evidente por qué la ha producido. La interpretabilitat ayuda a examinar su comportamiento, detectar dependencias inesperadas y comunicar resultados. No convierte automáticamente una asociación en una causa ni garantiza que el modelo sea justo.

Antes de calcular una explicación, formula la pregunta. «Qué variables usa más el modelo en general?» es una pregunta global. «Por qué esta observación recibe este resultado?» es local. «Qué pasaría si cambiamos una condición en el mundo real?» es causal y exige supuestos y evidencia adicionales.

| Pregunta | Herramienta posible | Que no permite concluir por sí suela |
|---|---|---|
| Qué información ayuda a predecir? | Importancia por permutación | Que la variable sea la causa del resultado |
| Cómo se reparte una predicción? | SHAP | Que las contribuciones sean únicas fuera de los supuestos elegidos |
| Cómo aproxima el modelo un entorno local? | LIME | Que la aproximación sea fiel lejos del caso |
| Cómo cambia la respuesta del modelo? | Dependencia parcial o ICE | Que una intervención real tenga el mismo efecto |

La primera explicación tendría que ser la del problema: población, respuesta, variables disponibles y uso previsto. Una figura sofisticada no sustituye esta información.

## 2. Empieza con una referencia interpretable

Una regla sencilla, una regresión lineal o un árbol poco profundo pueden servir de referencia. En regresión, un coeficiente describe un cambio en la predicción cuando varía una entrada manteniendo las otras fijas, dentro de la representación utilizada. Si las variables están estandarizadas, las unidades del coeficiente cambian.

La col·linearitat puede hacer inestables los coeficientes. Una variable binaria codificada y una numérica escalada no se tienen que comparar sin explicar las unidades. Un árbol profundo tampoco resulta fácil de leer solo porque sea un árbol.

Compara primero el rendimiento y la complejidad. Si una regla sencilla satisface el objetivo, puede ser preferible a añadir una explicación aproximada sobre un modelo muy complejo. Esta es una decisión que hay que justificar con datos y necesidades de uso.

## 3. Importancia por permutación

El procedimiento calcula una puntuación de referencia, desordena una columna, vuelve a puntuar y medida la pérdida de rendimiento. Repetir la permutación permite observar variabilidad. Hay que hacerlo en un conjunto de validación adecuado; una importancia alta en entrenamiento puede reflejar memorización.

La importancia depende del modelo, de las observaciones y de la métrica. Con `neg_mean_absolute_error`, una pérdida positiva de puntuación corresponde a un aumento de MAE cuando desordenamos la variable. No es un porcentaje de causalidad.

Cuando dos predictores contienen información parecida, el modelo puede compensar la permutación de uno con el otro. La importancia individual puede resultar baja aunque el conjunto sea útil. En series temporales, una permutación de filas también puede generar combinaciones irreales; el diseño tiene que respetar el contexto.

```python title="Una permutació que desfà una relació"
import numpy as np
x = np.array([1, 2, 3, 4, 5])
y = 2x
barrejat = np.random.default_rng(42).permutation(x)
print("MAE original:", np.abs(y-2x).mean())
print("MAE després de permutar:", np.abs(y-2barrejat).mean())
```

Aquí la regla `2x` es conocida. En un problema real, la importancia evalúa el modelo ajustado, no una ley del sistema.

## 4. SHAP: referencia y contribuciones

SHAP distribuye la diferencia entre una predicción y un valor de referencia entre las variables. Para la explicación de regresión del laboratorio, la comprobación es:

**predicción = valor base + suma de contribuciones**.

Una contribución positiva aumenta la predicción respecto de la base; una negativa la reduce. No significa «buena» o «mala» por sí misma. En clasificación, la suma puede expresarse en una escala distinta de probabilidad, según el modelo y la configuración; se tiene que identificar antes de comunicarla.

El conjunto de fondo determina contra qué población comparamos. Usar viviendas pequeñas como referencia puede dar una lectura distinta de usar una muestra representativa de todas las viviendas de entrenamiento. Documenta como se ha elegido esa muestra, su medida y su procedencia.

El laboratorio usa `TreeExplainer`, un bosque de regresión, una muestra de entrenamiento como fondo y `feature_perturbation="interventional"`. Esta configuración tiene supuestos sobre las dependencias entre predictores; la denominación «interventional» no convierte el resultado en una estimación causal del consumo.

Una media de valores SHAP absolutos resume magnitud global, pero pierde dirección y heterogeneidad. Dos personas pueden tener contribuciones opuestas para la misma variable. Revisa casos concretos además del resumen.

## 5. LIME: aproximar un entorno

LIME genera variaciones alrededor de un caso, consulta el modelo y ajusta una aproximación local interpretable. Su lectura depende de la distancia, de la ponderación, del muestreo y de la representación de variables continuas o categóricas.

Un coeficiente grande en el sustituto local no es un coeficiente del modelo original. Es una propiedad de la aproximación en el entorno explorado. Si las perturbaciones generan viviendas imposibles, la explicación puede describir comportamientos que no tienen sentido en el uso real.

Hay que revisar la fidelidad local. En la ejecución de referencia del laboratorio, la puntuación del sustituto es aproximadamente **0,15**: esto es una señal por no confiar en una lectura fuerte de sus pesos. El propósito docente es detectar esta limitación, no ocultarla.

Aumentar el número de muestras puede reducir variabilidad de muestreo, pero no arregla necesariamente un entorno mal definido o una aproximación inadecuada. Compara diversas entonces y configuraciones, registra el resultado y explica si las conclusiones se mantienen.

## 6. Explicaciones globales, locales y contradictorias

Es posible que una variable sea importando globalmente y contribuya poco en un caso concreto. No hay contradicción: las preguntas son diferentes. También es posible que SHAP y LIME destaquen aspectos diferentes, porque usen referencias y aproximaciones distintas.

Una discrepancia es una invitación a investigar: hay interacciones? El caso está lejos de los datos de entrenamiento? Las variables son redundantes? La fidelidad local es baja? El modelo trabaja con variables transformadas y la explicación muestra nombres equivocados?

En pipelines, conserva la correspondencia entre transformaciones y nombres. Si una categoría se despliega en varias columnas, explica si presentas cada columna o una agrupación. No etiquetas una contribución transformada cómo si fuera una magnitud original sin verificarlo.

## 7. Como redactar una interpretación defendible

Una ficha útil contiene el caso, la predicción, la unidad, la base de comparación, las principales contribuciones, el método y una limitación. Por ejemplo: «El modelo estima 109 unidades de consumo; la referencia es 91. La superficie y la ocupación elevan la predicción en este caso. Esta explicación describe el modelo y no demuestra el efecto de una intervención real».

Evita frases como «aumenta la ocupación y ahorrarás» a partir de una correlación o de un peso local. También evita usar la explicación como sustituto de la evaluación: un modelo con buen relato y errores grandes continúa siendo poco útil.

## 8. Lectura del laboratorio

Se generan 300 viviendas sintéticas con superficie, ocupantes y una variable de ruido. El modelo se ajusta solo con entrenamiento; las importancias se calculan en validación. Se explican cinco casos con SHAP y uno con LIME.

La suma SHAP se comprueba numéricamente con tolerancia. El fichero CSV permite revisar las contribuciones y el HTML de LIME muestra la aproximación local. Los datos tienen una relación generadora conocida para facilitar la crítica; esto no implica que todas las explicaciones recuperan perfectamente esa relación.

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
"""Explicacions d'un model de consum sintètic; no inferència causal."""
from pathlib import Path
import numpy as np
import pandas as pd
from sklearn.ensemble import RandomForestRegressor
from sklearn.model_selection import train_test_split
from sklearn.inspection import permutation_importance
from sklearn.metrics import mean_absolute_error
import shap
from lime.lime_tabular import LimeTabularExplainer

def main():
    rng = np.random.default_rng(21)
    X = pd.DataFrame({'superficie': rng.uniform(40, 180, 300), 'ocupants': rng.integers(1, 6, 300), 'soroll': rng.normal(0, 1, 300)})
    y = 20 + 0.45 * X.superficie + 8 * X.ocupants + rng.normal(0, 4, len(X))
    (train, valid, yt, yv) = train_test_split(X, y, test_size=0.25, random_state=42)
    model = RandomForestRegressor(n_estimators=60, min_samples_leaf=4, random_state=42, n_jobs=1).fit(train, yt)
    print('MAE de validació:', mean_absolute_error(yv, model.predict(valid)))
    # Importància sobre validació: no sobre les dades d’ajust.
    perm = permutation_importance(model, valid, yv, scoring='neg_mean_absolute_error', n_repeats=5, random_state=42)
    print('Importància per permutació:', dict(zip(X.columns, perm.importances_mean)))
    # La referència SHAP procedix d’entrenament i queda documentada.
    background = train.sample(60, random_state=42)
    explainer = shap.TreeExplainer(model, data=background, feature_perturbation='interventional')
    explanation = explainer(valid.iloc[:5])
    # Comprovem l’additivitat en l’escala de regressió del model.
    reconstruction = explanation.base_values + explanation.values.sum(axis=1)
    assert np.allclose(reconstruction, model.predict(valid.iloc[:5]), atol=0.0001)
    print('Predicció local:', model.predict(valid.iloc[[0]])[0])
    print('Base:', explanation.base_values[0], 'Contribucions:', explanation.values[0])

    def predict(array):
        return model.predict(pd.DataFrame(array, columns=X.columns))
    local = LimeTabularExplainer(train.to_numpy(), feature_names=list(X.columns), mode='regression', random_state=42)
    # Exigim revisar la fidelitat local abans d’interpretar els pesos.
    result = local.explain_instance(valid.iloc[0].to_numpy(), predict, num_features=3, num_samples=1000)
    print('LIME:', result.as_list(), 'Fidelitat local:', result.score)
    out = Path(__file__).resolve().parent / 'eixides'
    out.mkdir(exist_ok=True)
    pd.DataFrame(explanation.values, columns=X.columns).to_csv(out / 'shap-local.csv', index=False)
    result.save_to_file(str(out / 'lime-local.html'))
if __name__ == '__main__':
    main()
```

## Prácticas guiadas

Cada práctica deja una evidencia pequeña: una salida comprobada y una explicación de la decisión. Los tiempos son orientativos y no incluyen instalación.

### Práctica 1 · Importancia y rendimiento

**Nivel y tiempo:** Inicial · 20–30 min.

**Objetivo:** Interpretar una importancia global a partir de un modelo evaluado.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Ejecuta el laboratorio y registra primero el MAE de validación.
2. Ordena las importancias por permutación e identifica la variable de ruido.
3. Compara la importancia con la relación generadora conocida de los datos sintéticos.
4. Escribe una frase que describa dependencia predictiva sin atribuir causalidad.

!!! success "Comprobación y evidencia esperada"
    Superficie y ocupantes aportan información en este modelo; el ruido tiene una importancia muy menor en la ejecución de referencia.

**Preguntas de reflexión:**

- Por qué hay que conocer el rendimiento antes de confiar en una explicación?
- Qué podría pasar con dos variables redundantes?
- Una importancia negativa demuestra un efecto protector?

**Ampliación opcional:** Aumenta las repeticiones de permutación y muestra también la dispersión de los resultados.

### Práctica 2 · Reconstruir una predicción SHAP

**Nivel y tiempo:** Intermedia · 30–45 min.

**Objetivo:** Leer contribuciones con una base y una unidad explícitas.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Toma el primer caso explicado y anota predicción, valor baso y las tres contribuciones.
2. Súmalas manualmente o con un cálculo separado y compara con la predicción.
3. Redacta una ficha local que distinga contribuciones positivas y negativas.
4. Cambia en una copia la muestra de fondo, manteniendo el modelo, y compara base y atribuciones.

!!! success "Comprobación y evidencia esperada"
    Base más contribuciones reconstruye la predicción con tolerancia; cambiar la referencia puede redistribuir atribuciones.

**Preguntas de reflexión:**

- Qué significa que una contribución sea negativa?
- Por qué hay que documentar el conjunto de fondo?
- Qué parte de la conclusión sería incorrecta si usamos el verbo causar?

**Ampliación opcional:** Compara dos casos con predicciones parecidas y comprueba si las contribuciones también lo son.

### Práctica 3 · Criticar una explicación LIME

**Nivel y tiempo:** Avance · 45–60 min.

**Objetivo:** Evaluar fidelidad y estabilidad antes de comunicar pesos locales.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Abre `lime-local.html` y localiza los pesos de la aproximación.
2. Relaciónalos con la puntuación de fidelidad impresa, próxima a 0,15 en la ejecución comprobada.
3. Repite la explicación con otra semilla y con más muestras, conservando el mismo caso.
4. Registra configuración, puntuación y cambios de los factores destacados; decide si hay bastante evidencia para usar la interpretación.

!!! success "Comprobación y evidencia esperada"
    La conclusión puede ser que la aproximación no es bastante fiel; no se exige que aumente la puntuación para completar la práctica.

**Preguntas de reflexión:**

- Qué diferencia hay entre la predicción original y el sustituto local?
- Por qué más muestras no aseguran una mejor explicación?
- Qué perturbaciones podrían generar observaciones imposibles?

**Ampliación opcional:** Compara discretització activada y desactivada y explica qué pregunta local está respondiendo cada configuración.

## Ejercicios autónomos

Intenta resolver cada reto antes de desplegar la orientación. Se valora el razonamiento y las comprobaciones, no solo obtener una cifra.

### Reto 1 · Leer signos

Una base es 50 y las contribuciones son +12, −4 y +2. Interpreta el resultado.

??? example "Solución orientativa y criterio de revisión"
    La predicción es 60, diez unidades por encima de la base. El signo negativo reduce la predicción respecto de esa referencia; no implica una valoración moral ni un efecto causal.

### Reto 2 · Global y local

Explica como una variable importante globalmente puede aportar casi cero en un caso.

??? example "Solución orientativa y criterio de revisión"
    El resumen global agrega casos; el caso concreto puede situarse cerca de la referencia o en una región donde la variable tenga poco efecto en el modelo. Hay que identificar la pregunta de cada gráfico.

### Reto 3 · Fidelidad baja

Redacta una conclusión si LIME produce pesos claros pero puntuación local baja.

??? example "Solución orientativa y criterio de revisión"
    No mujeres una interpretación fuerte de los pesos. Informa de la aproximación deficiente, revisa en torno y muestreo y contrasta alternativas. Un gráfico legible no es una evidencia de fidelidad.

## Errores habituales y diagnóstico

| Síntoma | Causa que conviene investigar | Comprobación o corrección |
|---|---|---|
| La suma no coincide con la predicción | Escala de salida, modelo o caso diferentes. | Comprueba base, unidades y las mismas entradas. |
| SHAP y LIME no destacan el mismo | Referencias y aproximaciones diferentes. | Examina supuestos y fidelidad local. |
| Pes interpretado cómo causa | Confusión entre modelo y mecanismo real. | Redacta la conclusión como comportamiento predictivo. |

## Autoevaluación y evidencias

Antes de dar la unidad por trabajada, comprueba estos puntos y escribe una frase de evidencia para cada uno:

- [ ] Puedo distinguir explicaciones globales y locales.
- [ ] Puedo reconstruir una predicción a partir de contribuciones SHAP.
- [ ] Puedo comprobar la fidelidad y los supuestos de una explicación LIME.
- [ ] He ejecutado el laboratorio y he contrastado al menos un resultado independientemente.
- [ ] Puedo explicar una limitación y un caso en que el procedimiento requeriría cambios.

**Entregable de la píldora:** copia de trabajo reproducible, resultados de la práctica seleccionada y un texto breve que indico pregunta, decisión, comprobación y limitación. Si se usa dentro de BiciTierra Market, integra esta evidencia en el entregable semanal correspondiente; no hay que crear una entrega duplicada.

## Fuentes para profundizar

- [Documentació oficial de referència](https://shap.readthedocs.io/en/latest/generated/shap.TreeExplainer.html). Consulta especialmente los conceptos y los supuestos descritos en la unidad.
- [Documentació oficial de LIME](https://lime-ml.readthedocs.io/en/latest/lime.html).

Las versiones ejecutadas y los límites de la comprobación están en el [registre de validació](../VALIDACIO.md). Los ejemplos son originales y los datos son sintéticos.

## Aplicación final a BiciTierra Market

**Momento orientativo:** semana 5. Esta correspondencia ayuda a elegir materiales y no sustituye el documento de trabajo del alumnado.

Explica el modelo de ventas que haya superado la validación, sobre casos seleccionados con un criterio explícito. Documenta fondo, escala y correspondencia de variables del pipeline. Diferencia asociaciones predictivas de recomendaciones causales sobre precios o promociones.

**Transferencia:** identifica qué concepto acabas de practicar, qué dato del proyecto lo exige y que tienes que cambiar respecto del laboratorio. Justifica esa adaptación antes de copiar código.
