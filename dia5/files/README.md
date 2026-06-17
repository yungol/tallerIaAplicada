# Demo: Conciliación cuadrática encadenada (guía para el taller)

Kit para mostrar **encadenamiento de instrucciones en modo agente** (VS Code, Cline,
Claude Code, Copilot agente, etc.) usando un caso que los contadores reconocen al
instante: la **conciliación cuadrática** (prueba de efectivo a cuatro columnas).

## Archivos
| Archivo | Rol |
|---|---|
| `extracto_banco.csv` | Insumo: movimientos del banco (Mayo 2026) |
| `auxiliar_libros.csv` | Insumo: libro auxiliar de bancos |
| `paso1.md` | Construye el cuadro de 4 columnas → `cuadro_4col.csv` |
| `paso2.md` | Identifica partidas conciliatorias → `partidas_conciliatorias.csv` |
| `paso3.md` | Valida el cuadre y emite → `informe_cuadratica.md` |
| `orquestador.md` | Encadena los 3 pasos y exige el cuadre como criterio de éxito |

## Cómo correrlo en vivo
1. Abre la carpeta en VS Code con el agente activo.
2. **Plano "manual"** (para que vean el encadenamiento): dile al agente
   *"Sigue las instrucciones de `paso1.md`"*, revisa el `cuadro_4col.csv`,
   luego `paso2.md`, luego `paso3.md`.
3. **Plano "orquestado"**: en otra corrida, *"Sigue las instrucciones de
   `orquestador.md`"* y deja que haga los tres de un tirón.

## Las cifras (para que sepas qué debe salir)
Diferencia "cruda" del saldo final que verán tras el Paso 1:

- Saldo final BANCO  = **$17.000.000**
- Saldo final LIBROS = **$16.500.000**
- Diferencia = **$500.000** (banco mayor)

Esa diferencia se explica con **dos** partidas (Paso 2):

- **Cheque 1204 en circulación** = $800.000
  (en libros, el banco aún no lo paga → se resta al banco)
- **Nota débito comisión + GMF 4x1000** = $300.000
  (en el banco, libros aún no la causa → se resta a libros)

800.000 − 300.000 = 500.000 ✔ explica toda la diferencia.

Saldos **ajustados** que deben cuadrar en las cuatro columnas (Paso 3):

| Columna | Banco aj. | Libros aj. |
|---|---|---|
| Saldo inicial | 10.000.000 | 10.000.000 |
| Ingresos | 25.000.000 | 25.000.000 |
| Egresos | 18.800.000 | 18.800.000 |
| Saldo final | 16.200.000 | 16.200.000 |

Y `10.000.000 + 25.000.000 − 18.800.000 = 16.200.000` → **✅ CUADRA**.

## El momento didáctico
- **Abre el entregable intermedio** (`cuadro_4col.csv`) antes de seguir al Paso 2.
  Ahí cae la moneda: no es una caja negra, es un papel de trabajo auditable.
- **"Hacer fallar el cuadre" en vivo (opcional):** en el Paso 2, pídele al agente
  que ignore la nota débito (o bórrala de `partidas_conciliatorias.csv`). El Paso 3
  reportará **❌ NO CUADRA** por **$300.000** en la columna de saldo final. Vuelve a
  incluirla y ahora sí cuadra. Eso demuestra que el último eslabón **valida**, no solo
  redacta.
- **El paralelo que les habla:** instrucción suelta → `orquestador.md` → *skill*
  equivale a **ad-hoc → procedimiento → política**.
