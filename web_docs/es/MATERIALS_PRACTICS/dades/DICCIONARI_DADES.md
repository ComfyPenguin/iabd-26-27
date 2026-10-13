# Diccionario de datos · BiciTierra Market

Todos los valores son sintéticos. Importes en EUR; proporciones entre 0 y 1 (0.15 significa 15%). Nulo no significa cero. Los periodos son de la simulación.

## Ventas mensuales

| Camp | Tipo / unidad | Definición y disponibilidad |
|---|---|---|
| month | texto YYYY-MM | Mas que se quiere predecir; conocido antes |
| channel | categoría | `store` o `ecommerce`; conocido antes |
| product_line | categoría | `urban`, `mountain`, `electric`; conocido antes |
| region | categoría | `coast`, `inland`, `metro`; conocido antes |
| marketing_spend | decimal EUR | Gastada real del mes; al cierre; puede faltar |
| avg_discount | proporción | Descuento efectivo del mes; al cierre; puede faltar |
| stock_unidos | entero unidades | Disponibilidad inicial del mes; conocida antes. No es stock final |
| website_sesiones | entero visites | Sesiones del mes atribuidas a la celda comercial, también de apoyo a tienda; al cierre; puede faltar |
| salas_unidos | entero unidades | Objetivo: unidades vendidas; al cierre, limitadas por el stock |
| revenue | decimal EUR | Ingresos netos de descuento, sin impuestos; al cierre; no predictor de unidades del mismo mes |
| marketing_budget | decimal EUR | Presupuesto aprobado antes del mes; puede faltar en el extracto histórico |
| planned_discount | proporción | Descuento previsto antes del mes; puede faltar en el extracto histórico |
| list_price | decimal EUR/unidad | Precio de tarifa representativo fijado antes del mes |
| campaign | categoría | Pla simulado: `none`, `spring`, `summer`, `black_friday`, `christmas`; conocido antes, no calendario comercial real |
| days_in_month | entero días | Número de días naturales del mes; conocido antes |
| closing_stock_unidos | entero unidades | `stock_units - sales_units`; al cierre; no predictor anticipado |
| stockout | entero 0/1 | 1 si la demanda simulada supera la disponibilidad; solo al cierre |

No hay reposición intramensual. Por eso `sales_units <= stock_units` y el stock final nunca es negativo. No se publica la demanda no servida; el fichero permite explicar la censura por stock, no evaluar una predicción exacta de demanda latente.

## Entrada de predicción

`sales_forecast_input.csv` usa los diez campos conocidos antes del mes: `month`, `channel`, `product_line`, `region`, `stock_units`, `marketing_budget`, `planned_discount`, `list_price`, `campaign`, `days_in_month`.

Contiene julio de 2026 y no incluye columnas de objetivo vacías: no se tiene que confundir ausencia de resultado con ventas cero. Los retrasos se obtienen del histórico con la clave completa y el mes correcto.

## Clientes

| Camp | Tipo / unidad | Definición |
|---|---|---|
| customer_id | identificador | Clave ficticia única; excluirla del clustering |
| age | entero años | Edad entre 18 y 79; puede faltar |
| city | categoría | Gandia, Oliva, Sueca, Alzira, Tascas o Carcaixent; ubicación ficticia |
| channel_preference | categoría | `store` o `ecommerce` |
| annual_spend | decimal EUR | Importe acumulado de pedidos de los últimos 12 meses, antes de devoluciones; puede faltar |
| avg_order_value | decimal EUR/comanda | Ticket mediano de los últimos 12 meses |
| orders_last_12m | entero comandes | Pedidos en los últimos 12 meses; muestra de clientes con al menos una compra |
| returns_ratio | proporción | `returned_orders_last_12m / orders_last_12m`, redondeado a cuatro decimales |
| bike_interest | categoría | `urban`, `mountain`, `electric` o `service` |
| service_interest | categoría | `low`, `medium`, `high` |
| snapshot_dato | texto YYYY-MM-DD | Fecha de corte 2026-06-30; común a toda la muestra |
| tenure_months | entero meses | Antigüedad de 2 a 120 meses |
| days_since_last_order | entero días | Recència a la fecha de corte, entre 0 y 364; puede faltar |
| returned_orders_last_12m | entero comandes | Número de pedidos con devolución; no supera el de pedidos |

`annual_spend = avg_order_value × orders_last_12m` con redondeo a céntimos. Si falta gastada se puede reconstruir en este caso simulado; hay que justificarlo. No hay datos posteriores a la fotografía ni hashtag de compra futura: este CSV sirve para segmentación, no para validar predicción de churn o compras futuras.
