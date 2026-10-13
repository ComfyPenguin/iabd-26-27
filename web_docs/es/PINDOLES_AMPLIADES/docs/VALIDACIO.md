# Registro de validación

**Fecha:** 23 de septiembre de 2026. **Alcance:** once unidades ampliadas, once demostraciones Python, treinta y tres prácticas guiadas y treinta y tres retos con orientación.

## Comprobaciones ejecutadas

Se han ejecutado los once programas sobre datos sintéticos en un entorno separado. Todos han acabado correctamente. El notebook se ha ejecutado desde un núcleo nuevo; la primera ejecución se bloqueó por la restricción de conexiones locales de la en torno a comprobación y la repetición autorizada ha pasado.

| Unidad | Resultado comprobado |
|---|---|
| Python | 3 lecturas válidas, 4 rechazadas; cero aceptado |
| Jupyter | Notebook creado y ejecutado desde cero; media 120 litros/día |
| NumPy y pandas | 5 filas después de la unión; 1 servicio sin correspondencia y 1 duración ausente |
| Calidad | 7 filas auditadas; 3 lecturas válidas sin duplicación |
| Visualización | PNG y SVG generados; 180 observaciones repartidas entre 3 edificios |
| scikit-learn | Comparación con dummy, selección por validación y evaluación en prueba reservada |
| Validación | Dos cortes con fechas disjuntas; 24 filas de prueba en 2026 |
| Clustering | Comparación de k, estabilidad entre entonces, DBSCAN y proyección PCA |
| CORDERO | Additivitat SHAP verificada; explicación LIME generada con fidelidad local baja, comentada en el texto |
| Power BI | CSV y controles Python: 4 incidencias, 200 minutos, media 50 y resolución 75% |
| MLflow | Dos runs acabados, métricas y modelos registrados en almacenamiento local |

El código mostrado en cada unidad coincide con el programa descarregable. La reformulación de formato y comentarios se ha comprobado preservando el mismo árbol de sintaxis. También han pasado la ejecución de los 19 fragmentos Python breves de la teoría, la comprobación de los 69 enlaces locales y la construcción de la presentación con MkDocs en modo estricto. La figura del laboratorio de visualización se ha inspeccionado visualmente.

## Límites de la comprobación

- Power BI Desktop no se ha ejecutado en este entorno: la importación, las medidas DAX y las interacciones visuales se tienen que contrastar con los controles durante la práctica. No se incluye ningún PBIX.
- Se ha comprobado la ejecución automática del notebook, no su interfaz gráfica en todos los sistemas operativos.
- Las variantes propuestas en prácticas y retos son trabajo del alumnado; no se han ejecutado todas las posibles soluciones.
- Los resultados con datos sintéticos no son evidencia de rendimiento sobre datos reales. La prueba temporal es mensual sucesiva y no una previsión conjunta de todos los meses.
- Las versiones fijadas describen el entorno comprobado; no se afirma que sean las últimas disponibles ni que cualquier combinación futura sea compatible.

## En torno ejecutado

Python 3.10.12.

| Paquete | Versión |
|---|---|
| numpy | 2.2.6 |
| pandas | 2.3.3 |
| scikit-learn | 1.7.2 |
| matplotlib | 3.10.9 |
| seaborn | 0.13.2 |
| shap | 0.49.1 |
| lime | 0.2.0.1 |
| mlflow | 3.16.1 |
| nbformat | 5.11.1 |
| nbclient | 0.11.0 |
| ipykernel | 7.3.0 |

La presentación usa MkDocs Material 9.7.7. La instalación opcional de la interfaz Notebook 7 no forma parte de las once ejecuciones comprobadas.
