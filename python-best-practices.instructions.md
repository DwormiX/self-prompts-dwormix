---
description: "Use when: escribir código Python, notebooks, scripts de ciencia de datos, limpieza de datos, pandas, numpy, matplotlib, PEP 8, snake_case, refactorizar, legibilidad, buenas prácticas, nombrar variables, funciones, constantes, modularización. Covers naming conventions, variable initialization, constants, single responsibility functions, clean notebooks, and data science workflow structure."
applyTo: "**/*.{py,ipynb}"
---

# Buenas Prácticas de Programación en Python

## Propósito

Código legible, mantenible y fácil de depurar en **scripts**, **notebooks** y **soluciones de ciencia de datos**. Esta guía adapta principios clásicos al ecosistema Python respetando las diferencias del lenguaje: comprensiones, `break`/`continue` controlados, funciones auxiliares y celdas de notebook.

## Reglas Rápidas (Quick Reference)

| # | Regla | Aplica a |
|---|-------|----------|
| 1 | Nombres descriptivos y pronunciables | Variables, funciones, clases |
| 2 | Inicializar siempre | Acumuladores, listas, banderas |
| 3 | Extraer constantes de valores mágicos | Umbrales, tasas, límites |
| 4 | Una convención por archivo | Estilo, nomenclatura, indentación |
| 5 | Funciones pequeñas, una responsabilidad | Lógica reutilizable |
| 6 | Evitar variables globales | Siempre que sea posible |
| 7 | Una instrucción por línea | Salvo comprensiones legibles |
| 8 | Comentar el porqué, no el qué | Decisiones, no mecánica |
| 9 | Separar E/S de lógica | Input, cálculo, output |
| 10 | Celdas de notebook enfocadas | Una idea por celda |

---

## 1. Nombres Descriptivos

El nombre debe explicar **qué contiene** o **qué hace**.

### ✅ Preferir

```python
cantidad_alumnos = 0
indice_usuario = 0
total_ventas = 0

def es_par(numero: int) -> bool:
    return numero % 2 == 0

def calcular_area_rectangulo(base: float, altura: float) -> float:
    return base * altura
```

### ❌ Evitar

```python
x = 0          # ¿qué representa x?
xx = 0         # variante críptica
temp = 0       # demasiado genérico
```

### Criterio

- Pronunciable y escribible sin esfuerzo.
- Específico al contexto: `indice_fila` > `i` cuando hay múltiples índices.
- Para funciones: verbo + sustantivo (`calcular_media`, `filtrar_outliers`).
- Un nombre largo es aceptable si gana claridad, pero no abuses.

---

## 2. Inicialización de Variables

Toda variable debe tener un valor definido **antes de usarse**.

### ✅ Preferir

```python
suma = 0
contador = 0
nombres = []
encontrada = False
resultado = None
```

### ❌ Evitar

```python
suma          # NameError potencial
acumulador    # ¿vale 0, None, o qué?
```

### Por tipo

| Propósito | Valor inicial |
|-----------|---------------|
| Acumulador numérico | `0` |
| Lista a construir | `[]` |
| Bandera lógica | `False` |
| Valor aún no determinado | `None` |

---

## 3. Constantes y Valores Mágicos

Si un valor tiene **significado de dominio** o se **repite**, dale nombre.

### ✅ Preferir

```python
IVA = 0.21
MAX_ALUMNOS = 50
UMBRAL_OUTLIER = 3.0
SEXO_FEMENINO = "F"
```

### ❌ Evitar

```python
precio_final = precio * 1.21      # ¿de dónde sale 1.21?
if z_score > 3.0:                 # ¿por qué 3.0?
```

### Criterio

- Un número mágico que aparece **más de una vez** → constante.
- Una política del problema (tasa, límite, umbral) → constante.
- Un literal que podría cambiar en el futuro → constante.

---

## 4. Convención Única

Un archivo, un estilo. Sin mezclas.

### Estándar para este proyecto

- `snake_case` para variables, funciones y archivos.
- `UPPER_CASE` para constantes.
- `PascalCase` solo para clases.
- PEP 8 como referencia base.
- Type hints en funciones expuestas (`def calcular(x: int) -> float:`).

### ✅ Preferir

```python
cantidad_alumnos = 0
def calcular_promedio(valores: list[float]) -> float:
    return sum(valores) / len(valores)
```

### ❌ Evitar

```python
cantidadAlumnos = 0       # camelCase mezclado
def CalcularPromedio():   # PascalCase en función suelta
```

---

## 5. Modularización

Divide el problema en **funciones pequeñas con una sola responsabilidad**.

### ✅ Preferir

```python
def cargar_datos(ruta: str) -> pd.DataFrame:
    return pd.read_csv(ruta)

def limpiar_nulos(df: pd.DataFrame) -> pd.DataFrame:
    return df.dropna()

def entrenar_modelo(X, y):
    return RandomForestClassifier().fit(X, y)
```

### Estructura recomendada para notebooks

1. **Carga** → 2. **Exploración** → 3. **Limpieza** → 4. **Transformación** → 5. **Modelado** → 6. **Evaluación**

Cada etapa en su propia celda (o grupo de celdas) con una celda Markdown que la introduzca.

---

## 6. Variables Globales

Los datos entran por **parámetros**, los resultados salen por **retorno explícito**.

### ✅ Preferir

```python
def sumar(a: int, b: int) -> int:
    return a + b

resultado = sumar(2, 8)
```

### ❌ Evitar

```python
resultado = 0

def sumar(a, b):
    global resultado
    resultado = a + b
```

### En notebooks

Es inevitable usar variables entre celdas. Controla el alcance:
- Nombra las variables de forma que se entienda de dónde vienen.
- No reutilices el mismo nombre para cosas distintas en celdas diferentes.
- Si una celda modifica un DataFrame, documéntalo.

---

## 7. Indentación, Legibilidad y una Instrucción por Línea

### ✅ Preferir

```python
if numero > 0:
    if numero % 2 == 0:
        print("Positivo y par")

suma += valor
indice += 1
```

### ❌ Evitar

```python
if numero > 0:
if numero % 2 == 0:        # sin indentación
print("Ok")

suma += valor; indice += 1  # dos instrucciones en una línea
```

### Excepción

Comprensiones de lista bien formadas son aceptables:

```python
cuadrados = [x**2 for x in range(10) if x % 2 == 0]
```

Si una comprensión ocupa más de ~80 caracteres, usa un bucle tradicional.

---

## 8. Paréntesis en Condiciones

Agrupar condiciones complejas **siempre** con paréntesis. En pandas es **obligatorio**.

### ✅ Preferir

```python
if (edad < MAX_EDAD) and (activo is True):
    procesar()

# En pandas:
df_filtrado = df[(df["edad"] > 18) & (df["activo"] == True)]
```

---

## 9. Comentarios Útiles

El código explica el **qué**, el comentario explica el **porqué**.

### ✅ Preferir

```python
# Eliminamos outliers > 3σ para no distorsionar la regresión
df_limpio = df[df["z_score"].abs() <= 3]
```

### ❌ Evitar

```python
i += 1  # incremento i en 1
```

### Criterio

- Si necesitas muchos comentarios para que se entienda → refactoriza.
- Cada bloque de limpieza o transformación merece una línea de justificación.
- En notebooks, una celda Markdown breve antes de un paso importante es mejor que un comentario largo.

---

## 10. Pre y Post Condiciones

Documenta qué **espera** y qué **garantiza** cada función importante.

### ✅ Preferir

```python
def division_entera(numerador: int, denominador: int) -> int:
    """
    Pre: denominador != 0.
    Post: retorna el cociente entero de numerador / denominador.
    """
    return numerador // denominador
```

### En notebooks

Una celda Markdown antes de un bloque de modelado que diga:
> *"Esperamos que las features estén escaladas. La salida será un DataFrame con probabilidades por clase."*

---

## 11. Separar Entrada/Salida de Lógica

La función que **habla con el usuario** no debería ser la que **calcula**.

### ✅ Preferir

```python
def calcular_area(base: float, altura: float) -> float:
    return base * altura

def ejecutar_calculo():
    base = float(input("Base: "))
    altura = float(input("Altura: "))
    print(f"Área: {calcular_area(base, altura)}")
```

### En notebooks

Celda 1: cargar datos. Celda 2: transformar. Celda 3: visualizar. Celda 4: concluir.

---

## 12. `break` y `continue`

No están prohibidos, pero deben **simplificar de verdad** el código.

### ✅ Preferir (búsqueda con condición clara)

```python
indice = 0
while (indice < len(valores)) and (valores[indice] != buscado):
    indice += 1
```

### ✅ Aceptable en ciencia de datos

```python
for col in df.columns:
    if df[col].nunique() <= 1:
        break  # columna constante detectada, no seguimos
```

### Criterio

- Si el `break` hace el código **más corto y más claro** → úsalo.
- Si el `break` oscurece la condición de salida → reescribe el bucle.
- `continue` solo si salta iteraciones triviales y ahorra indentación profunda.

---

## 13. No Declarar Variables de Más

Cada variable debe **aportar claridad** o **reutilización**. Si no, sobra.

### ✅ Preferir

```python
total = precio * unidades
print(f"Total: {total}")
```

### ❌ Evitar

```python
id_producto = None       # nunca se usa
tasa_descuento = 0.0     # nunca se aplica
subtotal = precio * unidades
total = subtotal         # ¿por qué la copia?
print(f"Total: {total}")
```

---

## 14. Reglas Específicas para Notebooks de Ciencia de Datos

### Estructura

```
Celda Markdown: "## 1. Carga de datos"   →   Celda Code: pd.read_csv(...)
Celda Markdown: "## 2. Exploración"      →   Celda Code: df.info(), df.describe()
Celda Markdown: "## 3. Limpieza"         →   Celda Code: dropna(), fillna(), filtros
Celda Markdown: "## 4. Modelado"         →   Celda Code: train_test_split + .fit()
Celda Markdown: "## 5. Evaluación"       →   Celda Code: métricas + matriz de confusión
```

### Reglas operativas

- **Una celda = una idea**. Si una celda hace carga + limpieza + gráfico, sepárala.
- **Justifica cada decisión de limpieza** en Markdown o en un comentario breve.
- **Nombres de variable claros entre celdas**: `df_limpio`, `X_train`, `y_pred` son mejores que `df2`, `temp`, `x`.
- **Gráficos**: siempre con título, etiquetas en ejes y leyenda si aplica.
- **Métricas**: usar `print()` con formato legible, no solo el objeto crudo.
- **Reproducibilidad**: fijar `random_state=42` y documentarlo.
- **Cada celda debe poder ejecutarse de forma independiente** tras ejecutar las celdas previas en orden.

---

## Resumen

✔ Código que se **lee** como texto, no como acertijo.  
✔ Funciones que hacen **una cosa** y la hacen bien.  
✔ Notebooks que cuentan una **historia clara**: carga → limpia → modela → evalúa.  
✔ Variables con **nombres que explican** y constantes que **eliminan magia**.  
✔ Si se puede **entender, corregir y defender** en un examen, cumple el objetivo.
