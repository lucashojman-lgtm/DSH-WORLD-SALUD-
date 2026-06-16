# Instrucciones — Dashboard World Salud

Guía consolidada para generar y actualizar el dashboard mes a mes. Recoge todas las
correcciones acordadas en el proyecto.

## 1. Contexto

- Grupo con 3 unidades de negocio: **Farmacias, Laboratorio, Medicina**.
- Dashboard = HTML único con dos pestañas: **2025** (cerrado Ene–Dic, foto fija) y
  **2026** (en curso).
- Siempre dos versiones: archivo **real** (`V##`) y archivo **DEMO** (ilustrativo,
  generado desde el último real).

## 2. Archivos a proveer cada mes

- P&L del mes: `Economico_y_PL_MM2026_vs_BDGT.xlsx`, hoja **`REAL-BDGT2026`**.
- Dashboard del mes anterior (último `V##` real, no el DEMO).

## 3. Mapa de columnas (hoja REAL-BDGT2026)

| Concepto | Col |
|---|---|
| Real mensual | I=Ene, J=Feb, K=Mar, L=Abr, M=May, N=Jun … T=Dic |
| **Acumulado real** | **V (22)** |
| Budget mensual | X=Ene, Y=Feb, Z=Mar, AA=Abr, AB=May … AI=Dic |
| **Acumulado budget** | **AK (37)** |

## 4. Filas clave (layout del archivo "Versión Final" 2026)

> El layout de filas puede correrse entre versiones del Excel. Verificar siempre
> los **labels** de cada fila antes de extraer. Mapa vigente:

| Concepto | Fila | Nota |
|---|---|---|
| Farmacias | 7 | subtotal |
| Laboratorios | 20 | subtotal |
| Medicina | 37 | puede ser negativo (ajuste contable) — es correcto |
| Subtotal Ingresos (brutos) | 40 | |
| IIBB | 41 | negativo |
| **Total Ingresos Netos IIBB** | **42** | = Ingresos del dashboard |
| **Costos Operativos** (Total Egresos Operativos) | **171** | `costos_real` |
| Margen Bruto (Resultado) | 173 | `mb` / `ebitda_real` = Ing − CostosOp |
| MB% | 174 | V=real, AK=budget |
| Total de Otros Egresos | 187 | `cp_egr` |
| Total de Egresos | 189 | `egr_tot_abs` = Ing − Resultado |
| **Resultado Neto** | **191** | `rn_real` / `mn` |
| MN% | 192 | V=real, AK=budget |

**SIN PAMI** — valores ya calculados en el Excel, **siempre en filas 206–212**
(col V = real acum, AK = budget acum):

| Fila | Concepto |
|---|---|
| 206 | Ventas sin PAMI |
| 207 | Costos sin Frontier |
| 208 | Margen Operativo |
| 209 | MB% |
| 210 | Otros Egresos |
| 211 | Margen Neto |
| 212 | MN% |

## 5. Tabla CON PAMI (acumulado, constantes JS)

```
cp_ing      = V42       (Total Ingresos Netos IIBB acum)
cp_cos      = V171      (Costos Operativos acum)
cp_egr      = V187      (Otros Egresos acum) = cp_ing − cp_cos − cp_res
cp_res      = V191      (Resultado Neto acum)
cp_mb_real  = V174   |  cp_mb_bud = AK174
cp_mn_real  = V192   |  cp_mn_bud = AK192
```

## 6. Tabla SIN PAMI

Tomar directo de las filas 206–212 (no calcular a mano). En el HTML basta con
actualizar los 4 inputs; el resto se deriva y reproduce el Excel:

```
sp_ing_r = V206   sp_cos_r = V207   (mb = ing−cos, egr = mb*0.35, res = mb*0.65)
sp_ing_b = AK206  sp_cos_b = AK207
```

Nota de pie de tabla (obligatoria):
> LOS NÚMEROS SIN PAMI SON LOS COSTOS SIN FRONTIER Y EN EGRESOS SOLO SE CONTEMPLA
> IMPUESTOS A LAS GANANCIAS Y TASA DE HIGIENE

## 7. Arrays JS del objeto `B` (2026) — agregar el nuevo mes

Quitar un `null` del final y poner el valor del mes en la posición correspondiente:

`ing_real, costos_real, egr_tot_abs, rn_real, ebitda_real, farm_real, lab_real,
med_real, ing_inf (=ing_real), egr_inf (=costos_real), mb (=ebitda_real),
mn (=rn_real)`, y los escalares `pie_farm / pie_lab / pie_med` (valores del mes nuevo).

Los arrays `*_budget` / `*_bud` son el budget anual completo: **no cambian** entre
meses (verificar igualmente contra cols X..AI).

## 8. Cambios de texto / contadores

- `REAL_COUNT['26']` → cantidad de meses reales.
- `realMonths` en KPI 2026 → mismo número. Los KPI de "último mes" usan
  `[realMonths-1]` (NO un índice fijo).
- Header `DASHBOARD<br>MesActual 2026`.
- Banner: meses reales y mes que inicia el budget.
- Títulos: "Acumulado Ene–Mes", "Composición de Ingresos (Mes)", labels y tooltips
  de KPIs (Mes / Ene–Mes).
- Cascada 2026: suma los meses reales (`slice(0,REAL_COUNT['26'])`) e `iibb26` =
  |V41 acum|.

## 9. Verificaciones antes de entregar

- [ ] Líneas MB y MN llegan hasta el último mes real (no se cortan).
- [ ] Botón "Ocultar Budget" respeta exactamente los meses reales.
- [ ] Tabla CON PAMI: MB%/MN% coinciden con filas 174/192.
- [ ] Tabla SIN PAMI: valores de filas 206–212 (real y budget) → MB%/MN% positivos.
- [ ] KPIs muestran el mes correcto (ej. "May", "Ene–May").
- [ ] Pestaña **2025 no se modifica**.
- [ ] Pie chart usa el mes más reciente.
- [ ] `node --check` del script inline pasa sin errores.

## 10. Nombres de archivo

- Real: `Dashboard_WS_<MES>26_V01.html` (V02, V03… si hay correcciones).
- Demo: `Dashboard_WS_<MES>26_DEMO.html` (siempre desde el último real).

## Versión DEMO (presentaciones externas)

- Escalar todos los absolutos: ingreso máx mensual → $200.000 (factor
  `200000 / mayor_ingreso_mensual_en_miles`).
- Márgenes fijos: MB 30% / MN 10%. SIN PAMI real MB 25% / MN 5%.
  SIN PAMI budget 2026 ≈ 0; budget 2025 MB 3.64% / MN 2.40%.
- Banner obligatorio:
  > ⚠️ Los valores expuestos en este dashboard son de carácter ilustrativo y no
  > representan información financiera real de la compañía.
