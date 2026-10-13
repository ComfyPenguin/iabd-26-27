---
title: "MLflow: experimentos trazables y comparables"
description: "Cimientos, ejemplos reproducibles y práctica progresiva: mlflow: experimentos trazables y comparables."
tags: [IABD, datos, formación]
icon: material/book-open-page-variante
status: ampliada
---

# 11 · MLflow: experimentos trazables y comparables

[← Índex de les píndoles](../index.md)

!!! abstract "Qué aprenderás"
    - Registrar decisiones, métricas y artefactos por ejecución.
    - Comparar runs con datos y particiones compatibles.
    - Distinguir registro, persistencia y despliegue.

**Conocimientos previos:** Entrenamiento, validación, en torno Python y ficheros locales.

**Dedicación orientativa:** 3–4 h para lectura y laboratorio guiado; las extensiones y los ejercicios autónomos requieren tiempo adicional.

!!! tip "Dos recorridos posibles"
    **Esencial:** lee los cimientos, ejecuta el laboratorio y completa la práctica inicial. **Profundización:** continúa con las prácticas intermedia y avance y los ejercicios. El profesorado puede seleccionarlas según los conocimientos previos; no son todas obligatorias dentro del proyecto.

## Mapa de la unidad

De «me salió mejor» a una comparación verificable → Qué hay que registrar? → Metadatos y artefactos no son el mismo → Un ciclo mínimo de experimentación → Comparar sin cambiar la pregunta → Guardar y recuperar un modelo → Consultar lo registre → Lectura del laboratorio

## 1. De «me salió mejor» a una comparación verificable

Durante un experimento cambiamos variables, particiones, modelos y parámetros. Si solo conservamos la última celda del notebook, resulta difícil reconstruir qué producía cada resultado. Un registro de experimentos asocia decisiones, métricas y ficheros a una ejecución identificable.

MLflow ofrece herramientas para registrar y consultar este historial. No decide si una partición es correcta, si hay fuga de información o si una métrica responde al problema. Automatiza parte del registro; la calidad del diseño continúa siendo responsabilidad nuestra.

La unidad básica es el **run*, una ejecución con identificador, estado, parámetros, métricas, hashtags y artefactos. Un experimento agrupa runs relacionados. Conviene que tengan una pregunta compartida, como comparar intensidades de regularización bajo una misma partición.

## 2. Qué hay que registrar?

| Elemento | Ejemplo | Por qué es útil |
|---|---|---|
| Parámetro | `alpha=0.1` | Describe una decisión de ajuste |
| Métrica | `mae_valid=0.40` | Cuantifica un resultado y el conjunto donde se ha mesurado |
| Etiqueta | `split=100/50 iid sintetiques` | Añade contexto legible |
| Artefacto | Informe, figura o modelo | Conserva una salida de la ejecución |
| Identificador | ID del run | Permite recuperar exactamente la ejecución |

Distingue métricas de entrenamiento, validación y prueba en los nombres. Una columna llamada solo `error` puede mezclar resultados que no son comparables. Registra también unidades y sentido: MAE más bajo es mejor, mientras que otras puntuaciones se interpretan al contrario.

Una semilla facilita repetir componentes aleatorios, pero no asegura identidad completa entre hardware, versiones o algoritmos. Conserva versiones de dependencias, transformaciones y criterios de partición. Si usas control de versiones, asocia también la revisión del código.

## 3. Metadatos y artefactos no son el mismo

El laboratorio usa una base SQLite local para metadatos y un directorio local para artefactos. La base conserva la información del registro; los ficheros grandes, como un modelo, se guardan al directorio de artefactos. Copiar solo la base puede dejar referencias a ficheros ausentes.

La dirección de seguimiento o **tracking URI* indica donde se registran las ejecuciones. En el ejemplo se construye una ruta absoluta a `codi/eixides/mlflow.db`, de forma que cambiar el directorio desde el cual ejecutamos el programa no creo registros en lugares inesperados.

Esta configuración es para un laboratorio local. Un equipo que comparte experimentos necesita planificar almacenamiento, permisos y acceso. No hay que desplegar un servidor compartido para aprender los conceptos ni para completar esta práctica.

## 4. Un ciclo mínimo de experimentación

El proceso es: preparar datos y partición, ajustar una alternativa, abrir un run, registrar decisiones y resultados, guardar los artefactos y cerrar el run. Un gestor de contexto ayuda a cerrar la ejecución con el estado correspondiendo si hay una excepción.

```python title="Estructura del registre, amb una mètrica de demostració"
from pathlib import Path
import mlflow

base = Path("registre_demo").resolve()
base.mkdir(exist_ok=True)
mlflow.set_tracking_uri("sqlite:///" + str(base/"registre.db"))
mlflow.set_experiment("exemple-de-registre")
with mlflow.start_run(run_name="primera-prova"):
    mlflow.log_param("alpha", 1.0)
    mlflow.log_metric("mae_valid_demo", 2.5)
    mlflow.set_tag("origen", "valor il·lustratiu, no calculat")
```

El valor 2,5 de este fragmento es deliberadamente ilustrativo. El laboratorio completo calcula las métricas a partir de predicciones, que es el que se tiene que hacer en un experimento real. Nunca registros una cifra manual como si fuera una evaluación automática.

## 5. Comparar sin cambiar la pregunta

Dos runs son comparables si evalúan el mismo objetivo, con la misma definición de métrica y un diseño de validación compatible. Si uno usa filas aleatorias y el otro meses futuros, una diferencia de MAE no se puede atribuir solo al modelo.

La versión de datos también importa. El laboratorio calcula una impronta SHA-256 de los arrays utilizados. Esto ayuda a detectar cambios en esa representación concreta, pero no sustituye la descripción de los datos, su procedencia ni una copia recuperable.

En datos reales, registra el fichero o versión de origen y las transformaciones. No necesitas subir datos personales a los artefactos para hacer un buen registro; un resumen, un identificador de versión y controles agregados pueden ser suficientes según el contexto.

La selección se hace con validación. Consultar repetidamente el resultado de prueba para elegir el mejor run convierte ese conjunto en otra validación. El laboratorio compara dos alternativas y no incluye una prueba final: no se tiene que presentar como una estimación definitiva de generalización.

## 6. Guardar y recuperar un modelo

Guardar un modelo permite reutilizar el resultado ajustado. Cuando hay preprocessament, conviene guardar el pipeline completo porque imputación, codificación y modelo viajan juntos. Guardar solo el estimador final puede producir entradas incompatibles después.

El ejemplo registra un modelo scikit-learn con una muestra de entrada. La firma ayuda a describir el esquema esperado; no comprueba que la distribución o el significado de las variables continúan siendo adecuados.

La recuperación utiliza una URI del modelo que devuelve MLflow o que aparece en la información registrada. Después de cargarlo, compara las predicciones de un mismo lote con las del objeto original. La igualdad dentro de una tolerancia es una comprobación útil de persistencia.

No confundisques registro con despliegue. Un modelo guardado todavía necesita un proceso de uso, controles de entrada y seguimiento si se incorpora a un servicio. Esta píldora acaba en experimentación y recuperación local.

## 7. Consultar el registro

Se pueden consultar runs desde Python con `mlflow.search_runs`. Filtra ejecuciones acabadas y muestra las columnas que explican la comparación. Ordenar una métrica solo tiene sentido después de comprobar que los candidatos son comparables.

La interfaz web es una ayuda opcional. Desde el directorio de la píldora, con el entorno activado, se puede iniciar así:

```bash title="Interfície local opcional"
mlflow ui --backend-store-uri sqlite:///codi/eixides/mlflow.db --host 127.0.0.1 --port 5000
```

Abre la dirección local que indico la terminal. La ruta relativa de este mando presupone que estás dentro de la píldora; el programa Python imprime también la URI absoluta para poder comprobar que los dos apuntan en la misma base. Para la interfaz cuando acabas.

## 8. Lectura del laboratorio

El programa genera 150 observaciones sintéticas independientes, reserva 100 para entrenamiento y 50 para validación y compara Ridge con `alpha=0.1` y `alpha=10`. Mantiene datos y partición constantes.

Cada ejecución completa del programa crea dos runs nuevos. Volverlo a ejecutar no sustituye los anteriores: el historial crece. La comprobación final verifica que los dos identificadores acabados de generar aparecen con sido `FINISHED`.

En el entorno de comprobación, los MAE han sido aproximadamente 0,401 y 0,555. Estas cifras son una referencia del laboratorio, no una regla que diga que una regularización baja siempre es mejor. Cambiar datos o partición puede cambiar la comparación.

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
"""Registre local d'experiments amb SQLite, sense servidor extern."""
from pathlib import Path
import hashlib, json, sys
import numpy as np
import mlflow
import mlflow.sklearn
from sklearn.linear_model import Ridge
from sklearn.metrics import mean_absolute_error

def main():
    out = Path(__file__).resolve().parent / 'eixides'
    out.mkdir(exist_ok=True)
    # La base local guarda metadades; els artefactes van a una altra carpeta.
    mlflow.set_tracking_uri('sqlite:///' + str(out / 'mlflow.db'))
    experiment = 'energia-docent'
    if mlflow.get_experiment_by_name(experiment) is None:
        mlflow.create_experiment(experiment, artifact_location=(out / 'artifacts').as_uri())
    mlflow.set_experiment(experiment)
    rng = np.random.default_rng(42)
    X = rng.normal(size=(150, 2))
    y = 10 + 3 * X[:, 0] - 2 * X[:, 1] + rng.normal(0, 0.5, 150)
    (train, valid) = (X[:100], X[100:])
    (yt, yv) = (y[:100], y[100:])
    # Empremta de la representació concreta usada en este experiment.
    fingerprint = hashlib.sha256(X.tobytes() + y.tobytes()).hexdigest()
    run_ids = []
    # Comparem paràmetres mantenint dades i partició constants.
    for alpha in [0.1, 10.0]:
        model = Ridge(alpha=alpha).fit(train, yt)
        with mlflow.start_run(run_name=f'ridge-alpha-{alpha}') as run:
            mlflow.log_params({'alpha': alpha, 'seed': 42, 'train_rows': 100, 'valid_rows': 50})
            mlflow.set_tags({'data_sha256': fingerprint, 'split': '100/50 iid sintetiques', 'python': sys.version.split()[0]})
            mlflow.log_metric('mae_valid', mean_absolute_error(yv, model.predict(valid)))
            mlflow.log_text('Dades sintètiques; comparació en validació, sense test final en esta demo.', 'decisions.txt')
            mlflow.sklearn.log_model(model, name='model', input_example=train[:2])
            run_ids.append(run.info.run_id)
    # Comprovem que les execucions han acabat i es poden recuperar.
    runs = mlflow.search_runs(experiment_names=[experiment], filter_string="attributes.status = 'FINISHED'")
    assert set(run_ids).issubset(set(runs.run_id))
    print(runs[['run_id', 'params.alpha', 'metrics.mae_valid']].head().to_string(index=False))
    (out / 'execucio.json').write_text(json.dumps({'run_ids': run_ids, 'data_sha256': fingerprint}, indent=2))
    print('Tracking URI:', mlflow.get_tracking_uri())
if __name__ == '__main__':
    main()
```

## Prácticas guiadas

Cada práctica deja una evidencia pequeña: una salida comprobada y una explicación de la decisión. Los tiempos son orientativos y no incluyen instalación.

### Práctica 1 · Localizar dos experimentos comparables

**Nivel y tiempo:** Inicial · 20–30 min.

**Objetivo:** Relacionar run, parámetro, métrica y artefacto.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Ejecuta el programa y guarda los dos identificadores imprimidos o registrados en `execucio.json`.
2. Localiza la base SQLite y el directorio de artefactos generados.
3. Consulta los runs acabados y compara los valores de alpha y MAE de validación.
4. Explica qué elementos se mantienen constantes entre las dos alternativas.

!!! success "Comprobación y evidencia esperada"
    Los dos runs figuran como FINISHED, comparten impronta de datos y partición, y tienen alpha diferente.

**Preguntas de reflexión:**

- Qué diferencia hay entre experimento y run?
- Por qué la métrica se llama `mae_valid`?
- Qué perderíamos si solo conservamos un pantallazo?

**Ampliación opcional:** Abre la interfaz local opcional apuntando en la misma base y comprueba los identificadores.

### Práctica 2 · Hacer una comparación trazable

**Nivel y tiempo:** Intermedia · 30–45 min.

**Objetivo:** Añadir una alternativa sin cambiar silenciosamente datos o criterios.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Añade alpha 1 a la lista de candidatos en una copia del programa.
2. Registra un hashtag con una justificación breve de la comparación.
3. Ejecuta y filtra los tres identificadores de esta ejecución, evitando mezclarlos con anteriores.
4. Ordena por MAE de validación y escribe una decisión provisional con límites.

!!! success "Comprobación y evidencia esperada"
    La comparación incluye tres runs de la misma ejecución; no se presenta el resultado como prueba final.

**Preguntas de reflexión:**

- Por qué repetir el programa crea nuevos runs?
- Cómo distinguirías una mejora de modelo de un cambio en los datos?
- Qué problema hay a elegir según el mejor resultado de prueba de muchos runs?

**Ampliación opcional:** Añade duración de entrenamiento y valora si una mejora pequeña compensa el coste adicional.

### Práctica 3 · Recuperar y comprobar

**Nivel y tiempo:** Avance · 45–60 min.

**Objetivo:** Verificar que un modelo guardado conserva el comportamiento esperado.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Modifica la copia porque conservo el objeto devuelto por `mlflow.sklearn.log_model`.
2. Usa la suya `model_uri` con `mlflow.sklearn.load_model`.
3. Compara las predicciones del modelo original y del recuperado sobre el mismo lote con `numpy.allclose`.
4. Documenta qué hay que conservar para mover el experimento a otro ordenador: datos o versión, código, entorno, metadatos y artefactos.

!!! success "Comprobación y evidencia esperada"
    La comprobación de predicciones pasa; el informe no confunde el fichero del modelo con un servicio desplegado.

**Preguntas de reflexión:**

- Por qué hay que guardar el preprocessament cuando existe?
- Qué garantía aporta una firma y qué no?
- Por qué copiar solo SQLite puede ser insuficiente?

**Ampliación opcional:** Registra un pipeline con estandarización y Ridge y repite la comprobación de recuperación.

## Ejercicios autónomos

Intenta resolver cada reto antes de desplegar la orientación. Se valora el razonamiento y las comprobaciones, no solo obtener una cifra.

### Reto 1 · Métricas incomparables

Dos runs tienen MAE 4 y 3 pero usan particiones diferentes. Se puede declarar ganador?

??? example "Solución orientativa y criterio de revisión"
    No atribuyas la diferencia solo al modelo. Repite con un diseño comparable o describe explícitamente que responden preguntas diferentes.

### Reto 2 · Impronta de datos

Explica qué permite y que no permite recuperar un hash de los datos.

??? example "Solución orientativa y criterio de revisión"
    Permite contrastar identidad de la representación usada con otra copia. No reconstruye los datos ni explica su procedencia, calidad o permisos de uso.

### Reto 3 · Persistencia completa

Fez una lista mínima para recuperar un pipeline y verificar la predicción meses después.

??? example "Solución orientativa y criterio de revisión"
    Modelo y preprocessament, esquema de entrada, versiones, código, datos o versión recuperable, partición y un lote de comprobación. El ID del run ayuda a encontrarlo pero no sustituye los artefactos.

## Errores habituales y diagnóstico

| Síntoma | Causa que conviene investigar | Comprobación o corrección |
|---|---|---|
| Interfaz sin runs | Apunta en otra base de seguimiento. | Compara la URI absoluta del programa con la de la interfaz. |
| Modelo registrado pero no recuperable | Artefactos ausentes o rutas cambiadas. | Conserva artefactos junto con metadatos y comprueba la URI devuelta. |
| Ganador aparente entre runs diferentes | Cambios de datos, partición o métrica. | Filtra candidatos comparables y registra el contexto. |

## Autoevaluación y evidencias

Antes de dar la unidad por trabajada, comprueba estos puntos y escribe una frase de evidencia para cada uno:

- [ ] Puedo registrar decisiones, métricas y artefactos por ejecución.
- [ ] Puedo comparar runs con datos y particiones compatibles.
- [ ] Puedo distinguir registro, persistencia y despliegue.
- [ ] He ejecutado el laboratorio y he contrastado al menos un resultado independientemente.
- [ ] Puedo explicar una limitación y un caso en que el procedimiento requeriría cambios.

**Entregable de la píldora:** copia de trabajo reproducible, resultados de la práctica seleccionada y un texto breve que indico pregunta, decisión, comprobación y limitación. Si se usa dentro de BiciTierra Market, integra esta evidencia en el entregable semanal correspondiente; no hay que crear una entrega duplicada.

## Fuentes para profundizar

- [Documentació oficial de referència](https://mlflow.org/docs/latest/ml/tracking/). Consulta especialmente los conceptos y los supuestos descritos en la unidad.
- [Inici amb MLflow](https://mlflow.org/docs/latest/ml/getting-started/quickstart/).

Las versiones ejecutadas y los límites de la comprobación están en el [registre de validació](../VALIDACIO.md). Los ejemplos son originales y los datos son sintéticos.

## Aplicación final a BiciTierra Market

**Momento orientativo:** semana 3–6, ampliación. Esta correspondencia ayuda a elegir materiales y no sustituye el documento de trabajo del alumnado.

Es una ampliación opcional para registrar los modelos de ventas. Mantén constantes los cortes temporales cuando compadres runs y hashtag la versión de datos. Si el equipo todavía está aprendiendo pipelines y validación, una tabla de experimentos muy documentada puede ser el primer paso antes de automatizarla.

**Transferencia:** identifica qué concepto acabas de practicar, qué dato del proyecto lo exige y que tienes que cambiar respecto del laboratorio. Justifica esa adaptación antes de copiar código.
