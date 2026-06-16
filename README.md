# Dashboard World Salud

Repositorio que centraliza el **Dashboard ejecutivo de World Salud** (Farmacias,
Laboratorio y Medicina) y los archivos fuente del P&L. El dashboard es un archivo
HTML único, autocontenido, con dos pestañas (**2025** cerrado / **2026** en curso),
construido con Chart.js.

## Contenido del repo

| Archivo | Descripción |
|---|---|
| `Dashboard - WS - MAY26.html` | **Último dashboard vigente** — Real Ene–May 2026 + Budget Jun–Dic. |
| `Dashboard - WS - MAY26 - DEMO.html` | Versión ilustrativa (presentaciones externas). |
| `Dashboard_WS_ABR26.html` | Dashboard del mes anterior (Abril 2026), base del actual. |
| `Economico_y_PL_052026_vs_BDGT.xlsx` | P&L fuente de Mayo 2026 (hoja `REAL-BDGT2026`). |
| `Economico_y_PL_042026_vs_BDGT.xlsx` | P&L fuente de Abril 2026. |
| `INSTRUCCIONES.md` | Instructivo completo para generar/actualizar el dashboard cada mes. |

## Estado actual (Mayo 2026)

Acumulado **Ene–May 2026** (CON PAMI):

| Métrica | Real | Budget |
|---|---|---|
| Ingresos Netos IIBB | $10.089 B | $9.581 B |
| Costos Operativos | $4.053 B | $4.329 B |
| Otros Egresos | $1.384 B | — |
| Resultado Neto | $4.652 B | $3.413 B |
| MB% | 59.8% | 54.8% |
| MN% | 46.1% | 35.6% |

Mayo (mes): Ingresos Netos $2.171 B · Resultado Neto $1.107 B · MN% 51.0% · Margen Operativo% 60.6%.

> La pestaña **2025** es una foto fija y no debe modificarse.

## Cómo actualizar el mes siguiente

Ver [`INSTRUCCIONES.md`](INSTRUCCIONES.md). En resumen: partir del dashboard del mes
anterior (`Dashboard - WS - <MMM><YY>.html`), agregar el nuevo mes a los arrays del objeto `B` (2026), actualizar las
constantes acumuladas CON/SIN PAMI, los textos de encabezado/KPIs y `REAL_COUNT['26']`.
