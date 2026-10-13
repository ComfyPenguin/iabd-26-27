---
title: "Python y entornos de trabajo"
description: "Cimientos, ejemplos reproducibles y práctica progresiva: python y entornos de trabajo."
tags: [IABD, datos, formación]
icon: material/book-open-page-variant
status: ampliada
---

# 01 · Python y entornos de trabajo

[← Índex de les píndoles](../index.md)

!!! abstract "Qué aprenderás"
    - Distinguir intérprete, entorno y editor.
    - Escribir funciones con entradas, salidas y errores explícitos.
    - Leer y guardar resultados con rutas reproducibles.

**Conocimientos previos:** Ningún conocimiento previo de Python; saber crear carpetas y abrir una terminal.

**Dedicación orientativa:** 2–3 h para lectura y laboratorio guiado; las extensiones y los ejercicios autónomos requieren tiempo adicional.

!!! tip "Dos recorridos posibles"
    **Esencial:** lee los cimientos, ejecuta el laboratorio y completa la práctica inicial. **Profundización:** continúa con las prácticas intermedia y avance y los ejercicios. El profesorado puede seleccionarlas según los conocimientos previos; no son todas obligatorias dentro del proyecto.

## Mapa de la unidad

Qué significa programar con Python → Variables, tipos y estructuras → Decisiones, repetición y funciones → En torno virtual, dependencias e intérprete → Ficheros, rutas y errores → Leer el programa de laboratorio

## 1. Qué significa programar con Python

Un programa transforma unas entradas en unos resultados mediante instrucciones. Por ejemplo, un sistema de sensores recibe lecturas textuales, las valida, calcula un resumen y guarda un informe. El lenguaje permite expresar el procedimiento; el intérprete de Python ejecuta las instrucciones; el sistema operativo proporciona ficheros, memoria y otros recursos.

El fichero `.py` es texto. No contiene las bibliotecas instaladas ni los datos externos. Para compartir un programa hay que explicar también como se ejecuta y de que depende. Una aplicación que funciona solo en el terminal de quien la ha escrito todavía no es reproducible.

Python es de tipatge dinámico: el tipo pertenece al objeto con que trabajamos. Esto facilita la experimentación, pero no elimina la necesidad de distinguir números, texto, colecciones y ausencias. El texto `"12"` no se comporta como el número `12`.

```python title="Text, nombre i absència"
edat_text = "24"
edat = int(edat_text)
print(edat + 1)                 # 25
print(edat_text + "1")         # 241
lectura = None                 # Encara no hi ha cap lectura.
print(lectura is None)          # True
```

## 2. Variables, tipos y estructuras

Un nombre identifica un objeto. Asignar un nombre nuevo a una lista no  crea necesariamente una copia: dos nombres pueden referirse a la misma colección. Esta distinción es importante cuando una función modifica los datos que recibe.

| Estructura | Para que resulta útil | Precaución |
|---|---|---|
| `int`, `float` | Conteos y magnitudes | Un decimal binario puede no representar exactamente una cantidad monetaria |
| `str` | Identificadores, mensajes y datos de texto | Un código postal se tiene que conservar como texto |
| `bool` | Decisiones con dos valores | `False` no equivale conceptualmente a dato ausente |
| `list` | Secuencia ordenada de elementos | Es mutable y puede contener tipos heterogéneos |
| `tuple` | Agrupación de valores que no se reasignan por posición | La immutabilitat no transforma los objetos interiores |
| `dict` | Registro con campos identificados por una clave | Hay que decidir qué pasa si una clave no existe |
| `set` | Valores únicos y pertenencia | No se tiene que basar una secuencia de trabajo en su orden |

```python title="Referència compartida i còpia"
original = [10, 20]
alias = original
alias.append(30)
print(original)                # [10, 20, 30]
copia = original.copy()
copia.append(40)
print(original)                # Continua sense el 40.
```

La copia anterior es superficial. Si la lista contiene otras listas, estas pueden continuar compartidas. Antes de recurrir a una copia profunda, conviene preguntar si el programa necesita modificar las estructuras o podría construir un resultado nuevo.

## 3. Decisiones, repetición y funciones

Una condición representa una regla del problema. En una sala frigorífica, «valor inferior a cero» puede ser correcto; en un recuento de unidades, una cantidad negativa necesita una explicación. No hay una validación universal desconectada del significado del dato.

```python title="Una regla explícita i una funció reutilitzable"
def estat_estoc(unitats, minim):
    if unitats < 0:
        raise ValueError("L'estoc no pot ser negatiu en este model")
    if unitats < minim:
        return "reposar"
    return "suficient"

for unitats in [0, 4, 12]:
    print(unitats, estat_estoc(unitats, minim=5))
```

Una función permite dar nombre a una operación, delimitar entradas y salidas y verificar casos concretos. Una función que devuelve un resultado es más fácil de reutilizar que una que solo lo imprime. `print` es útil para comunicar; `return` permite continuar calculando.

Un bucle es apropiado para recorrer registros, aplicar una regla o coordinar pasos. Cuando llegamos a arrays grandes, estudiaremos operaciones vectoriales. No hay que eliminar los bucles por principio: primero se tiene que entender la corrección y, si hace falta, mesurar el rendimiento.

## 4. En torno virtual, dependencias e intérprete

Un entorno virtual separa las bibliotecas de un proyecto de las otros proyectos. No es una máquina virtual ni un contenedor: utiliza el sistema y un intérprete base, con un espacio propio de paquetes. Activarlo modifica la manera de resolver órdenes en el terminal actual; también podemos invocar directamente el intérprete.

```bash title="Preparar un entorn a Linux o macOS"
python3 -m venv .venv
source .venv/bin/activate
python -c "import sys; print(sys.executable)"
python -m pip --version
```

En Windows se puede crear con `py -m venv .venv` e invocar `.venv\Scripts\python.exe` directamente. Si PowerShell impide la activación, no hay que alterar políticas globales para seguir la práctica: usa el camino del intérprete.

`python -m pip` asocia la instalación al Python seleccionado. El nombre de la orden `pip`, sin más información, puede corresponder a otro entorno. Cuando un importe falla, comprueba primero esta correspondencia, no instalas paquetes repetidamente en ciegas.

Un fichero de requisitos comunica dependencias; las versiones fijadas ayudan a repetir una ejecución, pero también importan la versión de Python, el sistema y, a veces, bibliotecas nativas. `pip freeze` describe los paquetes instalados: no es por sí mismo una explicación de cuáles son necesarios.

## 5. Ficheros, rutas y errores

Una ruta relativa se interpreta desde el directorio de trabajo. En un script, `Path(__file__).resolve().parent` localiza la carpeta del programa; en un notebook `__file__` no suele existir y hay que establecer la base explícitamente. Confundir las dos situaciones explica muchos «fichero no encontrado».

```python title="Escriure text amb codificació explícita"
from pathlib import Path

base = Path.cwd() / "resultats"
base.mkdir(exist_ok=True)
(base / "nota.txt").write_text("Lectures revisades.\n", encoding="utf-8")
print((base / "nota.txt").read_text(encoding="utf-8"))
```

Lee el rastro del error desde el tipo y mensaje final, y localiza después la línea de tu programa. `ValueError` suele indicar un valor inadecuado; `TypeError`, una operación con tipos incompatibles; `KeyError`, una clave ausente. La respuesta no es rodear todo el programa con `except: pass`, porque ocultaría el fallo.

Captura solo el error que puedes gestionar y conserva información suficiente para revisar el registro. En una importación de datos, separar filas válidas y rechazadas puede ser mejor que parar todo el proceso o sustituir todos los errores por cero.

## 6. Leer el programa de laboratorio

El ejemplo descarregable valida temperaturas de un dispositivo ficticio. Tiene cuatro decisiones deliberadas: el cero es válido, el texto vacío es ausente, el texto no numérico es un error de formato y una temperatura fuera de rango se rechaza sin «arreglarla» silenciosamente.

La función devuelve una pareja `(valor, error)`. El recorrido principal conserva el texto original, calcula la media solo con lecturas válidas y exporta la auditoría. Esto separa regla, coordinación y persistencia. La misma estructura se puede aplicar a lecturas industriales, formularios o importes de un fichero administrativo.

Observa también las comprobaciones `assert`: ejemplifican invariantes en un laboratorio. Para validar entradas externas en una aplicación, utiliza excepciones o comprobaciones explícitas; Python puede desactivar los asertos en modo optimizado.

## Laboratorio completo y reproducible

**Contexto:** trabajaremos con datos sintéticos creados por el mismo programa. No hay que descargar datasets ni usar los CSV de BiciTierra. Los valores sirven para aprender y comprobar procedimientos; no describen una población real.

**Materiales:** Python 3.10 o superior, un entorno virtual y los [requisits de la unitat](requirements.txt). Consulta la [preparació comuna](../index.md) antes de instalarlos.

Desde la carpeta de esta píldora, con el entorno activado:

```bash
python -m pip install -r requirements.txt
python codi/exemple.py
```

Esta unidad usa solo la biblioteca estándar: el fichero de requisitos está vacío y no hay que instalar paquetes.

El programa crea `codi/eixides/`. Puedes examinar las salidas sin modificar el código original. Para resolver las variantes, trabaja sobre una copia y actualiza las comprobaciones cuando cambias deliberadamente los datos.

### Ejemplo comentado

[Obri o descarrega el programa complet](codi/exemple.py){ download="exemple.py" }. Cada comprobación `assert` expresa una propiedad esperada de los datos de demostración; si falla, investiga la causa antes de eliminarla.

```python title="codi/exemple.py" linenums="1"
"""Exemple autònom: validar lectures i escriure un informe. Python 3.10+."""
from pathlib import Path
import csv, json, sys

def valida_lectura(text):
    """Retorna (valor, error); zero és una lectura vàlida, no una absència."""
    if text is None or str(text).strip() == '':
        return (None, 'absent')
    try:
        valor = float(text)
    except (ValueError, TypeError):
        return (None, 'format')
    if not -40 <= valor <= 80:
        return (None, 'rang')
    return (valor, None)

def main():
    # Entrades deliberadament heterogènies per comprovar el contracte.
    lectures = ['21.5', '', 'error', '0', '95', '18.0', 'nan']
    resultat = []
    for (posicio, lectura) in enumerate(lectures, start=1):
        (valor, error) = valida_lectura(lectura)
        resultat.append({'id': posicio, 'original': lectura, 'valor': valor, 'error': error})
    # Només les lectures acceptades entren en el resum.
    bons = [r['valor'] for r in resultat if r['error'] is None]
    resum = {'valides': len(bons), 'rebutjades': len(resultat) - len(bons), 'mitjana': sum(bons) / len(bons)}
    desti = Path(__file__).resolve().parent / 'eixides'
    desti.mkdir(exist_ok=True)
    # CSV conserva l’original i el motiu de rebuig per poder auditar.
    with (desti / 'lectures.csv').open('w', encoding='utf-8', newline='') as f:
        w = csv.DictWriter(f, fieldnames=list(resultat[0]))
        w.writeheader()
        w.writerows(resultat)
    (desti / 'resum.json').write_text(json.dumps(resum, indent=2), encoding='utf-8')
    assert valida_lectura('0') == (0.0, None)
    assert resum['valides'] == 3 and resum['rebutjades'] == 4
    print('Intèrpret:', sys.executable)
    print(resum)
if __name__ == '__main__':
    main()
```

## Prácticas guiadas

Cada práctica deja una evidencia pequeña: una salida comprobada y una explicación de la decisión. Los tiempos son orientativos y no incluyen instalación.

### Práctica 1 · Identificar el entorno

**Nivel y tiempo:** Inicial · 20–30 min.

**Objetivo:** Comprobar qué Python ejecuta el trabajo antes de instalar dependencias.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Crea una carpeta de laboratorio y un entorno virtual siguiendo las instrucciones del índice general. Anota la versión de Python.
2. Activa el entorno y ejecuta `python -c "import sys; print(sys.executable)"`. Guarda la ruta en una nota.
3. Ejecuta el ejemplo desde la carpeta de la píldora y compara la ruta impresa con el anterior.
4. Abre `codi/eixides/resum.json` y localiza recuento y media. Relaciona cada campo con la línea que lo calcula.

!!! success "Comprobación y evidencia esperada"
    La ruta corresponde al entorno activado; hay 3 lecturas válidas, 4 rechazadas y una media aproximada de 13,17.

**Preguntas de reflexión:**

- Qué diferencia hay entre instalar Python y crear un entorno?
- Por qué un paquete puede estar disponible en la terminal y no en el editor?
- Qué información mínima permitiría a una compañera reproducir el resultado?

**Ampliación opcional:** Ejecuta el programa con una ruta absoluta desde otra carpeta y comprueba donde se guardan las salidas.

### Práctica 2 · Validar sin perder información

**Nivel y tiempo:** Intermedia · 30–45 min.

**Objetivo:** Distinguir valor cero, ausencia, formato incorrecto y valor fuera del rango acordado.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Fez una copia de trabajo del programa y añade las entradas `" -5 "`, `None`, `"inf"` y `"80"`.
2. Antes de ejecutar, escribe el resultado esperado de cada entrada según el contrato de la función.
3. Invoca `valida_lectura` individualmente con cada caso y compara la pareja valor/error.
4. Actualiza las comprobaciones del resumen de la copia porque reflejen las nuevas entradas. Conserva la columna original en el CSV.

!!! success "Comprobación y evidencia esperada"
    −5 y 80 son válidos; None es ausente e infinito queda fuera de rango. La función no confunde cero con ausencia.

**Preguntas de reflexión:**

- Por qué `if valor` sería inadecuado para detectar una lectura válida?
- Qué diferencia hay entre la entrada original y el valor convertido?
- Qué pasaría si capturamos cualquier excepción sin informar?

**Ampliación opcional:** Añade una regla que acepto coma decimal y documenta qué formatos podrían resultar ambiguos.

### Práctica 3 · Un resumen robusto

**Nivel y tiempo:** Avance · 45–60 min.

**Objetivo:** Adaptar el cálculo a un conjunto sin lecturas aceptadas.

**Materiales:** ejemplo comentado de esta unidad, sus salidas y una copia de trabajo para las modificaciones.

**Procedimiento:**

1. Crea una lista formada solo por entradas inválidas en una copia del programa.
2. Ejecútala e identifica por qué el cálculo de la mediana falla antes de llegar a las comprobaciones.
3. Define el comportamiento esperado: media ausente y un mensaje claro cuando no hay lecturas válidas.
4. Modifica solo el cálculo necesario, adapta los asertos y comprueba tanto el caso vacío como el conjunto original.

!!! success "Comprobación y evidencia esperada"
    El programa no divide entre cero; el JSON exprés ausencia con `null` y conserva los recuentos.

**Preguntas de reflexión:**

- Por qué una media cero sería engañosa en este caso?
- Qué responsabilidad corresponde a la función de validación y qué al resumen?
- Qué casos límite añadirías antes de reutilizar el programa?

**Ampliación opcional:** Separa validación, resumen y escritura en tres funciones y describe las entradas y salidas de cada una.

## Ejercicios autónomos

Intenta resolver cada reto antes de desplegar la orientación. Se valora el razonamiento y las comprobaciones, no solo obtener una cifra.

### Reto 1 · Un nuevo sensor

Adapta la validación a porcentajes de humedad entre 0 y 100. Prueba extremos, ausencias y un texto inválido.

??? example "Solución orientativa y criterio de revisión"
    Cambia el contrato y el rango, no solo el nombre de la variable. 0 y 100 tienen que ser válidos; −1 y 101 no. Conserva el motivo de rechazo.

### Reto 2 · Función sin efectos laterales

Escribe una función que reciba una lista de valores válidos y devuelvo recuento y media sin escribir ficheros.

??? example "Solución orientativa y criterio de revisión"
    La función devuelve un diccionario; la capa exterior decide como guardarlo. Con lista vacía, usa una media ausente. Esto permite comprobar el cálculo independientemente del disco.

### Reto 3 · Informe de calidad

Genera recuentos por tipos de error y comprueba que aceptados más rechazados coincide con entradas.

??? example "Solución orientativa y criterio de revisión"
    Agrupa por el campo error, manteniendo una categoría para aceptados. La suma de los grupos tiene que coincidir con la longitud original si cada entrada tiene un único resultado de validación.

## Errores habituales y diagnóstico

| Síntoma | Causa que conviene investigar | Comprobación o corrección |
|---|---|---|
| Paquete no encontrado | Se está usando otro intérprete. | Comprueba sys.ejecutable e instala con python -m pip dentro del entorno. |
| Fichero en una carpeta inesperada | Ruta relativa dependiente de la terminal. | Construye la ruta desde __hilo__ o una base explícita. |
| División entre cero | No hay lecturas aceptadas. | Define el resultado vacío antes de calcular la media. |

## Autoevaluación y evidencias

Antes de dar la unidad por trabajada, comprueba estos puntos y escribe una frase de evidencia para cada uno:

- [ ] Puedo distinguir intérprete, entorno y editor.
- [ ] Puedo escribir funciones con entradas, salidas y errores explícitos.
- [ ] Puedo leer y guardar resultados con rutas reproducibles.
- [ ] He ejecutado el laboratorio y he contrastado al menos un resultado independientemente.
- [ ] Puedo explicar una limitación y un caso en que el procedimiento requeriría cambios.

**Entregable de la píldora:** copia de trabajo reproducible, resultados de la práctica seleccionada y un texto breve que indico pregunta, decisión, comprobación y limitación. Si se usa dentro de BiciTierra Market, integra esta evidencia en el entregable semanal correspondiente; no hay que crear una entrega duplicada.

## Fuentes para profundizar

- [Documentació oficial de referència](https://docs.python.org/3/tutorial/venv.html). Consulta especialmente los conceptos y los supuestos descritos en la unidad.

Las versiones ejecutadas y los límites de la comprobación están en el [registre de validació](../VALIDACIO.md). Los ejemplos son originales y los datos son sintéticos.

## Aplicación final a BiciTierra Market

**Momento orientativo:** semana 1. Esta correspondencia ayuda a elegir materiales y no sustituye el documento de trabajo del alumnado.

Prepara el en torno a trabajo y comprueba las rutas antes de abrir los CSV de BiciTierra. Reutiliza la distinción entre dato ausente y cero en ventas. No copias el rango de temperaturas: cada columna del proyecto necesita su propio contrato.

**Transferencia:** identifica qué concepto acabas de practicar, qué dato del proyecto lo exige y que tienes que cambiar respecto del laboratorio. Justifica esa adaptación antes de copiar código.
