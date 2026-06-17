# Paso 2 — Identificar y clasificar las partidas conciliatorias

## Objetivo
Explicar las diferencias entre **BANCO** y **LIBROS** detectadas en el Paso 1,
identificando y clasificando cada partida conciliatoria.

## Insumos
- `cuadro_4col.csv` — salida del Paso 1 (resumen por columnas).
- `extracto_banco.csv` y `auxiliar_libros.csv` — detalle de movimientos.

## Instrucciones
1. Cruza los movimientos del extracto contra los del auxiliar.
   Empareja **principalmente por `fecha` y `valor`** (los conceptos pueden estar
   redactados distinto en cada fuente).
2. Identifica las partidas que quedan **sin emparejar** y clasifícalas:
   - **Cheque en circulación**: registrado en LIBROS (egreso) pero no pagado aún por el BANCO.
   - **Depósito en tránsito**: registrado en LIBROS (ingreso) pero no abonado aún por el BANCO.
   - **Nota débito no registrada**: cargo del BANCO (comisiones, GMF/4x1000) que LIBROS aún no causa.
   - **Nota crédito no registrada**: abono del BANCO (rendimientos) que LIBROS aún no causa.
   - **Error de digitación** en cualquiera de las dos fuentes.
3. Para cada partida indica: tipo, descripción, valor, columna(s) afectada(s)
   y a cuál fuente debe ajustarse para llegar al saldo real.
4. Comprueba que la suma de las partidas **explique exactamente** la diferencia
   del saldo final detectada en el Paso 1. Si sobra o falta diferencia por explicar,
   indícalo claramente.

## Entregable
Crea `partidas_conciliatorias.csv` con esta estructura:

```
tipo,descripcion,valor,columna_afectada,ajusta_a
```

Ejemplo de filas (ilustrativo, NO copies valores: dedúcelos de los insumos):

```
cheque_en_circulacion,Cheque NNNN proveedor,000,egresos|saldo_final,BANCO
nota_debito,Comision + GMF,000,egresos|saldo_final,LIBROS
```

Además, muestra un resumen: cuánto de la diferencia total explica cada partida.
