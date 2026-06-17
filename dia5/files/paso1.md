# Paso 1 — Construir el cuadro base de cuatro columnas

## Objetivo
A partir del extracto bancario y del libro auxiliar de bancos, construir el cuadro de
la **prueba de efectivo (conciliación cuadrática)** con las cuatro columnas —
Saldo inicial, Ingresos, Egresos y Saldo final — para **BANCO** y para **LIBROS**.

## Insumos
- `extracto_banco.csv` — movimientos según el banco.
- `auxiliar_libros.csv` — movimientos según la contabilidad.

Ambos archivos tienen columnas: `fecha, concepto, tipo, valor, saldo`,
donde `tipo` es uno de: `saldo_inicial`, `credito`, `debito`.

## Instrucciones
1. Lee ambos archivos.
2. Para **BANCO** calcula:
   - Saldo inicial = `saldo` de la fila con `tipo = saldo_inicial`.
   - Ingresos = suma de `valor` donde `tipo = credito`.
   - Egresos = suma de `valor` donde `tipo = debito`.
   - Saldo final = `saldo` de la última fila.
3. Para **LIBROS** calcula lo mismo a partir del auxiliar.
4. Verifica el **cuadre interno** de cada fuente:
   `Saldo inicial + Ingresos − Egresos = Saldo final`.
   Si alguna fuente no cuadra internamente, **detente** y reporta el descuadre.
5. NO intentes explicar todavía las diferencias entre banco y libros.
   Eso es trabajo del Paso 2.

## Entregable
Crea `cuadro_4col.csv` con esta estructura exacta:

```
fuente,saldo_inicial,ingresos,egresos,saldo_final
BANCO,...,...,...,...
LIBROS,...,...,...,...
```

Además, muestra en pantalla un resumen con la **diferencia por columna**
(BANCO − LIBROS), señalando en cuáles columnas hay diferencia.
