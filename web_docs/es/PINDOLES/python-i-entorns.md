---
title: Python I Entornos
---

# Python I Entornos

## Por qué hay que explicitar esta base

En este curso hagamos muchas cosas con `Python`, pero no conviene suponer que todo el alumnado llega con el mismo automatismo.

La idea de esta píldora no es volver a hacer un curso entero de programación general, sino asegurar una base común para poder:

- leer y escribir scripts simples
- trabajar con notebooks sin depender siempre de ejemplos copiados
- preparar datos y hacer pruebas de modelado
- construir Apios o demos de IA sin bloqueos básicos

## El núcleo que sí tenemos que dominar

- variables y tipos habituales
- listas, diccionarios, tuples y conjuntos
- condicionales y bucles
- funciones
- importes
- lectura y escritura de ficheros
- errores habituales y como leer un traceback

No todos estos bloques tienen el mismo peso teórico, pero todos tienen mucho peso operativo. En este curso, un alumno no se bloquea tanto por no saber una teoría muy abstracta como por no saber leer una estructura de datos, separar una regla en una función o entender por qué uno script rompe al ejecutarse.

## Qué significa realmente "dominar la base"

No significa saber de memoria todas las instrucciones del lenguaje.

Significa sobre todo ser capaz de:

- leer código ya escrito y entender qué hace
- modificar una regla simple sin romper el conjunto
- pasar de una idea verbal a una función pequeña
- detectar si un error es de datos, de sintaxis o de dependencias

Dedo de otro modo: queremos autonomía funcional, no exhibición de lenguaje.

## Estructuras que aparecen constantemente al curso

### Listas

```python
temperatures = [19.2, 20.1, 18.7]
```

Van bien cuando la orden importa y queremos recorrer elementos.

### Diccionarios

```python
ticket = {
    "id": "T-014",
    "canal": "web",
    "prioritat": "alta"
}
```

Son especialmente útiles cuando representamos registros, respuestas JSON o configuraciones.

### Por qué esto es tan importante en IA y datos

Una gran parte del curso no trata objetos "puros" de programación clásica, sino:

- registros de datasets
- mensajes de sensores
- payloads de APIO
- configuraciones de modelo o de escenario

Por eso las listas y los diccionarios no son solo una base generalista. Son la forma concreta en que el alumnado se encontrará la mayor parte de los datos en trabajo real.

## Funciones: punto clave de madurez

Cuando una secuencia de pasos empieza a repetirse, hay que extraerla a una función.

```python
def classifica_risc(valor, llindar):
    if valor >= llindar:
        return "alerta"
    return "normal"
```

Aquí el valor no es con solo que el código quedo más corto. El valor es que la regla se vuelve explícita, reutilizable y fácil de probar.

También hay una idea pedagógica importante: cuando el alumnado escribe toda la lógica seguida, todavía no está separando bien problema y solución. Cuando empieza a encapsular reglas en funciones, normalmente ya está pensando con más estructura.

## Variables, tipos y conversiones

En cursos como este, muchos errores no vienen de algoritmos difíciles, sino de confundir tipos.

Ejemplos muy habituales:

- un número leído como texto
- un campo vacío que provoca error al convertir a `float`
- comparar una cadena con un entero sin darse cuenta

Ejemplo:

```python
valor = "18.5"
humitat = float(valor)
```

Si el alumnado no entiende esta conversión simple, después le costará mucho interpretar por qué fallan los cálculos, los umbrales o el preprocessament.

## Condicionales y bucles como base de regla operativa

En proyectos de datos y IA hay mucho modelo, pero también mucha regla.

Por ejemplo:

- si la humedad baja de un umbral, genera alerta
- si el texto de un ticket contiene cierto patrón, revísalo
- si una fila tiene campos críticos vacíos, exclúyela o márcala

Estos casos no piden teoría sofisticada. Piden buen uso de condicionales, bucles y estructura clara.

## Entornos de trabajo

En este curso trabajaremos sobre todo con:

- `Jupyter Notebook` para exploración y pruebas
- `VS Code` para scripts, Apios y proyectos más estables
- entornos virtuales para separar dependencias

Ejemplo mínimo:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install pandas scikit-learn matplotlib seaborn
```

La idea importante no es el mando en si, sino que resuelve:

- separar dependencias de un proyecto respecto a otro
- evitar que una instalación "rompo" un entorno que ya funcionaba
- poder explicar con más claridad que necesita un notebook o una demo para ejecutarse

## Como leer un error sin entrar en pánico

Cuando uno script falla, no conviene leer el terminal como si todo tuviera el mismo peso.

A menudo la parte importante es:

- el último mensaje de error
- la línea donde ha fallado
- el tipo de error: `NameError`, `KeyError`, `ValueError`, `ModuleNotFoundError`

Ejemplo mental:

- `ModuleNotFoundError`: falta una dependencia o el entorno no es el correcto
- `KeyError`: el campo o la clave no existe
- `ValueError`: el valor no tiene el formato que esperábamos

Si el alumnado aprende esta lectura mínima, gana mucha autonomía con muy poca teoría extra.

## Error habitual

Instalar paquetes de manera desordenada y no saber después en qué en torno funcionaba cada cosa.

## Ejemplo guiado de clase

Situación:

en `Predicció Comercial`, queremos leer un CSV y marcar si una fila merece revisión.

```python
def necessita_revisio(import_total, descompte):
    return import_total < 0 or descompte > 0.6
```

Todavía no hay modelo ni dashboard. Pero si esta base falla, después todo el pipeline se hace frágil.

Podemos ampliar la lectura así:

1. leer la fila
2. decidir si hay incoherencia
3. marcarla por revisión
4. después, solo después, pensar si esto entra a modelo o a panel

El mensaje docente está claro: antes de usar herramientas avanzadas, hay que saber expresar una regla simple de negocio o calidad.

## Práctica guiada sugerida

Situación:

tenéis un fichero con incidentes de terminal o lecturas de sensores y queréis marcar registros sospechosos.

 1:

definís una función que reviso una sola fila.

 2:

recorréis una lista o conjunto de registros y guardáis los casos sospechosos.

 3:

explicáis por escrito:

- qué regla habéis aplicado
- qué casos estáis marcando
- qué todavía no sabeo resolver con una regla simple

Esta práctica es mucho mejor que un ejercicio abstracto porque conecta directamente con el tipo de lógica que aparece después a BiciTierra Market, P3 y P5.

## Mini práctica

Pregunta:

qué estructura encaja mejor para representar un ticket con campos como `id`, `estat` y `canal`?

## Solución orientativa

Un diccionario, porque queremos acceder por nombre de campo y la forma se asemeja mucho a un registro JSON.

## Resumen final

La base de `Python` no es un trámite. Es la capa que permite que el resto del curso sea realmente operativa.

Si esta base es débil, cualquier librería más avanzada pareixerà magia o bloqueo. Si esta base es suficiente, el resto del curso se vuelve mucho más explicable y manipulable.

## Cómo usarla en estos proyectos

- BiciTierra Market: antes de entrar en `Pandas`, limpieza y funciones simples de transformación
- P3: scripts pequeños para simular sensores o validar payloads
- P5: utilidades, pruebas y servicios antes de la integración grande

## Ved también

- `Python per a Dades I`
- `Jupyter Notebooks`
- `Scikit-Learn Introductori`


## Para profundizar

[Obri la píndola ampliada, amb pràctiques i exercicis](../PINDOLES_AMPLIADES/docs/python-i-entorns/index.md).
