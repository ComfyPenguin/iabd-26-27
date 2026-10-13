# Glosario de términos técnicos

Glosario de consulta rápida con el vocabulario técnico que aparece a lo largo del curso de especialización en IA y Big Fecha. Cada entrada incluye el ámbito al cual pertenece entre corchetes.

**Ámbitos:** [IA] Inteligencia Artificial *· [ML] Machine Learning *· [Datos] Ciencia de datos *· [BBDD] Bases de datos *· [Big Fecha] *· [Streaming] *· [Yate] *· [Observabilitat] *· [DevOps] Desarrollo y operación *· [Visualización] *· [Desarrollo] *· [Seguridad] *· [Contexto] Expresiones propias del contexto de negocio en que trabajan los proyectos.

---

## A

- ***Accuracy (exactitud)** [ML] — Métrica que medida cuántos aciertos hay en relación con el total de predicciones. Hay que mirarla con cautela cuando las clases del dataset están desequilibradas.

- **Agente (de IA)** [IA] — Sistema que percibe el entorno, decide y actúa para lograr un objetivo. Los agentes simples se integran con frecuencia dentro de una aplicación.

- **Agregación** [BBDD] — Operación que resume un grupo de registros o documentos (suma, media, conteo). Más allá de SQL, es típica de MongoDB e indispensable a los dashboards (Proyecto P2).

- **Análisis de sentimientos** [IA] — Tarea de PLN que identifica y clasifica opiniones o emociones expresadas en texto (positivo, negativo, neutral), útil para monitorizar opiniones de clientes o redes sociales.

- **APIO** *(Application Programming Interface)* [Desarrollo] — Interfaz que permite que dos sistemas se comunican e intercambian datos de manera estandarizada (por ejemplo, vía RISTRA o GraphQL).

- **Aprendizaje no supervisado** [ML] — Tipo de ML que trabaja sin hashtags para descubrir estructuras ocultas en los datos, como por ejemplo grupos de clientes (clustering).

- **Aprendizaje por refuerzo** *(reinforcement learning)* [ML] — Tipo de ML en que un agente aprende a tomar decisiones mediante pruebas y errores, recibiendo recompensas o sanciones por sus acciones (entorno, política, recompensa).

- **Aprendizaje supervisado** [ML] — Tipo de ML en que el modelo aprende a partir de datos etiquetados, para hacer clasificaciones o predicciones de valores numéricos.

- **Árbol de decisión** [ML] — Modelo que divide los datos en ramas mediante decisiones sucesivas. Es la base de técnicas más potentes como Random Monte.

- **Arquitectura** [Desarrollo] — Diseño de alto nivel del sistema: qué piezas lo componen (datos, modelo, aplicación, salida), como se conectan y por qué se han elegido estas tecnologías.

- **Automatización** [DevOps] — Conversión de tareas manuales en procesos automáticos, por ejemplo mediante workflows.

- ***Avro** [Big Fecha] — Sistema de serialización de datos de código abierto que utiliza esquemas JSON y datos binarios compactos, diseñado para Big Fecha y flujos de streaming (compatible con Kafka y Schema Registry).

## B

- ***Baseline** [ML] — Modelo o solución de referencia, simple y barato de construir, contra el cual se comparan las soluciones más avanzadas. Sin baseline no se puede justificar que un modelo "funciona".

- **Base de conocimiento documental** [IA] — Repositorio de documentos preparados y organizados que un sistema utiliza para recuperar información relevante. Tiene que estar limpia y localizable, no ser una carpeta caótica.

- ***Big Fecha** [Big Fecha] — Tratamiento de volúmenes de datos tan grandes o rápidos que hacen falta infraestructuras distribuidas (Spark, Cassandra, Kafka) en lugar de aproximaciones tradicionales.

- **Sesgo** *(bias)* [IA] — Desviación sistemática en los datos o en el modelo que produce resultados sesgados o discriminatorios. Es un riesgo ético que hay que identificar y mitigar.

- ***BI** *(Business Intelligence)* [Visualización] — Conjunto de herramientas y procesos para transformar datos en información útil para la toma de decisiones empresariales.

- ***Bottleneck (cuello de botella)** [Contexto] — Punto de congestión de un proceso que reduce la capacidad global del sistema. En el proyecto logístico se quiere predecir en las franjas de máxima actividad.

## C

- **Caja negra** *(black box)* [IA] — Sistema lógico interno del cual no se puede inspeccionar, y que solo se evalúa por sus entradas y salidas. Los modelos de deep learning tienden a serlo.

- ***Cassandra** [Big Fecha] — Base de datos NoSQL distribuida, diseñada para grandes volúmenes de datos con alta disponibilidad y particionat horizontal.

- ***Checksum** [Datos] — Valor calculado a partir de un fichero o conjunto de datos que permite verificar que no se han corrompido ni alterado.

- ***Chunk / Chunking** [IA] — División de documentos en fragmentos más pequeños para poderlos indexar y procesar, muy típica en flujos RAG.

- **Clasificación** [ML] — Tarea de aprendizaje supervisado que asigna a cada dato una categoría o hashtag entre un conjunto predefinido. Al Proyecto P1 se aplica a la clasificación de tickets.

- ***Cloud (nube)** [DevOps] — Modelo de despliegue en que los servicios se ejecutan en infraestructura remota y externalizada en lugar de equipos locales.

- ***Clustering** [ML] — Técnica de aprendizaje no supervisado que agrupa datos parecidos en clústeres, sin hashtags previos. La base de la segmentación de clientes.

- **CNN** *(Convolutional Neural Network)* [IA] — Red neuronal convolucional, arquitectura de deep learning especializada a procesar imágenes. Es la base de modelos como YOLO.

- **Codificación** *(encoding)* [Datos] — Transformación de variables categóricas en valores numéricos porque el modelo las pueda procesar.

- **Consistencia (de datos)** [BBDD] — Propiedad que garantiza que los datos permanecen correctos y coherentes en sistemas distribuidos y replicados.

- ***Consumer (consumidor)** [Streaming] — Aplicación que se conecta a un sistema de streaming (Kafka) y procesa los mensajes publicados en los topics.

- **Contenedor** [DevOps] — Unidad ligera que empaqueta una aplicación con todas sus dependencias porque se ejecute de manera aislada e igual en cualquier entorno.

- **Contrato de datos** *(fecha contract)* [Big Fecha] — Acuerdo formal sobre la estructura y la calidad de los datos entre quienes las produce y quienes las consume. Garantiza la evolución controlada de los formatos.

- **Corpus** [IA] — Conjunto de documentos o textos que alimentan un sistema de clasificación o un flujo RAG. Tiene que ser propio del proyecto, no un chatbot genérico.

- **Correlación** [Datos] — Relación estadística entre dos variables: hasta qué punto una varía de manera asociada a la otra.

- **Cross-validation (validación cruzada)** [ML] — Técnica que divide los datos en varias partes y alterna entrenamiento y evaluación para estimar mejor la capacidad de generalización del modelo.

- **Cola de camiones** *(truck queue)* [Contexto] — Indicador operativo de saturación de un terminal: el volumen de camiones en espera. Si supera el umbral operativo, se genera el alerta.

## D

- **Datos documentales** [BBDD] — Datos almacenados en formato de documento (por ejemplo, en MongoDB), en lugar de mesas relacionales rígidas.

- **Datos en tiempo real** [Big Fecha] — Datos que se generan y procesan de manera (casi) inmediata, como los acontecimientos de sensores o terminales.

- **Datos históricos** [Datos] — Registros recogidos en el pasado, utilizados como base para la construcción y validación de modelos predictivos.

- **Datos tabulares** [Datos] — Datos organizados en tablas de filas y columnas, el formato estándar de trabajo con Pandas y Power BI.

- ***Dashboard** [Visualización] — Panel visual que muestra datos, KPIs y alertas consolidadas para ayudar a tomar decisiones. No tiene que mostrar todo: solo el que ayuda a decidir mejor.

- ***Dashboard ejecutivo** [Visualización] — Panel diseñado para la dirección, pensado para la lectura rápida de indicadores estratégicos.

- **Fecha mining** *(excavación de datos)* [Datos] — Proceso de explorar y analizar grandes volúmenes de datos para descubrir patrones, correlaciones y conocimientos útiles que no son obvios a primera vista.

- ***Databricks Lakehouse** [Big Fecha] — Plataforma unificada que combina las capacidades de fecha warehouse y fecha lake sobre una arquitectura abierta, facilitando el análisis y el machine learning sobre grandes volúmenes.

- ***Dataset (conjunto de datos)** [Datos] — Colección estructurada de datos utilizada para analizar, entrenar o validar modelos.

- ***DBSCAN** [ML] — Algoritmo de clustering basado en densidad que detecta grupos de forma irregular y, a la vegada, identifica puntos anómalos (outliers).

- **Decisión de negocio** [Contexto] — Decisión estratégica empresarial (stock, campaña, segmento de clientes) que se apoya en los resultados de los datos y de los modelos.

- ***Deep Learning (DL)** [IA] — Subcamp del machine learning basado en redes neuronales profundas, capaz de trabajar con datos complejos como imágenes, audio o también texto.

- **Delta Lake** [Big Fecha] — Capa de almacenamiento abierta que aporta transacciones ACID, esquemas evolutivos y versionado a fecha lakes, haciéndolos más fiables para analítica y ML.

- **Despliegue (deploy)** [DevOps] — Proceso de poner en marcha un servicio o aplicación en un entorno operativo, local o en la nube.

- **Detección de objetos** [IA] — Tarea de visión por computador que localiza e identifica objetos dentro de una imagen, como hace YOLO.

- **Deuda técnica** [Desarrollo] — Costes de ejecución de decisiones rápidas (código poco estructurado, documentación ausente) que habrá que "pagar" con mejoras futuras.

- **Diagrama de dispersión** [Visualización] — Gráfico que muestra la relación entre dos variables numéricas mediante puntos.

- **Distribución** [Datos] — Forma en que se reparten los valores de una variable (frecuencias por valor), permitiendo detectar patrones y anomalías.

- ***Docker** [DevOps] — Plataforma de contenedores que permite construir, desplegar y ejecutar aplicaciones de manera aislada y portable.

- ***Docker Compongo** [DevOps] — Herramienta para definir y lanzar aplicaciones multe-contenedor a partir de un fichero de configuración (YAML).

## E

- ***EDA** *(Exploratory Fecha Analysis)* [Datos] — Análisis exploratorio de datos: inspección inicial para entender la estructura, patrones, outliers y calidad de un dataset antes de modelar.

- ***Edge AI** [IA] — IA ejecutada de forma local en el dispositivo de borde (edge), sin depender de un servidor central.

- ***ELK stack** *(Elasticsearch, Logstash, Kibana)* [Observabilitat] — Conjunto de herramientas para la ingesta, busca y visualización de logs y otros datos de operación.

- ***Embedding** [IA] — Representación vectorial de texto (u otros datos) que codifica su significado, de forma que valores o conceptos parecidos quedan cerca en el espacio vectorial.

- **Ingeniería de variables** *(feature engineering)* [ML] — Proceso de crear, transformar y seleccionar variables porque el modelo aprenda mejor.

- **Acontecimiento** *(event)* [Streaming] — Unidad mínima de información que fluye en tiempo real por un sistema de mensajería, como un cambio de estado de un sensor o terminal.

- **Escalado (de variables)** [Datos] — Normalización o estandarización de variables numéricas porque tengan rangos comparables dentro del modelo.

- **Esquema de datos** [BBDD] — Definición formal de cómo se organizan los datos: campos, tipos y relaciones.

## F

- **F1-score** [ML] — Media armónica entre precisión y recall; resume en un solo número el equilibrio entre los dos y es muy usada para valorar clasificadores.

- **Falso positivo / falso negativo** [ML] — Errores de clasificación: el modelo predice un positivo que no lo es, o no detecta un positivo que lo era. Los falsos positivos son especialmente problemáticos en entornos operativos.

- ***FastAPI** [Desarrollo] — Framework moderno de Python para crear Apios, incluidas las RISTRA.

- **Fino-tuning** *(ajuste fino)* [IA] — Proceso de tomar un modelo preentrenat y ajustarlo con datos específicos del dominio para mejorar el rendimiento en una tarea concreta.

- ***FIWARE / Orion** [Yate] — Plataforma abierta de gestión de contexto para Yate; Orion es el "contexto broker" que gestiona los datos de contexto y sus suscripciones en tiempo real.

- ***Flask** [Desarrollo] — Microframework ligero de Python para crear aplicaciones web y Apios.

## G

- **Generalización** [ML] — Capacidad de un modelo de funcionar bien con datos que no ha visto durante el entrenamiento. Es el objetivo real de cualquier modelo.

- ***Git** [Desarrollo] — Sistema de control de versiones que permite llevar el historial de cambios del código y trabajar en equipo de manera ordenada.

- **Gradiente Boosting** [ML] — Técnica de ensemble que construye modelos de manera secuencial, corrigiendo los errores de los modelos anteriores.

- ***Grafana** [Observabilitat] — Herramienta de visualización y dashboarding de métricas, habitualmente acompañada de Prometheus; genera paneles y alertas.

- ***GraphQL** [Desarrollo] — Lenguaje de consulta de Apios alternativo a RISTRA que permite al cliente pedir exactamente los datos que necesita.

- ***Gradio** [Desarrollo] — Herramienta para crear interfaces web interactivas para modelos de IA/ML de forma rápida.

- ***Guardrails** [IA] — Mecanismos de seguridad que limitan las respuestas de un sistema de IA porque no genero contenido inadecuado o fuera de contexto (por ejemplo, obligarlo a decir "no lo sé").

## H

- **Heurística** [IA] — Regla práctica o estrategia que encuentra soluciones basta buenas sin explorar todas las posibilidades (base de la busca en sistemas de IA).

## I

- ***IA (Inteligencia Artificial)** [IA] — Disciplina que persigue que las máquinas hagan tareas que requieren inteligencia: razonar, percibir, comprender lenguaje y aprender.

- **Iceberg** [Big Fecha] — Formado de mesas abierto y escalable diseñado para fecha lakes, que permite consultas analíticas eficientes, evolución de esquemas y versionado de datos.

- **Imputación** [Datos] — Técnica para llenar valores que faltan en un dataset con estimaciones razonables, en lugar de descartar los registros.

- **Inferencia** [IA] — Ejecución de un modelo ya entrenado sobre datos nuevos para obtener predicciones o detecciones (por ejemplo, pasar una imagen a YOLO).

- **Integración de servicios** [Desarrollo] — Conexión y coordinación de componentes o servicios porque funcionan como un sistema único.

- **Integración vertical** [Desarrollo] — Flujo que recorre todas las capas de la solución de punta a punta: desde el dato hasta la decisión o salida operativa. Cada proyecto tiene que mostrar como mínimo una.

- **Integridad de datos** [Datos] — Garantía que los datos son completos, correctos y no han sido alteradas de manera no autorizada.

- ***Interpretabilitat** [IA] — Capacidad de entender qué hace y por qué un modelo; un paso previo al explicabilitat.

- **Yate (Internet de las Cosas)** [Yate] — Red de dispositivos conectados (sensores, actuadores) que recogen y envían datos del entorno en tiempo real.

## K

- **K-Means** [ML] — Algoritmo de clustering que agrupa los datos en K grupos según la distancia al centróide del grupo.

- **Kafka** [Streaming] — Plataforma de mensajería distribuida para procesar flujos de acontecimientos en tiempo real. Los mensajes se organizan en topics y los gestionan producers y consumers.

- ***KNN** *(K-Nearest Neighbors)* [ML] — Algoritmo de clasificación que asigna a un dato la clase mayoritaria de sus K vecinos más próximos.

- ***KPI** *(Key Performance Indicator)* [Visualización] — Indicador clave de rendimiento: una métrica de negocio que ayuda a evaluar el logro de objetivos. Ejemplos: terminales en riesgo, cola mediana, alertas activas.

## L

- ***Lakehouse** [Big Fecha] — Arquitectura de datos que combina las mejores características de los fecha lakes (bajo coste y flexibilidad) y los data warehouses (fiabilidad y rendimiento analítico) en una sola plataforma.

- **Lectura ejecutiva / comercial** [Contexto] — Interpretación de los resultados técnicos en términos de valor para la empresa, pensada para quien toma decisiones sin formación técnica.

- ***LIME** [IA] — Método de explicabilitat que explica una predicción concreta de manera local, acercando el comportamiento del modelo con simplificaciones.

- **Línea temporal** [Visualización] — Gráfico de líneas que muestra la evolución de una variable a lo largo del tiempo.

- **Umbral (operativo)** [Observabilitat] — Valor límite que, al superarse, activa una alerta o protocolo de actuación (por ejemplo, una cola de camiones que supera la capacidad de un terminal).

- ***LLM** *(Large Language Modelo)* [IA] — Modelo de lenguaje grande: red neuronal entrenada con grandes cantidades de texto capaz de generar y comprender lenguaje natural.

- ***LLMOps** [IA] — Prácticas y herramientas para gestionar el ciclo de vida de los LLMs en producción: ajuste fino, evaluación, despliegue, monitorització y actualización.

- ***Log (registro)** [Observabilitat] — Acontecimiento registrado cronológicamente de actividad del sistema, esencial para la depuración, la auditoría y la trazabilidad.

## M

- ***Machine Learning (ML, aprendizaje automático)** [ML] — Disciplina en que los sistemas aprenden a partir de datos en lugar de ser programados con reglas explícitas.

- ***MapReduce** [Big Fecha] — Modelo de programación distribuida que procesa y genera grandes volúmenes de datos en paralelo (fase de "map" y fase de "reduce"); es la base de Hadoop.

- ***Matplotlib** [Desarrollo] — Librería de Python para la creación de gráficas y visualizaciones.

- **Matriz de confusión** [ML] — Tabla que muestra, para cada clase, cuántos aciertos y cuántos errores ha cometido un clasificador. Permite ver de qué tipo son los errores.

- **Métrica** [ML] — Valor numérico que evalúa el rendimiento de un modelo o sistema (accuracy, precisión, recall, F1...). Hay que elegir la métrica adecuada al objetivo.

- ***MLflow** [IA] — Plataforma para la gestión del ciclo de vida de modelos: tracking de experimentos, versiones y despliegue.

- ***MLOps** [IA] — Prácticas para llevar modelos de ML a producción de manera fiable y automatizable, como se hace con el código tradicional.

- **Modelo (predictivo)** [ML] — Resultado del entrenamiento: una función capaz de predecir valores o categorías a partir de nuevas entradas. Tiene que estar justificado y comparado con un baseline.

- **Modelado de datos** [BBDD] — Diseño de la estructura de los datos (documentos, mesas, particiones) porque las consultas sean eficientes.

- ***MongoDB** [BBDD] — Base de datos NoSQL orientada a documentos, muy usada para datos persistentes como tickets o registros de interacción.

- ***Monitorització** [Observabilitat] — Seguimiento continuo del estado del sistema (datos, servicios, infraestructura) para detectar anomalías y riesgos.

- ***MQTT** [Yate] — Protocolo de mensajería ligero pensado para la comunicación entre dispositivos Yate en redes de ancho de banda reducido.

- **MVP** *(Minimum Viable Product)* [Desarrollo] — Producto mínimo viable: versión reducida pero funcional que demuestra el valor central de la solución.

## N

- ***n8n** [DevOps] — Herramienta de automatización de flujos de trabajo (workflows) de código abierto, que orquesta servicios externos (correo, mensajería, OCR, IA).

- **Limpieza de datos** [Datos] — Eliminación o corrección de datos erróneos, incompletas o incoherentes antes de analizar.

- **Nodo-RED** [Yate] — Herramienta de programación visual basada en flujos que conecta dispositivos Yate, Apios y servicios a partir de nodos.

- ***NoSQL** [BBDD] — Familia de bases de datos no relacionales (documentos, columnas, clave-valor, grafos), pensadas para escalar horizontalmente.

- ***Notebook (de Jupyter)** [Desarrollo] — Documento que combina código ejecutable, texto y resultados visuales, ideal para exploración, modelado y prácticas guiadas.

- ***NumPy** [Desarrollo] — Librería de Python para el cálculo numérico con arrays y operaciones matriciales.

## O

- ***Observabilitat** [Observabilitat] — Capacidad de comprender el estado interno de un sistema a partir de sus salidas externas: logs, métricas y trazas.

- **Objetivos de dirección** [Contexto] — Hitos que la dirección de la empresa quiere lograr y que orientan el proyecto técnico (reducir colas, detectar sensores averiados, mejorar la satisfacción...).

- ***OpenCV (cv2)** [IA] — Librería de visión por computador para el procesamiento de imágenes (leer, convertir a escala de grises, redimensionar).

- **Orquestación** [DevOps] — Coordinación automatizada de múltiples servicios o contenedores porque funcionan conjuntamente.

- ***Outlier (valor atípico)** [Datos] — Punto aislado que se aparta del comportamiento general del resto de los datos; puede ser un error o un dato realmente singular.

- ***Overfitting** *(sobreajust)* [ML] — Situación en que un modelo aprende demasiado bien los datos de entrenamiento (incluyendo el ruido) y generaliza mal con datos nuevos. Se detecta cuando el error de entrenamiento es muy menor que el de prueba.

## P

- ***Pandas** [Desarrollo] — Librería de Python para la manipulación y análisis de datos tabulares (DataFrames), el caballo de trabajo de la ciencia de datos.

- **Parqué** [Big Fecha] — Formado de fichero de datos columnar optimizado para el almacenamiento y el análisis eficiente de grandes volúmenes de datos en entornos distribuidos.

- ***Particionat** [BBDD] — Estrategia de distribución de los datos entre nodos para obtener escalabilidad y disponibilidad, característica de Cassandra.

- ***Payload** [Yate] — Carga de información que un dispositivo (por ejemplo, un sensor) envía dentro de un mensaje.

- ***PCA** *(Principal Componente Analysis)* [ML] — Técnica de reducción de dimensionalidad que transforma variables correlacionadas en unos cuántos componentes principales, para simplificar y visualizar.

- ***Pipeline** [ML] — Cadena ordenada de pasos que transforma los datos desde la entrada hasta el resultado (limpieza, preprocessament, modelo, salida).

- ***PLN** *(Procesamiento de Lenguaje Natural, NLP)* [IA] — Camp de la IA que trabaja la interacción entre ordenadores y lenguaje humano: comprensión, generación y análisis de texto.

- ***Power BI** [Visualización] — Herramienta de Microsoft para crear dashboards e informes interactivos a partir de datos de la organización (Proyecto P2).

- **Precisión (precision)** [ML] — Métrica: de los elementos predichos como positivos, cuántos son realmente positivos. Baja precisión quiere decir muchos falsos positivos.

- ***Preprocessament** [Datos] — Preparación de los datos antes de modelar: limpieza, imputación, codificación y escalado.

- **Preguntas de negocio** [Contexto] — Problemas formulados en lenguaje empresarial que el sistema de datos tiene que responder (por ejemplo, "como afecta el descuento a las ventas?"). No son preguntas técnicas: guían el análisis y el diseño del modelo.

- ***Producer (productor)** [Streaming] — Aplicación que publica mensajes en los topics de un sistema de streaming como Kafka.

- ***Prometheus** [Observabilitat] — Sistema de monitorització y recogida de métricas que alimenta alertas y paneles, habitualmente visualizadas a Grafana.

- ***Prompt engineering** [IA] — Diseño y afinadura de las instrucciones (prompts) que se le dan a un LLM para obtener respuestas más útiles y fiables.

- ***PyTorch** [Desarrollo] — Framework de deep learning para construir y entrenar redes neuronales.

- ***Python** [Desarrollo] — Lenguaje de programación principal del curso, con un ecosistema masivo para datos y IA.

## Q

- **Calidad de datos** [Datos] — Grado de corrección, completitud y fiabilidad de un dataset. Hay que revisarla antes de modelar.

## R

- ***RAG** *(Retrieval-Augmented Generation)* [IA] — Técnica que genera respuestas con ayuda de un LLM pero aumentadas con documentos recuperados de una base de conocimiento. Primero recupera, después genera, y tiene que saber decir "no lo sé" si no hay contexto (Proyecto P1).

- ***Random Monte** [ML] — Ensemble de muchos árboles de decisión que combina sus predicciones para mejorar la estabilidad y la precisión.

- ***Recall (sensibilidad)** [ML] — Métrica: de los elementos realmente positivos, cuántos ha detectado el modelo. Bajo recall quiere decir que se pierden casos positivos.

- **Recomendaciones accionables** [Contexto] — Conclusiones prácticas y concretas que la dirección puede aplicar directamente, derivadas del análisis de datos.

- **Regresión** [ML] — Tipo de algoritmo de aprendizaje supervisado que predice un valor numérico continuo (ventas, minutos de cola, temperatura...).

- **Repositorio (repo)** [Desarrollo] — Espacio donde se guarda el código de un proyecto con su histórico de versiones (Git). 

- **RISTRA** [Desarrollo] — Estilo arquitectónico para Apios web basado en recursos y verbos HTTP (GET, POST, HEDE, DELETE).

- ***RGPD** [Seguridad] — Reglamento europeo de protección de datos personales; todo sistema que trato datos personales tiene que cumplirlo.

- ***ROC / AUC** [ML] — Curva ROC y área bajo la curva: métricas que valoran la capacidad del modelo para separar clases a lo largo de varios umbrales.

- ***RPA** *(Robotic Process Automation)* [IA] — Automatización de procesos basados en reglas mediante "robots" de software, a menudo contextualizada con IA.

## S

- **S3** *(Amazon Simple Storage Service)* [Big Fecha] — Servicio de almacenamiento de objetos de Amazon Web Services, ampliamente utilizado como base de los fecha lakes a la nube.

- ***scikit-learn** [Desarrollo] — Librería de Python de ML clásico: clasificación, regresión, clustering y preprocessament con una APIO coherente.

- ***Seaborn** [Desarrollo] — Librería de visualización estadística construida sobre Matplotlib, con gráficos más elaborados.

- **Segmentación (de clientes)** [ML] — Agrupación de clientes en segmentos con características comunes mediante técnicas de clustering. Los segmentos tienen que tener una lectura comercial, no ser solo hashtags.

- **Sensor** [Yate] — Dispositivo que captura datos del entorno (temperatura, humedad, estado de maquinaria) y las envía al sistema.

- **Serie temporal** [Datos] — Secuencia de datos ordenada cronológicamente. Se utiliza generalmente para predicción de evolución.

- ***Serverless** [DevOps] — Modelo de despliegue en que la nube gestiona el servidor; el código se ejecuta bajo demanda (functions as a service).

- ***SHAP** [IA] — Método de explicabilitat que descompone cada predicción y muestra la contribución de cada variable a partir de la teoría de juegos.

- ***Schema Registry** [Streaming] — Registro centralizado de esquemas que valida y controla la evolución de los formatos de datos en un sistema de streaming.

- **Sistemas expertos** [IA] — Sistemas basados en reglas y conocimiento explícito, contrapuestos a los LLMs dentro de la evolución de la IA.

- ***Snowflake** [Big Fecha] — Plataforma de fecha cloud que separa el almacenamiento y la computación, permitiendo escalarlos de manera independiente y simplificando la gestión de datos.

- **Ruido (en datos)** [Datos] — Datos no útiles, incorrectos o irrelevants que interfieren en el análisis o el modelo.

- ***Spark** [Big Fecha] — Motor de procesamiento distribuido para grandes volúmenes de datos y analítica sobre clusters.

- ***Streaming** [Streaming] — Procesamiento continuo de flujos de datos a medida que se generan, en lugar de por lotes.

- ***Streamlit** [Desarrollo] — Herramienta para crear aplicaciones web interactivas a partir de scripts de Python, muy usada para demos y paneles.

- ***SVM** *(Support Vector Machine)* [ML] — Algoritmo de clasificación que busca el hiperplà que separa mejor las clases.

## T

- ***Target (variable objetivo)** [ML] — La variable que el modelo quiere predecir; el resto de datos son las características o variables independientes.

- **Taxonomía** [IA] — Estructura ordenada de categorías y prioridades que un sistema de clasificación utiliza para etiquetar.

- ***Token** [IA] — Unidad mínima de texto con la cual trabajan los modelos de lenguaje (palabra, fragmento o carácter); el proceso de dividir el texto se llama tokenització.

- ***Topic (tema)** [Streaming] — Canal lógico de Kafka donde se agrupan y se almacenan los mensajes de un mismo asunto.

- **Trazabilidad** [Datos] — Capacidad de seguir y documentar el origen y el recorrido de cada dato o respuesta del sistema. Hay que poder justificar de donde sale cada resultado.

- ***Train/maceta (partición)** [ML] — División del dataset en un conjunto de entrenamiento y uno de prueba para entrenar y evaluar el modelo con datos no vistos.

- ***Transformers** [IA] — Arquitectura de redes neuronales sobre la cual se construyen los LLMs modernos.

- ***Tracking (de experimentos)** [IA] — Registro sistemático de parámetros, métricas y versiones de los experimentos de ML, por ejemplo con MLflow.

- ***Transfer learning** *(aprendizaje por transferencia)* [ML] — Técnica que reaprofita un modelo entrenado en una tarea como punto de partida para otra tarea similar, reduciendo el tiempo y los datos necesarios.

## Uno

- ***UI** *(User Interface)* [Desarrollo] — Interfaz gráfica con la cual interactúa el usuario final.

- ***Underfitting** *(infraajust)* [ML] — Situación en que un modelo es demasiado simple para capturar los patrones de los datos, y obtiene resultados malos tanto en entrenamiento como en prueba.

## V

- **Valor perdido** [Datos] — Dato ausente en un dataset; hay que decidir entre imputarla o descartar el registro.

- **Vector DB (Base de datos vectorial)** [BBDD] — Base de datos pensada para almacenar y buscar embeddings de forma eficiente; el apoyo funcional del RAG.

- **Vertical funcional** [Desarrollo] — Versión reducida pero completa del sistema que travesía todas las capas (dato → análisis → salido) y demuestra que el flujo principal funciona.

- **Visión por computador** [IA] — Camp de la IA que mujer a las máquinas la capacidad de interpretar imágenes y video.

- **Visualización de datos** [Visualización] — Representación gráfica de datos para facilitar la interpretación y la toma de decisiones.

## W

- **Web scraping** [Desarrollo] — Técnica para extraer automáticamente datos de páginas web, transformando el contenido HTML en datos estructurados para su análisis.

- ***Workflow** [DevOps] — Flujo de trabajo automatizado que orquesta pasos, condiciones y respuestas entre servicios.

## X

- **CORDERO** *(Explainable AI, IA explicable)* [IA] — Camp de IA que aporta técnicas para hacer comprensibles y justificables las decisiones de los modelos (SHAP, LIME, importancias).

- **Red neuronal (artificial)** [IA] — Modelo inspirado en el cerebro formado por capas de neuronas que aprende representaciones a partir de los datos; la base del deep learning.

- ***XGBoost / CatBoost / LightGBM** [ML] — Implementaciones optimizadas y escalables de la técnica Gradiente Boosting, muy utilizadas en competiciones y proyectos reales por su alta precisión.

## Y

- ***YOLO** *(You Only Look Once)* [IA] — Arquitectura de detección de objetos en tiempo real sobre imágenes y video. Se utiliza por inferencia (no entrenamiento) con la variante YOLOv8n, por ejemplo para detección visual en lo proyecte Yate/logística.

---

## Índice por ámbitos

| Ámbito | Termas |
|---|---|
| [IA] Inteligencia Artificial | Análisis de sentimientos, Agente, Embedding, LLM, LLMOps, RAG, PLN, Fino-tuning, Token, Tokenització, Prompt engineering, Guardrails, CORDERO, SHAP, LIME, Interpretabilitat, Caja negra, Sistemas expertos, Visión por computador, YOLO, Detección de objetos, Inferencia, CNN, Transformers, Deep Learning, Red neuronal, RPA, Edge AI, MLOps, MLflow, Tracking, Base de conocimiento documental, Corpus, Taxonomía, Chunk, Sesgo, Heurística |
| [ML] Machine Learning | Machine Learning, Aprendizaje supervisado, Aprendizaje no supervisado, Aprendizaje por refuerzo, Baseline, Clasificación, Regresión, Clustering, K-Means, DBSCAN, PCA, KNN, SVM, Árbol de decisión, Random Monte, Gradiente Boosting, Cross-validation, Train/maceta, Matriz de confusión, Accuracy, Precisión, Recall, F1-score, ROC/AUC, Falso positivo/falso negativo, Métrica, Generalización, Valor objetivo, Ingeniería de variables, Modelo predictivo, Segmentación de clientes, Pipeline, Overfitting, Underfitting, Transfer learning, XGBoost / CatBoost / LightGBM |
| [Datos] Ciencia de datos | EDA, Dataset, Calidad de datos, Limpieza de datos, Fecha mining, Preprocessament, Imputación, Codificación, Escalado, Outlier, Valor perdido, Serie temporal, Correlación, Distribución, Ruido, Datos históricos, Datos tabulares, Datos en tiempo real, Integridad de datos, Trazabilidad, Checksum |
| [BBDD] Bases de datos | MongoDB, Cassandra, NoSQL, Esquema de datos, Modelado de datos, Agregación, Particionat, Consistencia, Vector DB, Datos documentales |
| [Big Fecha] | Big Fecha, Spark, MapReduce, Avro, Parqué, Delta Lake, Iceberg, Lakehouse, Databricks Lakehouse, S3, Snowflake |
| [Streaming] | Kafka, Topic, Producer, Consumer, Schema Registry, Contrato de datos, Acontecimiento, Streaming |
| [Yate] | Yate, Sensor, MQTT, Nodo-RED, FIWARE/Orion, Payload |
| [Observabilitat] | Observabilitat, Monitorització, Log, Métrica, Alerta, Umbral, Prometheus, Grafana, ELK stack | 
| [DevOps] | Docker, Docker Compongo, Contenedor, Orquestación, n8n, Workflow, Cloud, Serverless, Automatización, Despliegue |
| [Visualización] | Dashboard, Dashboard ejecutivo, KPI, Power BI, Visualización de datos, BI, Línea temporal, Diagrama de dispersión |
| [Desarrollo] | Python, En torno virtual, NumPy, Pandas, Matplotlib, Seaborn, scikit-learn, PyTorch, OpenCV, Notebook, APIO, RISTRA, GraphQL, FastAPI, Flask, Streamlit, Gradio, UI, Git, Repositorio, Arquitectura, Integración de servicios, Integración vertical, Vertical funcional, MVP, Deuda técnica, Autenticación, Web scraping |
| [Seguridad] | RGPD, Autenticación |
| [Contexto] Expresiones de negocio | Preguntas de negocio, Objetivos de dirección, Recomendaciones accionables, Decisión de negocio, Lectura ejecutiva/comercial, Bottleneck (cuello de botella), Cola de camiones, Triaje |
