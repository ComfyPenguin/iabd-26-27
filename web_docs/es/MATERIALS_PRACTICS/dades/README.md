# Datos de BiciTierra Market

**Datos íntegramente sintéticos para docencia.** No representan personas, ventas, campañas ni precios observados de una empresa real. La fecha de corte del caso es el **30 de junio de 2026**, independientemente de la fecha real de la clase.

## Qué fichero tengo que abrir?

| Fichero | Contenido | Uso |
|---|---|---|
| [sales_history.csv](sales_history.csv){ download="sales_history.csv" } | 1.188 filas, 66 meses, enero 2021–junio 2026 | EDA y predicción mensual de unidades |
| [customers.csv](customers.csv){ download="customers.csv" } | 2.000 clientes, fotografía a 30/06/2026 | Segmentación de comportamientos |
| [sales_forecast_input.csv](sales_forecast_input.csv){ download="sales_forecast_input.csv" } | 18 combinaciones para julio de 2026, sin ventas ni ingresos | Aplicar el modelo a un mes todavía desconocido dentro del caso |

Empieza por los dos primeros. El tercero se usa después de validar el modelo. Los ficheros son UTF-8, separados por comas; los decimales usan punto y los nulos son celdas vacías.

## Qué representa cada fila?

**Ventas:** un mes × canal (`store`, `ecommerce`) × línea (`urban`, `mountain`, `electric`) × región (`coast`, `inland`, `metro`). Hay 18 series completas de 66 meses. La clave son estos cuatro campos; no se tiene que sumar más de una vez la misma fila.

**Clientes:** una persona ficticia identificada con una clave única, con compras de los últimos 12 meses. Es una muestra independiente para segmentación, no el libro de transacciones de las ventas agregadas. No hay una clave que permita unir clientes y ventas hila a fila. La suma de `annual_spend` no tiene que coincidir con `revenue`.

## Qué predicción hagamos?

**Objetivo principal:** predecir `sales_units` del mes siguiente antes de empezar ese mes, por cada canal, producto y región. Son ventas realizadas, limitadas por el stock; no son demanda total si hay ruptura de stock.

**Disponibles antes del mes:** `month`, `channel`, `product_line`, `region`, `marketing_budget`, `planned_discount`, `stock_units`, `list_price`, `campaign`, `days_in_month`. El stock es el disponible al inicio; en esta simulación no hay reposición durante el mes.

**Solo conocidas al cerrar el mes:** `marketing_spend`, `avg_discount`, `website_sessions`, `sales_units`, `revenue`, `closing_stock_units`, `stockout`. No usas estos campos del mismo mes como predictores anticipados. Sí que puedes calcular retrasos con datos de meses anteriores de la misma serie.

En particular, predecir `sales_units` con `revenue` sería usar un dato que incorpora directamente la respuesta. La relación contable es `revenue = sales_units × list_price × (1 − avg_discount)`, con redondeo a céntimos. El descuento real está oculto en algunas filas como ejercicio de calidad.

## Orden de trabajo y separación temporal

1. ***EDA:** comprueba nulos, claves, cobertura mensual y patrones por producto/canal/región.
2. **Entrenamiento:** meses hasta diciembre de 2024. Si usas `lag_12`, el primer año actúa como calentamiento y el entrenamiento efectivo empieza en enero de 2022.
3. **Validación:** enero–diciembre de 2025, para comparar modelos y elegir parámetros.
4. **Prueba final:** enero–junio de 2026, una sola evaluación después de fijar decisiones.
5. **Aplicación:** julio de 2026 con `sales_forecast_input.csv`; el resultado real no está incluido.

Separa meses completos: todas las series de un mes tienen que ir al mismo periodo. Evita una partición aleatoria de filas. Ajusta imputación y transformaciones solo con entrenamiento. Usa MAE en unidades y WAPE sobre la suma de unidades reales.

Los retrasos de la prueba representan **predicción de un mes vista con actualización mensual**: cuando predim junio, mayo ya es conocido. No equivale a predecir seis meses simultáneamente en diciembre. No usas datos de enero–junio para afinar el modelo antes de hacer la prueba final.

## Qué hay para aprender?

- Estacionalidad diferente por línea, crecimiento gradual y diferencias entre canales y regiones.
- Campañas, presupuesto y descuentos con efectos sobre las ventas.
- Variabilidad aleatoria, dependencia temporal y episodios de stock insuficiente.
- Clientes con comportamientos solapados, devoluciones, antigüedad y recència.
- Nulos controlados: en ventas, 18 en gasto real, 24 en descuento real, 18 en sesiones, 12 en presupuesto y 12 en descuento previsto; en clientes, 30 en gasto anual, 20 en edad y 20 en recència. No hay claves ni objetivos ausentes ni duplicados intencionados.

Los patrones son hipótesis simuladas, no estimaciones causales ni evidencia sobre el mercado real. No hay un hashtag de segmento «correcto» ni una respuesta única de clustering. En clientes, `annual_spend` deriva de frecuencia × ticket: considera la redundancia antes de usar las tres variables en clustering.

Consulta el [diccionari de dades](DICCIONARI_DADES.md) para unidades, campos y valores válidos. Para trabajar, usa los CSV de esta carpeta.
