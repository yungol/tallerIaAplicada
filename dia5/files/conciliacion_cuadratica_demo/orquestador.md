# Orquestador — Conciliación cuadrática completa

Ejecuta la conciliación cuadrática de principio a fin **encadenando los tres pasos**.
Cada paso consume el entregable del paso anterior.

## Secuencia
1. Ejecuta las instrucciones de **`paso1.md`**.
   → Debe generar `cuadro_4col.csv`.
   → Si el cuadre interno de alguna fuente falla, **no continúes**: repórtalo.
2. Ejecuta las instrucciones de **`paso2.md`** usando `cuadro_4col.csv` y el detalle.
   → Debe generar `partidas_conciliatorias.csv`.
3. Ejecuta las instrucciones de **`paso3.md`** usando los dos archivos anteriores.
   → Debe generar `informe_cuadratica.md`.

## Criterio de éxito
La orquestación es **EXITOSA** únicamente si `informe_cuadratica.md` concluye
**✅ CUADRA** en las cuatro columnas.
Si concluye **❌**, reporta en qué columna y por qué monto, y señala en qué paso
conviene abrir el entregable intermedio para auditar dónde se rompió.

## Regla de oro (anti-invención)
No inventes valores. Si un dato no está en los insumos, **detente y repórtalo**.
Cada cifra del informe final debe poder rastrearse hasta `extracto_banco.csv`
o `auxiliar_libros.csv`. Esa trazabilidad es el papel de trabajo.
