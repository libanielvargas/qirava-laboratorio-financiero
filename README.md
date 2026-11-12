# Qirava · Laboratorio financiero v1.2

**Aplicación pública (sin login):** https://libanielvargas.github.io/qirava-laboratorio-financiero/

Herramienta autónoma e interactiva para contrastar la tesis: **destinar el excedente creciente del canon al abono a capital o acumularlo invertido hasta liquidar la hipoteca de una vez**. Se usa el mismo inmueble, crédito, alquiler, horizonte temporal y presupuesto mensual en ambas estrategias. No procesa dinero ni requiere cuentas. No forma parte del software operacional Qirava Rentas.

## Modelo y parámetros

- Escenario inicial: inmueble COP 100 millones, inicial 0 %, tasa hipotecaria 10 % efectiva anual, 20 años, canon mensual COP 1.200.000, incremento del 5 % anual, inversión al 10 % E.A., inflación del 5 %, gastos 0 para aislar hipótesis.
- Calcula cada mes: canon/ocupación, costos opcionales, cuota ordinaria, intereses, abono a capital, saldo de deuda, excedente, retorno/variación del fondo, saldo de inversión y recursos adicionales requeridos.
- Tres estrategias: 100 % abono extraordinario; 100 % inversión; proporción ajustable.
- Tabla anual de tasas editables **por cada año**: tasa hipotecaria, retorno de inversión, IPC y aumento de canon. Presets tasas constantes; refinanciación del banco −2 pp desde año 5; inversiones volátiles con pérdidas y ganancias.
- Si cambia la tasa hipotecaria, se calcula nueva cuota ordinaria usando deuda pendiente del calendario de crédito sin abonos extraordinarios y meses restantes; los abonos adicionales se emplean para acortar duración. No se descuentan costos bancarios por compra de cartera.
- Cambio nominal/real: cualquier flujo o saldo real = valor nominal / producto de factores de IPC anuales acumulados hasta el mes. La inflación no es un ingreso adicional.
- Gráficos de deuda y fondo, canon y cuota, interés/capital, valor nominal/real; tabla por cuota mensual exportable a CSV.
- Gráficos y valores precargados *en el propio HTML* para que exista contenido útil incluso en vistas sin JavaScript. Para controles interactivos, abrir la URL web en un navegador moderno.

## QA verificado el 11 de noviembre de 2026

Validación con Google Chrome/Playwright en Mac y cálculo independiente mes a mes en Python. Caso inicial con cuota mínima aproximada **COP 936.640** y excedente inicial de **COP 263.360**.

| Trayectoria de tasas | Abono directo | Inversión y cancelación total |
|---|---:|---:|
| 10 % E.A. banco y 10 % E.A. inversión constantes | Cuota 105 | Cuota 105 |
| Banco 10 % E.A. hasta año 4 y 8 % desde año 5; inversión 10 % | Cuota 102 | Cuota 100 |
| Banco 10 % E.A.; rendimientos variables con años negativos | Cuota 105 | Cuota 113 |

En último caso se usa trayectoria anual de inversión [-8,14,5,17,2,12,0,10,-5,18,6,15,1,9,4,11,-3,8,5,10] % E.A. Se asume canon creciente al 5 % y ocupación total. No se incorporan impuestos sobre rendimientos ni comisiones ni costos de refinanciación.

Pruebas Chrome:
- 360, 390, 430, 782, 1024 y 1440 píxeles: no desbordamiento horizontal, cuatro gráficos y resultados visibles en primera carga, primer gráfico visible en móvil.
- Cambio de tasa bancaria (año 5), variación de rendimientos con pérdidas, edición manual año 2, inflación año 4 y conmutación valores reales/nominales: resultados recalculados.
- Edición de campos móviles, persistencia tras recarga en almacenamiento local y exportación CSV: correctos.
- Página publicada: respuesta HTTP 200 y prueba real de controles en la URL pública. Sin errores JavaScript.
- JavaScript deshabilitado: valores del ejemplo y gráficos estáticos visibles.

## Interpretación matemática

Una deuda fija en pesos no pierde *saldo nominal* por inflación; disminuye su **valor real**. No debe sumarse una supuesta «ganancia por inflación» a los retornos del fondo, porque el descuento por IPC ya refleja el poder adquisitivo en ambas estrategias. La comparación de retorno neto de inversión vs tasa del crédito debe usar la misma moneda, plazo, fecha de comparación y riesgos/retenciones aplicables.

No garantiza tasas o rentas futuras. Los escenarios son hipótesis editables, útiles para poner a prueba la tesis, no una predicción del mercado.

## Referencias canónicas

[Qirava Rentas — definición y estado de producto](https://github.com/libanielvargas/qirava-knowledge-vault/blob/main/90-research/product-discovery/2026-11-07_qirava-rentas_plataforma-arriendos-convivencia.md) (repositorio privado). Este laboratorio mantiene su repositorio y URL web separados del producto SaaS.