---
title: "Exploración y calidad de datos"
description: "Cimientos, ejemplos reproducibles y práctica progresiva: exploración y calidad de datos."
tags: [IABD, datos, formación]
icon: material/book-open-page-variante
status: ampliada
---

# 04 · Exploración y calidad de datos

[← Índex de les píndoles](../index.md)

!!! abstract "Qué aprenderás"
    - Definir qué representa cada fila.
    - Auditar ausencias, formatos, duplicados y valores extremos.
    - Documentar decisiones de limpieza y sus consecuencias.

**Conocimientos previos:** Operaciones básicas con pandas.

**Dedicación orientativa:** 3–4 h para lectura y laboratorio guiado; las extensiones y los ejercicios autónomos requieren tiempo adicional.

!!! tip "Dos recorridos posibles"
    **Esencial:** lee los cimientos, ejecuta el laboratorio y completa la práctica inicial. **Profundización:** continúa con las prácticas intermedia y avance y los ejercicios. El profesorado puede seleccionarlas según los conocimientos previos; no son todas obligatorias dentro del proyecto.

## Mapa de la unidad

Explorar es formular y contrastar preguntes → Dimensiones de calidad → Contrato de datos y granularidad → Nulos, errores de formato y valores extremos → Distribuciones y relaciones → La EDA también puede contaminar la evaluación → Decisiones de limpieza con trazabilidad → Como cerrar una exploración

## 1. Explorar es formular y contrastar preguntas

El análisis exploratorio de datos, o EDA, es el proceso de entender una muestra antes de construir explicaciones o modelos. Incluye estructura, calidad, distribuciones, relaciones y límites de representatividad. No es una colección obligatoria de gráficos: cada operación tiene que responder una pregunta.

En un servicio de atención, una disminución del tiempo mediano podría indicar mejora, pero también desaparición de los casos difíciles del registro. En sensores, valores idénticos durante horas pueden representar estabilidad o una lectura bloqueada. La estadística necesita contexto de captura.

Empieza con tres frases: qué representa una fila, de qué población proviene y qué decisión queremos ayudar a tomar. Después identifica quién genera el dato, con qué frecuencia y qué cambios de sistema pueden afectarla.

## 2. Dimensiones de calidad

| Dimensión | Pregunta de control | Ejemplo |
|---|---|---|
| Completesa | Falta información necesaria? | Solicitudes sin fecha |
| Validez | El valor cumple el formato y el dominio? | Temperatura textual no convertible |
| Coherencia | Dos campos compatibles concuerdan? | Fecha de cierre anterior a la apertura |
| Unicidad | Hay repeticiones de una misma observación? | Dos registros con la misma clave de event |
| Actualidad | El dato corresponde al periodo útil? | Catálogo de servicios desactualitzat |
| Cobertura | Quedan fuera grupos o periodos relevantes? | Sensores de una sola planta |

Una mesa sin nulos puede tener mala calidad si todos los vacíos se han convertido en cero. Igualmente, una mesa con nulos puede ser útil si están identificados y el procedimiento los trata de manera coherente.

## 3. Contrato de datos y granularidad

Un contrato de datos describe campos, tipos, unidades, claves, dominios y reglas. No hay que empezar con una plataforma compleja: una tabla de definiciones y unas comprobaciones ejecutables ya evitan muchas ambigüedades.

```python title="Comprovacions explícites sobre una taula menuda"
import pandas as pd

df = pd.DataFrame({"id":[1,2,3], "unitats":[2,0,5], "preu":[10.,8.,12.]})
assert df["id"].is_unique
assert df["unitats"].ge(0).all()
df["import"] = df["unitats"] * df["preu"]
print(df)
```

Estos asertos son comprobaciones didácticas. Una aplicación productiva tiene que definir como rechaza, aísla o registra las filas inválidas, sin depender que los asertos estén activados.

La granularidad determina la clave. En un sensor, puede ser dispositivo e instante; en ventas agregadas, mes, canal y producto. No eliminas duplicados por una columna que no identifica la unidad de observación: dos pagos de un mismo cliente pueden ser dos hechos legítimos.

## 4. Nulos, errores de formato y valores extremos

Un valor ausente puede deberse a un fallo aleatorio, a una regla de negocio o a un proceso selectivo. Si las incidencias más complejas son las que no tienen tiempo registrado, calcular la media solo con observadas infrarepresenta la dificultad. La información disponible no siempre permite identificar el mecanismo de falta de datos, pero sí formular y comprobar hipótesis.

Un valor extremo tampoco es necesariamente erróneo. Una jornada de consumo excepcional puede corresponder a una avería real. Recortar automáticamente con `clip` altera las evidencias. Primero comprueba unidades, sensor, contexto, distribución y registros relacionados.

El rango interquartílic, IQR, es Q3−Q1. La regla Q1−1,5×IQR y Q3+1,5×IQR marca observaciones para revisar; no es un criterio universal de eliminación. En distribuciones asimétricas puede señalar muchos casos legítimos.

## 5. Distribuciones y relaciones

La media es sensible a extremos; la mediana describe el centro por posición. Necesitamos también dispersión y medida de muestra. Dos equipos con la misma media pueden tener riesgos muy diferentes si uno es estable y el otro alterna resultados muy bajos y muy altos.

Examina cuantiles, histogramas y relaciones entre variables. Desagrega cuando el significado lo pido: una relación global puede desaparecer o invertirse cuando se distinguen grupos. La correlación lineal próxima a cero no descarta una relación curva, y una correlación alta no prueba una causa.

En una tabla de centros educativos, la asociación entre recursos e incidencias puede reflejar medida del centro. Antes de concluir que más recursos generan problemas, revisa denominadores, cobertura y variables de contexto.

## 6. El EDA también puede contaminar la evaluación

Si se eligen variables porque se ve que funcionan sobre el conjunto de prueba, esa prueba ya ha influido en el modelo. La separación temporal o de grupos se tiene que plantear antes de la exploración orientada a decisiones predictivas.

Se pueden comprobar claves y formatos globales para entender el extracto, pero los patrones usados para elegir modelos y transformaciones tienen que proceder del entrenamiento y de la validación acordada. Documenta qué se ha mirado y para qué.

En problemas descriptivos sin modelo predictivo, la pregunta es diferente: interesa explicar fielmente la población observada y limitar la generalización. No hay que imponer una partición artificial a toda mesa solo porque se trabaje con Python.

## 7. Decisiones de limpieza con trazabilidad

Una buena limpieza deja una tabla de auditoría: regla, filas afectadas, justificación y efecto sobre los resultados. Conserva la fuente inmutable y genera una versión derivada. Esto permite comparar decisiones sin reconstruir manualmente el estado anterior.

En el laboratorio hay una ausencia, un texto no numérico, una temperatura fuera de rango y una repetición exacta. La solución distingue cuatro flags y conserva el valor original. Solo retira del análisis las observaciones que no cumplen el contrato definido.

No se imputa una lectura imposible como si hubiera sido observada. Si posteriormente se quiere imputar, se tendrá que registrar la variable derivada, el método y su efecto. Un «dato limpio» no es un dato sin historia.

## 8. Como cerrar una exploración

Un informe de EDA tiene que acabar con conclusiones operativas: qué es utilizable, que queda incierto, qué transformaciones se proponen y qué riesgo pueden introducir. Incluye al menos una limitación de cobertura y una comprobación que todavía falto.

Por ejemplo: «las lecturas de dos salas tienen formatos consistentes, pero la tercera contiene errores de conversión; el resumen excluye estas filas y no representa todo el periodo». Es más útil que «hemos eliminado nulos y hecho gráficos», porque explica a que se aplica el resultado.

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
"""Auditoria d'una taula de sensors: no corregix silenciosament els extrems."""
from pathlib import Path
import pandas as pd

def main():
    raw = pd.DataFrame({'id': [1, 2, 2, 3, 4, 5, 6], 'sala': [' A ', 'a', 'a', 'B', 'B', 'C', 'C'], 'temperatura': ['20', '21', '21', None, '999', '23', 'error']})
    # Treballem sobre una còpia i conservem els valors originals.
    df = raw.copy()
    df['sala'] = df.sala.str.strip().str.upper()
    df['valor'] = pd.to_numeric(df.temperatura, errors='coerce')
    df['absent_origen'] = df.temperatura.isna()
    df['error_parseig'] = df.valor.isna() & ~df.absent_origen
    df['fora_rang'] = df.valor.notna() & ~df.valor.between(-20, 60)
    df['duplicat_excedent'] = df.duplicated('id', keep='first')
    # El cas conegut repetit és exacte; els conflictes exigirien una altra política.
    usable = df.loc[~df.duplicat_excedent & ~df.fora_rang & df.valor.notna()].copy()
    assert len(usable) == 3 and set(usable.id) == {1, 2, 5}
    report = {'files': len(df), 'absents': int(df.absent_origen.sum()), 'parseig': int(df.error_parseig.sum()), 'fora_rang': int(df.fora_rang.sum()), 'duplicats_excedents': int(df.duplicat_excedent.sum())}
    out = Path(__file__).resolve().parent / 'eixides'
    out.mkdir(exist_ok=True)
    df.to_csv(out / 'auditoria.csv', index=False)
    usable.to_csv(out / 'lectures-valides.csv', index=False)
    print(report)
    print('Files vàlides sense duplicar:', len(usable))
if __name__ == '__main__':
    main()
```

## Prácticas guiadas

Cada práctica deja una evidencia pequeña: una salida comprobada y una explicación de la decisión. Los tiempos son orientativos y no incluyen instalación.

### Práctica 1 · Inventario de problemas

**Nivel y tiempo:** Inicial · 20–30 min.

**Objetivo:** Detectar problemas antes de modificar valores.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Ejecuta la auditoría y compara los datos originales con `auditoria.csv`.
2. Localiza una ausencia original, un error de conversión, un valor fuera de rango y una repetición excedente.
3. Comprueba que las salas se han normalizado pero la temperatura original se conserva.
4. Crea una mesa con problema, número de casos, regla aplicada y decisión pendiente.

!!! success "Comprobación y evidencia esperada"
    Hay 7 filas; cada una de las cuatro banderas cuenta un caso. Quedan 3 lecturas utilizables.

**Preguntas de reflexión:**

- Una misma fila podría activar varias banderas?
- Por qué no conviene sumar siempre los recuentos como si fueron excluyentes?
- Qué permite auditar conservar el valor original?

**Ampliación opcional:** Calcula el porcentaje de filas con al menos una incidencia mediante una combinación lógica de banderas.

### Práctica 2 · Duplicado u observación repetida?

**Nivel y tiempo:** Intermedia · 30–45 min.

**Objetivo:** Distinguir una repetición exacta de un conflicto de identidad.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Revisa las dos filas con id 2 y comprueba que coinciden.
2. En una copia, cambia la temperatura de una de ellas a 22.
3. Antes de descartarla, crea un informe de los identificadores con más de un valor diferente.
4. Propone una política para los conflictos: consulta de fuente, cuarentena o selección justificada con marca temporal.

!!! success "Comprobación y evidencia esperada"
    El nuevo caso se trata como un conflicto a investigar; no se elimina automáticamente solo porque comparte id.

**Preguntas de reflexión:**

- Qué representa la clave en este conjunto?
- Cuando serían legítimas dos lecturas de la misma sala?
- Qué información haría posible elegir la lectura correcta?

**Ampliación opcional:** Añade una marca temporal y compara una clave simple con una clave compuesta sala–instando.

### Práctica 3 · Mesurar el efecto de limpiar

**Nivel y tiempo:** Avance · 45–60 min.

**Objetivo:** Relacionar decisiones de calidad con cambios en los resúmenes.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Calcula la media numérica antes de filtrar y después de aplicar la política del laboratorio.
2. Compárala con la mediana e identifica el efecto de 999.
3. Redacta un registro de decisiones que justifico el rango docente de −20 a 60 y la gestión del duplicado conocido.
4. Desa por separado datos auditados y datos utilizables, con recuento de entrada y salida.

!!! success "Comprobación y evidencia esperada"
    El resumen final corresponde a 20, 21 y 23; la media es aproximadamente 21,33. Ninguna corrección queda escondida.

**Preguntas de reflexión:**

- Una mediana robusta hace innecesaria la auditoría?
- Un valor extremo es siempre incorrecto?
- Por qué el rango de aceptación no se puede copiar sin revisarlo a cualquier sensor?

**Ampliación opcional:** Repite el resumen por sala y explica por qué algunos grupos quedan sin observaciones válidas.

## Ejercicios autónomos

Intenta resolver cada reto antes de desplegar la orientación. Se valora el razonamiento y las comprobaciones, no solo obtener una cifra.

### Reto 1 · Cero y ausencia

Diseña cuatro observaciones de consumo que incluyan cero, vacío, texto inválido y un valor positivo. Audítalas.

??? example "Solución orientativa y criterio de revisión"
    Cero es una observación posible; vacío y conversión fallada tienen que tener marcas distintas. La regla de rango depende de la unidad y del proceso.

### Reto 2 · Contrato de datos

Escribe un contrato para una tabla de préstamos con clave, tipo, valores admitidos y dos controles entre columnas.

??? example "Solución orientativa y criterio de revisión"
    Ejemplo: id único, fechas válidas, devolución posterior o igual al préstamo cuando existe, estado compatible con devolución. Diferencia préstamo todavía abierto de un error.

### Reto 3 · Decisión reversible

Propone como corregir un error conocido de sensor sin destruir el valor original.

??? example "Solución orientativa y criterio de revisión"
    Conserva original, valor corregido, regla, motivo y versión del proceso. Una tabla de auditoría permite reconstruir la decisión; sobrescribir sin traza no.

## Errores habituales y diagnóstico

| Síntoma | Causa que conviene investigar | Comprobación o corrección |
|---|---|---|
| Desaparecen demasiado hilas | Filtros encadenados sin recuento. | Registra cada regla y compara identificadores antes y después. |
| Todos los extremos corregidos al límite | Recorte automático sin evidencia. | Marca e investiga antes de modificar. |
| Conflictos tratados como duplicados exactos | Clave incompleta o regla indiscriminada. | Compara contenido y significado de la clave. |

## Autoevaluación y evidencias

Antes de dar la unidad por trabajada, comprueba estos puntos y escribe una frase de evidencia para cada uno:

- [ ] Puedo definir qué representa cada fila.
- [ ] Puedo auditar ausencias, formatos, duplicados y valores extremos.
- [ ] Puedo documentar decisiones de limpieza y sus consecuencias.
- [ ] He ejecutado el laboratorio y he contrastado al menos un resultado independientemente.
- [ ] Puedo explicar una limitación y un caso en que el procedimiento requeriría cambios.

**Entregable de la píldora:** copia de trabajo reproducible, resultados de la práctica seleccionada y un texto breve que indico pregunta, decisión, comprobación y limitación. Si se usa dentro de BiciTierra Market, integra esta evidencia en el entregable semanal correspondiente; no hay que crear una entrega duplicada.

## Fuentes para profundizar

- [Documentació oficial de referència](https://pandas.pydata.org/docs/user_guide/missing_data.html). Consulta especialmente los conceptos y los supuestos descritos en la unidad.

Las versiones ejecutadas y los límites de la comprobación están en el [registre de validació](../VALIDACIO.md). Los ejemplos son originales y los datos son sintéticos.

## Aplicación final a BiciTierra Market

**Momento orientativo:** semana 1–2. Esta correspondencia ayuda a elegir materiales y no sustituye el documento de trabajo del alumnado.

Elabora el primer informe de calidad de salas_history.csv y customers.csv por separado. Comprueba periodos, cobertura de combinaciones, rangos y ausencias. Separa la detección de problemas de las decisiones que dependerán del modelo o de la segmentación.

**Transferencia:** identifica qué concepto acabas de practicar, qué dato del proyecto lo exige y que tienes que cambiar respecto del laboratorio. Justifica esa adaptación antes de copiar código.
