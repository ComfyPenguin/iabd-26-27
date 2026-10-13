---
title: "NumPy y pandas: de los datos a las mesas"
description: "Cimientos, ejemplos reproducibles y práctica progresiva: numpy y pandas: de los datos a las mesas."
tags: [IABD, datos, formación]
icon: material/book-open-page-variant
status: ampliada
---

# 03 · NumPy y pandas: de los datos a las mesas

[← Índex de les píndoles](../index.md)

!!! abstract "Qué aprenderás"
    - Trabajar con arrays y operaciones vectoritzades.
    - Filtrar, agrupar y unir mesas conservando la granularidad.
    - Tratar fechas y ausencias sin confundirlas con ceros.

**Conocimientos previos:** Python básico y noción de filas y columnas.

**Dedicación orientativa:** 3–4 h para lectura y laboratorio guiado; las extensiones y los ejercicios autónomos requieren tiempo adicional.

!!! tip "Dos recorridos posibles"
    **Esencial:** lee los cimientos, ejecuta el laboratorio y completa la práctica inicial. **Profundización:** continúa con las prácticas intermedia y avance y los ejercicios. El profesorado puede seleccionarlas según los conocimientos previos; no son todas obligatorias dentro del proyecto.

## Mapa de la unidad

Del dato a la mesa → Arrays, dimensiones y broadcasting → Serías, DataFrame y índice → Leer y seleccionar datos → Tipos, nulos y transformaciones → Agregar con un denominador correcto → Uniones y concatenaciones → Fechas, resúmenes temporales y retrasos

## 1. Del dato a la mesa

Antes de escoger una función hay que entender qué representa una fila. Una mesa puede contener personas, transacciones, medidas de sensores o resúmenes mensuales. Mezclar estos niveles sin una regla produce errores que ninguna biblioteca detectará automáticamente.

NumPy proporciona arrays numéricos y operaciones sobre dimensiones. Pandas aporta `Series` y `DataFrame`, con hashtags, índices y operaciones tabulares. El primero es conveniente para cálculo numérico regular; el segundo, para datos con campos de tipos diferentes y claves. Las dos bibliotecas se complementan, pero no tienen exactamente las mismas reglas de alineación.

## 2. Arrays, dimensiones y broadcasting

Un array tiene una forma, un número de dimensiones y un tipo de dato. La forma `(3,)` representa tres elementos; `(3, 1)` representa tres filas y una columna. Esta diferencia afecta operaciones e interfaces de modelos.

```python title="Formes i operacions NumPy"
import numpy as np

minuts = np.array([30., 60., 90.])
print(minuts.shape)                    # (3,)
print(minuts / 60)                     # [0.5, 1.0, 1.5]
matriu = np.array([[1, 2, 3], [4, 5, 6]])
print(matriu + np.array([10, 20, 30]))  # Suma per columnes.
```

El **broadcasting* compara dimensiones desde la derecha: son compatibles si son iguales o una de ellas es 1. Un vector de forma `(3,)` y una columna `(3,1)` pueden generar una matriz `(3,3)`, no una resta elemento a elemento. Comprueba la forma antes y después de un cálculo; un resultado numérico no prueba que se haya calculado aquello que esperabas.

Otra distinción es el eje. `axis=0` agrega a lo largo de las filas y devuelve un valor por columna; `axis=1` agrega las columnas de cada fila. Escribir antes «quiero una media por persona» ayuda a escoger la operación adecuada.

## 3. Serías, DataFrame y índice

Una `Series` es una secuencia etiquetada. Un `DataFrame` combina columnas con un índice de filas. El índice no es necesariamente una clave de negocio: puede ser solo el contador creado al cargar un fichero.

```python title="L’alineació es fa per etiqueta"
import pandas as pd

a = pd.Series([10, 20], index=["web", "correu"])
b = pd.Series([1, 2], index=["correu", "web"])
print(a + b)  # correu: 21; web: 12, segons les etiquetes.
```

Convertir a NumPy elimina los hashtags. Es útil cuando una APIO requiere arrays, pero obliga a garantizar que filas y columnas mantienen el orden correcto. No conviertas para ocultar un problema de alineación sin investigarlo.

## 4. Leer y seleccionar datos

La carga tiene que hacer explícitos codificación, separador, decimales y fechas cuando puedan ser ambiguos. Un identificador como `00127` puede perder ceros iniciales si se convierte automáticamente a entero. Un `head()` correcto no demuestra que todas las filas se hayan interpretado bien.

```python title="Construcció i selecció reproduïble"
import pandas as pd

df = pd.DataFrame({"id":["001","002","003"],
                   "servei":["web","vpn","web"],
                   "minuts":[20, 50, 35]})
web = df.loc[df["servei"].eq("web"), ["id","minuts"]].copy()
web["hores"] = web["minuts"] / 60
print(web)
```

`loc` selecciona por hashtag; `iloc`, por posición. En una selección booleana con varias condiciones, usa paréntesis y `&`, `|` o `~`. Una copia explícita deja clara la intención de trabajar con una mesa derivada. Para modificar la mesa original, hiciera una asignación única con `.loc` en lugar de encadenar selecciones.

## 5. Tipo, nulos y transformaciones

Un nulo significa información no disponible, no necesariamente cero. La media de [10, nulo, 20] sobre valores observados es 15; llenar el nulo con cero la convertiría en 10 y cambiaría la interpretación.

`pd.to_numeric(..., errors='coerce')` puede ser útil, pero convierte a nulo tanto la ausencia como el texto que no se ha podido interpretar. Conserva una máscara de error de conversión si la distinción importa. Las fechas requieren el mismo criterio.

```python title="Distingir absència i error de conversió"
import pandas as pd

text = pd.Series(["10", None, "error", "0"])
valor = pd.to_numeric(text, errors="coerce")
error_format = valor.isna() & text.notna()
print(pd.DataFrame({"original":text,"valor":valor,"error_format":error_format}))
```

La imputación por mediana o media es una decisión analítica. En aprendizaje automático, la regla se ajusta con entrenamiento y después se aplica a validación y prueba. Una transformación aparentemente simple puede filtrar información si se calcula sobre todo el conjunto.

## 6. Agregar con un denominador correcto

`groupby` separa grupos, aplica una operación y reúne resultados. `size` cuenta filas; `count` cuenta valores no nulos del campo. Cuando hay nulos, las dos cifras explican cosas diferentes.

Una tasa global no es siempre la media de tasas de grupo. Si un equipo resuelve 9 de 10 casos y otro 1 de 100, la tasa conjunta es 10/110, no la media de 90% y 1%. Calcula numerador y denominador compatibles antes de agregar porcentajes.

Con `sum(min_count=1)`, un grupo sin ningún valor observado puede conservar el nulo en lugar de convertirse en cero. Una mesa dinámica también puede generar celdas ausentes: no las ompligues automáticamente si no sabes si representan ausencia de actividad o de registro.

## 7. Uniones y concatenaciones

Una unión asocia filas por una clave; una concatenación apila o alinea mesas. Dos campos con el mismo nombre no demuestran que exista una relación válida.

El parámetro `validate='many_to_one'` comprueba que la tabla de dimensión tiene una sola fila por clave. `indicator=True` permite detectar filas sin correspondencia. Comprueba también el número de filas y los totales antes y después de la unión.

En el laboratorio, las incidencias se relacionan con el catálogo de servicios. «vpn» no tiene responsable registrado; una unión izquierda conserva la incidencia y expone el vacío. Una unión interna lo eliminaría, reduciendo silenciosamente el volumen analizado.

## 8. Fechas, resúmenes temporales y retrasos

Ordenar texto de fecha solo funciona en formatos diseñados para mantener la orden, como ISO. Convierte a fecha cuando necesitas periodos, diferencias o reglas temporales. El cambio de zona horaria es otra decisión: una fecha local sin zona puede ser ambigua en registros distribuidos.

`resample('MS')` agrupa en periodos mensuales etiquetados al inicio. `rolling(3)` opera sobre tres observaciones; no significa siempre tres meses. Si faltan periodos, una ventana de filas no equivale a una ventana de tiempo.

Para un predictor retrasado, ordena por entidad y tiempo antes de `groupby(...).shift(1)`. Una media móvil predictiva tiene que desplazarse porque no incluya el resultado del momento que pretende anticipar. Estas reglas importan en energía, demanda, incidencias y otros muchos datos con dependencia temporal.

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
"""Taules i arrays: unió controlada d'incidències amb serveis."""
from pathlib import Path
import numpy as np
import pandas as pd

def main():
    temps = np.array([20.0, 30.0, 50.0])
    assert temps.shape == (3,)
    print('Minuts a hores:', temps / 60)
    incidencies = pd.DataFrame({'id': [1, 2, 3, 4, 5], 'servei': ['web', 'web', 'correu', 'correu', 'vpn'], 'minuts': [20.0, 30.0, None, 50.0, 10.0], 'data': ['2026-01-01', '2026-01-03', '2026-02-01', '2026-02-03', '2026-02-04']})
    serveis = pd.DataFrame({'servei': ['web', 'correu'], 'responsable': ['Equip A', 'Equip B']})
    incidencies['data'] = pd.to_datetime(incidencies['data'], format='%Y-%m-%d')
    # Exigim una sola coincidència de catàleg per incidència.
    resultat = incidencies.merge(serveis, on='servei', how='left', validate='many_to_one', indicator=True)
    assert len(resultat) == 5
    assert (resultat['_merge'] == 'left_only').sum() == 1
    # size compta files; count només valors no absents.
    resum = resultat.groupby('servei', dropna=False).agg(files=('id', 'size'), observades=('minuts', 'count'), mitjana=('minuts', 'mean'))
    # min_count evita presentar absència completa com a suma zero.
    mensual = incidencies.set_index('data')['minuts'].resample('MS').sum(min_count=1)
    out = Path(__file__).resolve().parent / 'eixides'
    out.mkdir(exist_ok=True)
    resultat.to_csv(out / 'incidencies.csv', index=False)
    resum.to_csv(out / 'resum-serveis.csv')
    print(resum.to_string())
    print('Totals mensuals observats:\n', mensual)
    print('Nuls conservats:', resultat.minuts.isna().sum())
if __name__ == '__main__':
    main()
```

## Prácticas guiadas

Cada práctica deja una evidencia pequeña: una salida comprobada y una explicación de la decisión. Los tiempos son orientativos y no incluyen instalación.

### Práctica 1 · Contar aquello que hay y aquello que falta

**Nivel y tiempo:** Inicial · 20–30 min.

**Objetivo:** Distinguir filas totales de valores observados dentro de cada grupo.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Ejecuta el laboratorio y abre `resum-serveis.csv`.
2. Reconstruye manualmente el grupo corráis: dos registros, un valor de minutos ausente y un valor 50.
3. Compara las columnas `files`, `observades` y `mitjana`. Calcula qué pasaría si imputamos cero.
4. Escribe una nota que diferencio la media observada de la media que se obtendría con aquella imputación.

!!! success "Comprobación y evidencia esperada"
    Correo tiene 2 filas, 1 valor observado y media 50; sustituir la ausencia por cero produciría 25.

**Preguntas de reflexión:**

- Por qué `size` y `count` dan resultados diferentes?
- Es legítimo considerar la ausencia como cero?
- Qué dato faltaría para decidir una imputación?

**Ampliación opcional:** Añade una columna de porcentaje de ausencias por servicio.

### Práctica 2 · Unir mesas con un contrato

**Nivel y tiempo:** Intermedia · 30–45 min.

**Objetivo:** Verificar cobertura y cardinalidad de una unión antes de usarla.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Localiza la unión con `validate="many_to_one"` y explica qué exige en el catálogo.
2. Identifica el registro VPN sin correspondencia a partir de `_merge`.
3. En una copia, duplica el servicio web dentro del catálogo y ejecuta la unión para observar el error.
4. Restaura la unicidad e incorpora VPN con un responsable explícito. Actualiza la comprobación de cobertura.

!!! success "Comprobación y evidencia esperada"
    El catálogo duplicado se rechaza; con tres servicios únicos la unión conserva 5 filas y no queda ningún `left_only`.

**Preguntas de reflexión:**

- Qué total podría hincharse si eliminamos el control de cardinalidad?
- Cuando tendría sentido una unión muchos a muchos?
- Por qué una unión izquierda facilita auditar registros sin catálogo?

**Ampliación opcional:** Compara una unión interna y una izquierda sobre el catálogo original y explica qué fila desaparece.

### Práctica 3 · Resumir por periodos

**Nivel y tiempo:** Avance · 45–60 min.

**Objetivo:** Construir un agregado mensual que conservo el significado de los nulos.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Revisa la conversión explícita de fechas y el uso de `resample("MS")`.
2. Comprueba a mano los totales observados de enero y febrero: 50 y 60 minutos.
3. Crea un mes adicional con todas las duraciones ausentes y usa `sum(min_count=1)`.
4. Exporta un resumen con total observado y recuento de duraciones disponibles, y redacta una nota de calidad.

!!! success "Comprobación y evidencia esperada"
    El mes sin ninguna duración observada queda ausente, no como un cero de trabajo.

**Preguntas de reflexión:**

- Qué diferencia hay entre ninguna fila y filas sin duración?
- Por qué hay que ordenar o indexar correctamente las fechas?
- Qué información se pierde cuando solo exportamos la suma?

**Ampliación opcional:** Agrupa a la vez por mes y servicio y comprueba que la suma de los totales observados coincide con la del conjunto.

## Ejercicios autónomos

Intenta resolver cada reto antes de desplegar la orientación. Se valora el razonamiento y las comprobaciones, no solo obtener una cifra.

### Reto 1 · Un grupo sin valores

Crea tres filas de un servicio nuevo con minutos ausentes y compara size, count, mean y sum(min_count=1).

??? example "Solución orientativa y criterio de revisión"
    Obtendrás 3 filas, 0 observaciones, media ausente y suma ausente. Una suma cero sin min_count podría ocultar que no se ha observado ninguna duración.

### Reto 2 · Claus compuestas

Diseña una unión entre lecturas y catálogo cuando un mismo código de sensor se repite en edificios diferentes.

??? example "Solución orientativa y criterio de revisión"
    La clave puede ser edificio más sensor si esa combinación identifica una única entrada de catálogo. Comprueba unicidad y usa las dos columnas en la unión.

### Reto 3 · Vectoritzar una regla

Crea una columna booleana para duraciones observadas superiores a 45 minutos sin recorrer filas con un bucle.

??? example "Solución orientativa y criterio de revisión"
    Usa una máscara sobre la serie y decide explícitamente como representar ausencias. Si marcas los nulos como False, documenta que significa no confirmado, no duración corta demostrada.

## Errores habituales y diagnóstico

| Síntoma | Causa que conviene investigar | Comprobación o corrección |
|---|---|---|
| Aumento de filas después de unir | Clave duplicada en el catálogo. | Comprueba unicidad y validate antes de agregar. |
| Media demasiada baja | Ausencias sustituidas por cero sin justificación. | Conserva ausencias e informa del recuento observado. |
| Fechas mal interpretadas | Formato o configuración regional ambigua. | Indica formato y comprueba fechas que distingan día de mes. |

## Autoevaluación y evidencias

Antes de dar la unidad por trabajada, comprueba estos puntos y escribe una frase de evidencia para cada uno:

- [ ] Puedo trabajar con arrays y operaciones vectoritzades.
- [ ] Puedo filtrar, agrupar y unir mesas conservando la granularidad.
- [ ] Puedo tratar fechas y ausencias sin confundirlas con ceros.
- [ ] He ejecutado el laboratorio y he contrastado al menos un resultado independientemente.
- [ ] Puedo explicar una limitación y un caso en que el procedimiento requeriría cambios.

**Entregable de la píldora:** copia de trabajo reproducible, resultados de la práctica seleccionada y un texto breve que indico pregunta, decisión, comprobación y limitación. Si se usa dentro de BiciTierra Market, integra esta evidencia en el entregable semanal correspondiente; no hay que crear una entrega duplicada.

## Fuentes para profundizar

- [Documentació oficial de referència](https://pandas.pydata.org/docs/user_guide/merging.html). Consulta especialmente los conceptos y los supuestos descritos en la unidad.

Las versiones ejecutadas y los límites de la comprobación están en el [registre de validació](../VALIDACIO.md). Los ejemplos son originales y los datos son sintéticos.

## Aplicación final a BiciTierra Market

**Momento orientativo:** semana 1–2. Esta correspondencia ayuda a elegir materiales y no sustituye el documento de trabajo del alumnado.

Carga los CSV del proyecto, verifica claves y granularidad antes de unirlos y construye agregados mensuales con recuento de observaciones. No presupones que el fichero de clientes se puede unir directamente a ventas agregadas sin una clave y una relación justificadas.

**Transferencia:** identifica qué concepto acabas de practicar, qué dato del proyecto lo exige y que tienes que cambiar respecto del laboratorio. Justifica esa adaptación antes de copiar código.
