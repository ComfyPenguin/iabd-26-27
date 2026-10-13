# Modelo de mesas · BiciTierra Market

## Ventas

`sales_history.csv` es una tabla de hechos mensual con clave `month + channel + product_line + region`. Se puede relacionar con dimensiones de calendario, canal, producto y región. Suma unidades e ingresos; no sumas precios ni porcentajes. Para un descuento agregado ponderado, justifica los pesos y el tratamiento de nulos.

`sales_forecast_input.csv` contiene inputs de julio de 2026, no ventas reales. Guarda las predicciones en una mesa separada con la misma clave y una marca clara de previsión. No representas el mes futuro como venta cero.

## Clientes

`customers.csv` es una fotografía independiente de 2.000 clientes a 30/06/2026. No tiene clave transaccional compartida con ventas. Muestra los segmentos en una vista separada; no hagas una unión muchos-a-muchos por canal o región ni esperas que el gasto sume los ingresos de ventas.

Consulta el [diccionari](../dades/DICCIONARI_DADES.md) para granularidad, unidades y disponibilidad de los campos.
