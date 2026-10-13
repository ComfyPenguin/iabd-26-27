---
title: "Clustering e interpretación de perfiles"
description: "Cimientos, ejemplos reproducibles y práctica progresiva: clustering e interpretación de perfiles."
tags: [IABD, datos, formación]
icon: material/book-open-page-variant
status: ampliada
---

# 08 · Clustering e interpretación de perfiles

[← Índex de les píndoles](../index.md)

!!! abstract "Qué aprenderás"
    - Construir una matriz de características con una fila por entidad.
    - Comparar agrupaciones con métricas y perfiles.
    - Examinar estabilidad y limitaciones de los hashtags.

**Conocimientos previos:** pandas, escalas numéricas y gráficos de dispersión.

**Dedicación orientativa:** 3–4 h para lectura y laboratorio guiado; las extensiones y los ejercicios autónomos requieren tiempo adicional.

!!! tip "Dos recorridos posibles"
    **Esencial:** lee los cimientos, ejecuta el laboratorio y completa la práctica inicial. **Profundización:** continúa con las prácticas intermedia y avance y los ejercicios. El profesorado puede seleccionarlas según los conocimientos previos; no son todas obligatorias dentro del proyecto.

## Mapa de la unidad

Agrupar no es descubrir hashtags inevitables → Elegir variables y escalas → K-Means paso a paso → Escoger el número de grupos → DBSCAN y densidad → PCA y representación → Estabilidad y asignación de casos nuevos → Perfiles que ayudan a decidir

## 1. Agrupar no es descubrir hashtags inevitables

El clustering agrupa observaciones según una noción de parecido. A diferencia de la clasificación supervisada, normalmente no partimos de un hashtag de respuesta que el modelo tenga que imitar. Las agrupaciones dependen de las variables, la escala, la distancia y el algoritmo.

En una biblioteca podemos agrupar personas por frecuencia de visitas, duración y recència. Una segmentación por intereses culturales podría dar grupos diferentes. Ninguno de las dos es «la partición real» de las personas; cada una responde una pregunta.

Segmentar incluye, además de agrupar, interpretar y decidir como se utilizarán los perfiles. Un algoritmo puede separar puntos geométricamente y producir grupos sin utilidad operativa.

## 2. Elegir variables y escalas

Un identificador puede ser numérico, pero la diferencia entre dos identificadores no representa una distancia de comportamiento. Exclúyelo de la geometría. Una fecha común tampoco aporta separación útil.

Si se combinan euros, días y números de visitas sin escala, una magnitud puede dominar la distancia por su rango. La estandarización transforma cada variable con su media y desviación. Es una opción, no una garantía: ante extremos fuertes puede convenir una escala robusta o una transformación justificada.

Variables redundantes pueden duplicar una misma información. Si gastada = frecuencia × ticket, incorporar las tres sin reflexión modifica el peso implícito del valor monetario. Elige según la pregunta y compara como cambia el resultado.

## 3. K-Means paso a paso

K-Means busca centros que reduzcan la suma de distancias cuadráticas de cada punto al centro asignado. De manera simplificada, inicializa centros, asigna puntos al más próximo, recalcula centros y repite hasta converger.

Funciona especialmente bien con grupos compactos en una geometría euclídea. Puede dividir un grupo alargado o combinar grupos de densidad desigual. También es sensible a escala y a valores extremos.

```python title="Una agrupació mínima"
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler

X = [[1, 10], [2, 12], [1, 8], [8, 70], [9, 75], [7, 68]]
z = StandardScaler().fit_transform(X)
model = KMeans(n_clusters=2, n_init=10, random_state=42).fit(z)
print(model.labels_)
```

Los números de grupo son identificadores arbitrarios. El grupo 0 de una ejecución no es necesariamente el grupo 0 de otra. Para comparar particiones, usa una medida que no dependa de los nombres, como el índice de Rand ajustado.

## 4. Escoger el número de grupos

La inercia disminuye cuando aumenta `k`; el método del codo busca una reducción con rendimientos decrecientes, pero no siempre hay un codo claro. La silueta compara proximidad al grupo propio con separación del grupo alternativo más próximo.

Una silueta alta puede ayudar a identificar separación, pero no demuestra utilidad comercial ni corrección social de una segmentación. Además, algunas geometrías que resultan útiles no maximizan esta métrica.

Compara un rango razonable, revisa tamaños, estabilidad e interpretación. Un grupo de dos casos puede representar una anomalía interesante o un artefacto. Hay que ver los registros, no solo el índice global.

## 5. DBSCAN y densidad

DBSCAN conecta regiones de densidad a partir de un radio `eps` y de un mínimo de puntos. Puede identificar formas no esféricas y marcar observaciones como ruido, habitualmente con hashtag `-1`.

No requiere fijar el número de grupos, pero sí parámetros que dependen de la escala y la densidad. Con un radio demasiado pequeño, muchos puntos quedan como ruido; con un demasiado grande, los grupos se fusionan. En densidades muy diferentes, un único radio puede ser inadecuado.

El ruido no es necesariamente un cliente defectuoso ni una observación eliminable. Significa que el algoritmo no lo ha incluido en una región densa con esos parámetros. Describe la decisión sin convertirla en un juicio sobre la persona.

## 6. PCA y representación

El análisis de componentes principales, PCA, transforma variables en direcciones que explican varianza. Puede ayudar a visualizar una matriz de muchas dimensiones en dos ejes. No es un algoritmo de clustering y no preserva automáticamente todas las distancias relevantes.

Una vista en dos componentes puede esconder solapamiento o separación presente en otras dimensiones. Reporta la varianza explicada e interpreta los perfiles con las variables originales, no solo con la posición en un dibujo.

Si se usa PCA antes de agrupar, se ha cambiado el espacio sobre el cual se define parecido. Justifica por qué se hace y compara con una alternativa más simple.

## 7. Estabilidad y asignación de casos nuevos

Cambiar la semilla comprueba sensibilidad a la inicialización; cambiar la muestra comprueba una cuestión distinta. Una partición estable entre dos entonces puede continuar siendo frágil si se excluye un grupo de datos o cambia la escala.

K-Means permite asignar puntos nuevos en los centros ya ajustados mediante `predict`. Esto no rehace los centros. DBSCAN, en la implementación habitual de scikit-learn, no ofrece un `predict` equivalente: no se tiene que prometer una asignación automática futura sin definir un procedimiento.

Conserva el preprocessament ajustado. Volver a escalar cada lote nuevo con su propia media cambia la geometría y deja de ser la misma segmentación.

## 8. Perfiles que ayudan a decidir

Por cada segmento, describe número y proporción de observaciones, mediana o media de variables útiles, dispersión y casos que no encajan en una lectura simple. Posa un nombre descriptivo, como «visita frecuente y corta», en lugar de inferir motivaciones personales no observadas.

Una acción sugerida tiene que incluir una hipótesis comprobable. «Probar un horario ampliado para visitas largas» es una propuesta que se puede evaluar; «este grupo es fiel» puede exceder el que mesuran tres variables.

El laboratorio genera 270 perfiles sintéticos de biblioteca y compara K-Means, estabilidad entre entonces, DBSCAN y una proyección PCA. La mezcla de partida facilita el experimento, pero los hashtags latentes no se utilizan como respuesta. El trabajo consiste a justificar una lectura, no a adivinar los números del generador.

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
"""Perfils sintètics d'ús d'una biblioteca. Les etiquetes no són veritat externa."""
from pathlib import Path
import numpy as np
import pandas as pd
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans, DBSCAN
from sklearn.metrics import silhouette_score, adjusted_rand_score
from sklearn.decomposition import PCA

def main():
    rng = np.random.default_rng(14)
    centers = [(2, 15, 60), (7, 60, 10), (4, 100, 25)]
    data = np.vstack([rng.normal(c, [1, 12, 8], size=(90, 3)) for c in centers])
    df = pd.DataFrame(np.maximum(data, 0), columns=['visites_mes', 'minuts_visita', 'dies_absencia'])
    # Escalem perquè minuts, visites i dies no dominen només per unitats.
    scaler = StandardScaler()
    z = scaler.fit_transform(df)
    scores = {}
    models = {}
    for k in range(2, 6):
        models[k] = KMeans(n_clusters=k, n_init=10, random_state=42).fit(z)
        scores[k] = silhouette_score(z, models[k].labels_)
    # La selecció per silhouette és exploratòria, no una veritat externa.
    k = max(scores, key=scores.get)
    labels = models[k].labels_
    # ARI compara particions encara que els números d’etiqueta canvien.
    replica = KMeans(n_clusters=k, n_init=10, random_state=9).fit_predict(z)
    print('Silhouette:', scores, 'k exploratori:', k)
    print('Estabilitat entre llavors (ARI):', adjusted_rand_score(labels, replica))
    df['grup'] = labels
    print(df.groupby('grup').mean().round(2).to_string())
    # −1 representa soroll de DBSCAN, no un grup addicional.
    db = DBSCAN(eps=0.65, min_samples=8).fit_predict(z)
    print('DBSCAN: grups', len(set(db) - {-1}), 'soroll', int((db == -1).sum()))
    coords = PCA(n_components=2).fit_transform(z)
    assert coords.shape == (270, 2) and df.grup.nunique() == k
    out = Path(__file__).resolve().parent / 'eixides'
    out.mkdir(exist_ok=True)
    df.to_csv(out / 'perfils.csv', index=False)
if __name__ == '__main__':
    main()
```

## Prácticas guiadas

Cada práctica deja una evidencia pequeña: una salida comprobada y una explicación de la decisión. Los tiempos son orientativos y no incluyen instalación.

### Práctica 1 · Preparar perfiles comparables

**Nivel y tiempo:** Inicial · 20–30 min.

**Objetivo:** Entender el efecto de las unidades antes de calcular distancias.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Ejecuta el ejemplo y describe las tres variables de uso de biblioteca.
2. Compara rangos originales de visitas, minutos y días de ausencia.
3. Localiza la estandarización y comprueba medias aproximadas cero y desviaciones poblacionales aproximadas uno.
4. Relaciona cada fila con una persona sintética y descarta identificadores como variables de distancia.

!!! success "Comprobación y evidencia esperada"
    La matriz usada en clustering tiene 270 filas y tres características escaladas.

**Preguntas de reflexión:**

- Por qué una variable con rango más grande puede dominar la distancia?
- Qué significa una coordenada negativa después de escalar?
- Qué información se perdería eliminando una variable sin revisarla?

**Ampliación opcional:** Compara una agrupación sin escalar y explica qué variable parece dominarla.

### Práctica 2 · Elegir una agrupación exploratoria

**Nivel y tiempo:** Intermedia · 30–45 min.

**Objetivo:** Combinar silhouette con interpretación de los perfiles.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Registra los resultados para k entre 2 y 5.
2. Revisa el k seleccionado por el máximo de silhouette sin denominarlo número real de grupos.
3. Calcula medida, media y mediana de cada perfil en unidades originales.
4. Propone nombres descriptivos basados en comportamientos observados y una posible utilidad para el servicio bibliotecario.

!!! success "Comprobación y evidencia esperada"
    Cada nombre se justifica con valores y no expresa un juicio sobre el valor de las personas.

**Preguntas de reflexión:**

- Que medida silhouette y que no medida?
- Por qué dos agrupaciones pueden ser útiles para preguntas diferentes?
- Qué riesgo tiene dar un nombre demasiado categórico a un grupo?

**Ampliación opcional:** Compara los perfiles con k inmediatamente inferior y describe qué grupo se ha fusionado.

### Práctica 3 · Estabilidad y puntos sin grupo

**Nivel y tiempo:** Avance · 45–60 min.

**Objetivo:** Examinar dependencia de la inicialización y del modelo de agrupación.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Compara los hashtags de dos entonces mediante ARIO, sin exigir que los números de grupo coincidan.
2. Revisa el resultado de DBSCAN y separa grupos del hashtag −1.
3. Varía `eps` en una copia y registra número de grupos y puntos de ruido.
4. Explica por qué una proyección PCA de dos dimensiones puede ocultar separaciones presentes en tres.

!!! success "Comprobación y evidencia esperada"
    El informe distingue estabilidad de entonces, sensibilidad de densidad y limitación de proyección.

**Preguntas de reflexión:**

- Por qué comparar números de hashtag hila a fila puede ser engañoso?
- Un punto marcado como ruido es necesariamente un dato incorrecto?
- Por qué estabilidad no equivale a utilidad?

**Ampliación opcional:** Repite el análisis en una submostra y compara perfiles, manteniendo explícita la población que se ha excluido.

## Ejercicios autónomos

Intenta resolver cada reto antes de desplegar la orientación. Se valora el razonamiento y las comprobaciones, no solo obtener una cifra.

### Reto 1 · Hashtags permutados

Compara dos resultados idénticos que denominan los grupos 0/1 y 1/0.

??? example "Solución orientativa y criterio de revisión"
    La partición es la misma aunque ningún número coincida. Una métrica como ARIO tiene en cuenta la pertenencia compartida y no exige la misma numeración.

### Reto 2 · Un identificador numérico

Argumenta por qué no tienes que escalar e incluir automáticamente el número de carné en las distancias.

??? example "Solución orientativa y criterio de revisión"
    Que sea numérico no le da un significado cuantitativo. La proximidad entre números de carné no representa necesariamente similitud de comportamiento.

### Reto 3 · Del perfil a la acción

Propone una acción de servicio para un perfil y una manera de evaluarla.

??? example "Solución orientativa y criterio de revisión"
    Por ejemplo, ofrecer información de horarios a usuarios con ausencias largas y mesurar respuesta con un diseño adecuado. El cluster no prueba por sí mismo la causa de la ausencia ni el efecto de la acción.

## Errores habituales y diagnóstico

| Síntoma | Causa que conviene investigar | Comprobación o corrección |
|---|---|---|
| Un grupo domina el espacio | Escalas muy diferentes o variable irrelevante. | Revisa unidades y selección antes de cambiar k. |
| Cambian los números de grupo | Permutación de hashtags. | Compara pertenencia con ARIO y perfiles. |
| DBSCAN marca casi todo como ruido | Parámetros de densidad inadecuados por la escala. | Revisa distancias, eps y min_samples sin forzar un número de grupos. |

## Autoevaluación y evidencias

Antes de dar la unidad por trabajada, comprueba estos puntos y escribe una frase de evidencia para cada uno:

- [ ] Puedo construir una matriz de características con una fila por entidad.
- [ ] Puedo comparar agrupaciones con métricas y perfiles.
- [ ] Puedo examinar estabilidad y limitaciones de los hashtags.
- [ ] He ejecutado el laboratorio y he contrastado al menos un resultado independientemente.
- [ ] Puedo explicar una limitación y un caso en que el procedimiento requeriría cambios.

**Entregable de la píldora:** copia de trabajo reproducible, resultados de la práctica seleccionada y un texto breve que indico pregunta, decisión, comprobación y limitación. Si se usa dentro de BiciTierra Market, integra esta evidencia en el entregable semanal correspondiente; no hay que crear una entrega duplicada.

## Fuentes para profundizar

- [Documentació oficial de referència](https://scikit-learn.org/stable/modules/clustering.html). Consulta especialmente los conceptos y los supuestos descritos en la unidad.

Las versiones ejecutadas y los límites de la comprobación están en el [registre de validació](../VALIDACIO.md). Los ejemplos son originales y los datos son sintéticos.

## Aplicación final a BiciTierra Market

**Momento orientativo:** semana 4. Esta correspondencia ayuda a elegir materiales y no sustituye el documento de trabajo del alumnado.

Construye perfiles de customers.csv con variables justificadas y una fila por cliente. Excluye identificadores de las distancias. Libra medidas, valores representativos, estabilidad y una interpretación prudente antes de proponer acciones comerciales.

**Transferencia:** identifica qué concepto acabas de practicar, qué dato del proyecto lo exige y que tienes que cambiar respecto del laboratorio. Justifica esa adaptación antes de copiar código.
