# Paso 3 — Validar el cuadre y emitir el informe

## Objetivo
Aplicar las partidas conciliatorias al cuadro base y verificar que **BANCO** y
**LIBROS** cuadren en las cuatro columnas. Emitir el informe final.

## Insumos
- `cuadro_4col.csv` — salida del Paso 1.
- `partidas_conciliatorias.csv` — salida del Paso 2.

## Instrucciones
1. Parte de los saldos del cuadro base (BANCO y LIBROS).
2. Aplica cada partida a la fuente indicada en `ajusta_a`:
   - **Cheque en circulación** → resta del Saldo final del BANCO (y sube sus Egresos).
   - **Depósito en tránsito** → suma al Saldo final del BANCO (y sube sus Ingresos).
   - **Nota débito no registrada** → resta del Saldo final de LIBROS (y sube sus Egresos).
   - **Nota crédito no registrada** → suma al Saldo final de LIBROS (y sube sus Ingresos).
3. Calcula los **saldos ajustados** de BANCO y LIBROS en las cuatro columnas.
4. **VALIDA el cuadre** (esto es lo que hace "cuadrática" a la conciliación):
   - Que en cada fila ajustada se cumpla `SI + Ingresos − Egresos = SF`.
   - Que **BANCO ajustado = LIBROS ajustado** en las CUATRO columnas.
5. Veredicto: marca **✅ CUADRA** o **❌ NO CUADRA**. Si no cuadra, indica la
   columna y el monto exacto del descuadre.

## Entregable
Crea `informe_cuadratica.md` que contenga:
- Encabezado con cuenta y período.
- El cuadro de cuatro columnas mostrando: saldos según banco, saldos según libros,
  los ajustes aplicados a cada lado, y los **saldos ajustados**.
- La lista de partidas conciliatorias con su valor.
- El **veredicto final del cuadre** (✅ / ❌) por cada una de las cuatro columnas.
- Si NO cuadra: la diferencia que queda pendiente por explicar.
