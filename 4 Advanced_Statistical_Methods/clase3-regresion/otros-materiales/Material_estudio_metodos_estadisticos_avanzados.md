# Material de Estudio

# Métodos Estadísticos Avanzados: Regresión Lineal y Logística

## Módulo 2 — Applied Statistics with Python

**Docente:** Walter J. Méndez · UTEPSA / Khipus.ai
**Dataset de referencia:** `data_dev.csv` (Stack Overflow Developer Survey 2023 — 1.183 desarrolladores, 24 columnas)
**Notebook:** `03a_Metodos_Estadisticos_Avanzados_Practica_Guiada_Mini.ipynb`

---

## Índice

| Parte | Contenido |
|---|---|
| **1** | ¿Qué es la regresión? |
| **2** | Las dos regresiones y el mapa del módulo |
| **3** | Los conceptos, uno por uno, con la encuesta de desarrolladores |
| **4** | Python y las librerías: statsmodels y scikit-learn |
| **5** | Bibliografía comentada |
| **Anexo** | Tabla maestra de valores del dataset |

### Cómo usar este material

Esta clase tiene tres entregables en el campus, y cada uno se apoya en una parte distinta de este documento:

| Obligación | Qué te exige | Dónde está acá |
|---|---|---|
| **Examen MD2–U4** (abre 08/08, cierra 22/08) | Conceptos de regresión lineal **y** logística | Partes 1 a 3 completas + Anexo |
| **Trabajo práctico MD2–U4** (`student_scores.csv`) | *Hacer* una regresión simple en el dialecto scikit-learn | Parte 3 (§3.1–3.8) + **Parte 4.3** |
| **Foro 3: "Regresión Lineal Vs Logística"** | *Argumentar* la diferencia entre ambas | §2.2, §3.13 y §3.15 |

💡 Si solo tenés una hora: leé la Parte 1, la tabla de §2.2, y las fichas §3.4 (pendiente), §3.7 (RMSE), §3.8 (R²) y §3.13 (logística). Ese es el esqueleto de la clase.

---
---

# PARTE 1 — ¿Qué es la regresión?

## 1.1 La definición

> **La regresión es el conjunto de métodos para modelar la relación entre variables: construir, a partir de los datos, una función que conecte lo que sabés (X) con lo que querés predecir (Y) — y medir cuánto se equivoca esa función.**

Tres palabras cargan el peso de esa definición:

**Modelar** — reemplazar una nube de 1.183 puntos por una ecuación manejable. Un modelo es una simplificación deliberada: cambia detalle por capacidad de respuesta.

**Predecir** — usar esa ecuación para responder por casos que todavía no viste: *¿cuánto va a ganar un programador con 5 años de experiencia?*

**Medir cuánto se equivoca** — y esta es la palabra que separa al profesional del aficionado. Todo modelo se equivoca. La regresión seria no esconde ese error: **lo calcula y lo reporta al lado de la predicción**.

La tercera palabra es la frontera moral de esta clase. Una predicción sin su error al lado no es una predicción: es una promesa.

## 1.2 El problema que resuelve

En la Clase 1 aprendiste a resumir **una** variable a la vez: la media, la mediana, la desviación del precio. En la Clase 2 aprendiste a preguntarle a una diferencia si era real o azar.

Pero el mundo laboral no pregunta así. El mundo pregunta:

- *"Si contrato a alguien con 5 años de experiencia, ¿cuánto le tengo que pagar?"*
- *"Si el departamento tiene 80 m², ¿en cuánto lo publico?"*
- *"Si este cliente gana tanto y debe tanto, ¿le presto o no le presto?"*

Todas esas preguntas tienen la misma estructura: **conozco X, quiero Y**. Y todas exigen un **número** como respuesta, no un "hay relación".

La correlación —que ya conocés de la Clase 1— te dice que la relación existe y qué tan fuerte es. Pero la correlación entre salario y experiencia es 0,29, y con ese dato solo no podés responderle al gerente. **La regresión convierte la relación en una ecuación con la que se puede calcular.**

> **La pregunta ancla de toda esta clase:**
> ### *¿Cuánto va a ganar un programador con 5 años de experiencia?*
>
> La vamos a responder dos veces. Primero de forma ingenua (un número). Después de forma honesta (un número **con su error al lado**). La diferencia entre las dos respuestas es la clase entera.

## 1.3 Las dos preguntas de la regresión

Todo análisis de regresión responde, en orden, dos preguntas distintas — y confundirlas es el error profesional más caro de esta disciplina:

| # | Pregunta | Se responde con | En la encuesta |
|---|---|---|---|
| **1** | ¿**Existe** relación entre X e Y? | El **p-valor** de la pendiente | Sí: p = 2,32 × 10⁻²⁴ |
| **2** | ¿Qué tan **fuerte y útil** es esa relación? | **R²** y **RMSE** | Casi nada: R² = 0,0841, error típico del 86% |

Fijate en la tercera columna: en nuestro dataset las dos respuestas van en **direcciones opuestas**. La relación entre experiencia y salario existe sin ninguna duda (ese p-valor es una millonésima de una millonésima de una millonésima de duda)… y aun así el modelo es casi inservible para predecir.

> ## 💡 El eje de esta clase, en tres palabras
> ### **Significativo ≠ útil.**
> ### El p-valor te dice si la relación existe. El R² y el RMSE te dicen si te sirve. Un modelo puede aprobar la primera pregunta con honores y reprobar la segunda estrepitosamente. El nuestro lo hace.

## 1.4 Lo que ya sabés y hoy se junta

Esta es la clase donde el módulo entero se cierra sobre sí mismo. Nada de lo anterior fue relleno:

| De la clase... | Concepto que aprendiste | Hoy reaparece como... |
|---|---|---|
| **1 — Descriptiva** | Correlación (r) | El punto de partida del modelo — y r² **es** el R² |
| **1 — Descriptiva** | Media, desviación, asimetría | La forma de los **residuos** se diagnostica con ellas |
| **1 — Descriptiva** | Outliers | Los sueldos de $1.200.000 que tiran de la recta |
| **2 — Inferencial** | H₀ y el p-valor | La pendiente tiene su propia H₀ (β₁ = 0) y su propio p-valor |
| **2 — Inferencial** | "Significativo" | La palabra que hoy vamos a desarmar |

**El puente exacto con la Clase 2:** el sábado pasado le preguntaste a una **diferencia entre dos grupos** si era real, y la respuesta fue *no* (p = 0,3529). Hoy le hacés la misma pregunta a una **pendiente**, y la respuesta va a ser *sí*… y aun así el modelo no sirve para predecir. Dos sábados, dos fallas de intuición en direcciones opuestas.

## 1.5 Lo que la regresión NO hace

Esta sección importa tanto como la definición.

| No hace | Detalle |
|---|---|
| **Establecer causas** | β₁ = $3.320/año significa que la experiencia *se asocia* con más sueldo, no que un año más te lo *cause* |
| **Predecir fuera del rango observado** | El modelo devuelve un número para cualquier X, pero fuera del rango de los datos ese número es ficción (§3.5) |
| **Elegir las variables por vos** | Si le das predictores redundantes, los acepta y se confunde en silencio (§3.11) |
| **Garantizar el futuro** | El modelo aprende del pasado; si el mercado cambia, la recta queda vieja |
| **Reemplazar mirar los datos** | Cuatro datasets distintos pueden dar la misma recta y el mismo R² (§3.8, Anscombe) |

> ## ⚠️ La frontera, en una frase
> ### **Asociación ≠ causalidad, y ajustar ≠ acertar.**
> ### La regresión encuentra la mejor recta posible para tus datos. "La mejor posible" puede seguir siendo mala — y el modelo no te lo va a decir solo. Medirlo es tu trabajo.

---
---

# PARTE 2 — Las dos regresiones y el mapa del módulo

## 2.1 Dónde estamos parados

El recorrido completo del Módulo 2, según el mapa que venimos siguiendo desde la Clase 1:

```
   ESTADÍSTICA          PROBABILIDAD          ESTADÍSTICA           MODELADO
   DESCRIPTIVA               │                INFERENCIAL          ESTADÍSTICO
        │                    │                    │                    │
  Resume lo que        Modela el azar       Generaliza de la      Relaciona variables
   ya tenemos                │               muestra al total      para PREDECIR
        │                    │                    │                    │
   "Esto es lo         "Esto es lo que      "Esto es lo que       "Esto es lo que
    que hay"            podría pasar"        probablemente         va a pasar"
        │                    │                sea cierto"              │
     Clase 1              Clase 2              Clase 2            ✅ ESTAMOS ACÁ
                                                                    (Clase 3)
```

El modelado es la **etapa de cierre** porque usa todo lo anterior: describe los datos antes de modelar (descriptiva), asume una estructura de azar en los errores (probabilidad) y le pone p-valores e intervalos a sus coeficientes (inferencial).

Y tiene otra particularidad: es la etapa donde la estadística **se convierte en machine learning**. Pero de eso hablamos en §2.5.

## 2.2 Las dos máquinas de esta clase

La clase enseña dos modelos. Se parecen en el nombre y en la mecánica, pero responden preguntas de naturaleza distinta:

| Criterio | Regresión LINEAL | Regresión LOGÍSTICA |
|---|---|---|
| **Predice** | Un **número** continuo | Una **probabilidad** (que se convierte en categoría) |
| **Pregunta típica** | ¿*Cuánto* va a ganar? | ¿*Es* de Estados Unidos o no? |
| **Variable Y** | Cuantitativa (salario en $) | Binaria (sí/no, 1/0) |
| **Forma del modelo** | Una recta | Una curva en S (sigmoide) |
| **En la encuesta** | `salario ~ experiencia` | `es_us ~ salario` |
| **Se evalúa con** | RMSE, MAE, R² | Exactitud, matriz de confusión |
| **En ML se llama** | Regresión | **Clasificación** |

La regla de profundidad de la clase fue: **lineal con las manos, logística con los ojos**. La lineal la construimos, la desarmamos y le medimos el error pieza por pieza. La logística la vimos funcionar y entendimos su lógica — el detalle fino queda para el Módulo 3.

💡 Para el **Foro 3** ("Regresión Lineal Vs Logística"), esta tabla es tu argumento. La frase corta: *la lineal responde "cuánto"; la logística responde "qué tan probable" — y por eso una dibuja una recta que puede irse al infinito y la otra una S encerrada entre 0 y 1.*

## 2.3 Predecir no es lo mismo que explicar

El mismo modelo de regresión tiene dos oficios distintos, y conviene saber cuál estás ejerciendo (Bruce y Bruce le dedican una sección con nombre propio: *Prediction vs. Explanation*):

| Oficio | Qué te importa del modelo | Quién lo usa así |
|---|---|---|
| **Explicar** | Los coeficientes: ¿cuánto vale β₁? ¿es distinto de cero? ¿qué dice del mercado? | Economistas, científicos sociales, auditores |
| **Predecir** | El error en casos nuevos: ¿cuánto le erra? ¿me sirve para decidir? | Ciencia de datos, machine learning |

Nuestro modelo del salario es un caso de manual: como **explicación** tiene algo que decir (la experiencia sí se asocia al sueldo, unos $3.320 por año, y eso no es azar). Como **predictor** es casi inútil (le erra el 86% del sueldo típico).

**Los dos veredictos son verdad a la vez.** Reportar solo uno de los dos es contar media historia.

## 2.4 El contrato del Capítulo 2 §4 (lo que evalúa el examen)

El capítulo oficial del curso define qué debe cubrir esta unidad. Este material cubre todo el contrato — y lo excede a propósito donde el capítulo se queda corto:

| § del capítulo | Contenido oficial | Dónde está acá |
|---|---|---|
| **4.1.1** | Regresión lineal simple: el modelo, mínimos cuadrados | §3.1, §3.2 |
| **4.1.2** | R² vía SSRES/SSTOT; un R² alto NO garantiza un buen modelo | §3.8 |
| **4.1.3** | El intercepto β₀ y su (falta de) sentido físico | §3.3 |
| **4.1.4** | El coeficiente β₁: cuánto cambia Y por unidad de X | §3.4 |
| **4.1.5** | **MAE, MSE, RMSE** — con el RMSE como métrica principal | §3.7 |
| **4.2** | Regresión logística, sigmoide, probabilidades | §3.13 |
| **4.3** | Interpretación, residuos, **multicolinealidad**, supuestos | §3.6, §3.10–3.12 |

**Lo que agregamos por encima del contrato** (porque sin esto el contrato no se sostiene): correlación como punto de partida (§3.0), predicción y extrapolación (§3.5), el p-valor de la pendiente (§3.9), regresión múltiple (§3.10), heterocedasticidad (§3.12), matriz de confusión, precisión y sensibilidad (§3.14) y el umbral de decisión (§3.15).

> ⚠️ **Si estudiás del PDF oficial:** el capítulo tiene un error de edición — §4.1.2 y §4.1.3 llevan el **mismo título** ("Comprendiendo el R-cuadrado"), pero §4.1.3 trata en realidad del **intercepto**. No es que leíste mal: está repetido en el original.

## 2.5 El puente al Módulo 3: esto YA es machine learning

La regresión lineal que aprendés hoy **es un modelo de machine learning** — el primero, el más simple y el más usado del mundo. No es una metáfora:

| Palabra de esta clase | Cómo se dice en ML |
|---|---|
| Ajustar el modelo | **Entrenar** (train) |
| Variables X | **Features** (características) |
| Variable Y | **Target** (objetivo) |
| Regresión lineal | Regresión (aprendizaje **supervisado**) |
| Regresión logística | **Clasificación** (aprendizaje supervisado) |
| RMSE | La métrica de evaluación estándar |

El 15/08 arranca el Módulo 3 (Aplicación de Modelos de ML, con Dennis Delgado) y arranca **exactamente desde acá**: los mismos conceptos, más variables, más modelos y una pregunta nueva que hoy solo dejamos sembrada — *¿cuánto le erra el modelo a datos que nunca vio?* (§3.7).

Y un teaser más lejano: la **sigmoide** de la regresión logística (§3.13) es, matemáticamente, una neurona. Cuando en el Módulo 4 veas redes neuronales, vas a estar viendo miles de regresiones logísticas apiladas.

---
---

# PARTE 3 — Los conceptos, uno por uno

Esta es la sección de referencia. Cada concepto sigue la misma estructura: **qué es → para qué sirve → cómo se obtiene → ejemplo en la encuesta → errores comunes**.

**El escenario de todos los ejemplos:** `data_dev.csv`, la encuesta de Stack Overflow 2023 que ya conocés de la Clase 2 — 1.183 desarrolladores, con su sueldo anual (`salario`), sus años de experiencia profesional (`experiencia`), su experiencia laboral total (`exp_laboral`), sus años programando en total (`anios_codigo`) y si viven o no en Estados Unidos (`es_us`).

Dos datos de la variable estrella, para tener a mano desde el principio:

| `salario` (anual, en dólares) | Valor |
|---|---:|
| Media | **$90.684** |
| Mediana | **$72.714** |
| Mínimo | **$3** |
| Máximo | **$1.200.000** |

Ya con esto sabés dos cosas de la Clase 1: media > mediana → **cola derecha** (hay sueldos altísimos estirando el promedio), y los extremos son sospechosos — nadie programa un año entero por $3. Ese "dev fantasma de $3" va a reaparecer.

---

## 3.0 Prerrequisito: la correlación (el semáforo de entrada)

### Qué es

Una medida de la **relación lineal** entre dos variables numéricas, en un número entre −1 y +1. La viste en la Clase 1; acá es el semáforo que decide si vale la pena construir el modelo.

| \|r\| | Fuerza |
|---|---|
| 0,0 – 0,2 | Nula o muy débil |
| 0,2 – 0,4 | Débil |
| 0,4 – 0,6 | Moderada |
| 0,6 – 0,8 | Fuerte |
| 0,8 – 1,0 | Muy fuerte |

### Para qué sirve

- Es el **paso previo obligatorio a cualquier regresión**: si r ≈ 0, la recta que ajustes va a ser plana e inútil, y conviene saberlo *antes* de modelar.
- Anticipa el R²: en regresión simple, **r² = R², exactamente** (§3.8).

### Cómo se obtiene

```python
df[["salario", "experiencia"]].corr()
```

### Ejemplo en la encuesta

> **r = 0,2900** entre salario y experiencia profesional.

Débil, pero real. Y un adelanto que vas a ver confirmado en §3.8:

```
r² = 0,29² = 0,0841  ←  exactamente el R² del modelo
```

La correlación ya te estaba diciendo, antes de ajustar nada, que este modelo iba a explicar poco. **El semáforo estaba en amarillo y entramos igual — a propósito**, porque un modelo débil enseña más que uno perfecto.

### ⚠️ Errores comunes

1. **Creer que r mide toda relación.** Pearson solo detecta relaciones *lineales*: una curva perfecta en U puede dar r = 0.
2. **Saltarse el scatter.** El número sin el gráfico miente (§3.8, Anscombe).
3. **Leer causalidad.** r = 0,29 no dice quién causa a quién, ni si hay un tercero causando a ambos.

---

## 3.1 El modelo: una recta con nombre y apellido

### Qué es

La regresión lineal simple propone que la relación entre X e Y se puede describir con **una recta**:

```
   ŷ = β₀ + β₁ · x
    │     │    │
    │     │    └── la pendiente: cuánto sube ŷ por cada unidad de x
    │     └── el intercepto: dónde arranca la recta cuando x = 0
    └── "y sombrero": el valor PREDICHO (no el real)
```

La versión completa del modelo, la que escriben los libros, agrega el término que más importa:

```
   y = β₀ + β₁ · x + ε
                     │
                     └── el error: todo lo que la recta NO captura
```

Ese ε (épsilon) es la admisión de humildad del modelo: la recta no pretende explicar todo, solo la parte lineal. Lo que sobra —talento, industria, país, suerte— vive en ε.

### El diccionario de símbolos

| Símbolo | Se lee | Qué es |
|---|---|---|
| y | "y" | El valor **real** observado (el sueldo que la persona declaró) |
| ŷ | "y sombrero" | El valor **predicho** por la recta |
| β₀ | "beta cero" | Intercepto (§3.3) |
| β₁ | "beta uno" | Pendiente (§3.4) |
| ε | "épsilon" | El error del modelo; su versión medible es el **residuo** (§3.6) |

### Cómo se obtiene

```python
import statsmodels.formula.api as smf

modelo = smf.ols("salario ~ experiencia", data=df).fit()
modelo.summary()
```

La fórmula `"salario ~ experiencia"` se lee: *"salario explicado por experiencia"*. La virgulilla `~` es el "explicado por".

### Ejemplo en la encuesta

> **salario_estimado = 64.251 + 3.320 × experiencia**

Una ecuación de secundaria. Eso es todo el modelo: con la experiencia de cualquier persona, la recta te devuelve un sueldo estimado en dos operaciones.

### ⚠️ Errores comunes

1. **Confundir y con ŷ.** El modelo produce ŷ (estimaciones); los datos son y (realidad). La diferencia entre ambos es el residuo, y olvidarla es creer que el modelo "sabe".
2. **Creer que si la recta existe, la relación existe.** `smf.ols` te devuelve una recta **siempre**, hasta para ruido puro. Ajustar no es evidencia de nada; por eso existen el p-valor (§3.9) y el R² (§3.8).

---

## 3.2 Mínimos cuadrados: por qué ESA recta y no otra

### Qué es

Por la nube de 1.183 puntos pasan infinitas rectas posibles. **Mínimos cuadrados** (OLS, *Ordinary Least Squares*) es el criterio que elige una: la recta que hace **mínima la suma de los residuos al cuadrado**.

```
   Residuo de cada punto:   eᵢ = yᵢ − ŷᵢ     (real menos predicho)

   La recta elegida minimiza:   SSE = Σ (yᵢ − ŷᵢ)²
```

La imagen mental correcta: colgá cada punto de la recta con un resorte vertical. Los mínimos cuadrados encuentran la posición de la recta donde la energía total de los resortes es mínima. (Podés *jugar* con esta imagen en el simulador PhET de la bibliografía, §5.3.)

### ¿Por qué al cuadrado?

Las mismas dos razones que viste con la varianza en la Clase 1:

1. **Los signos se cancelan.** Si sumaras los residuos tal cual, los positivos y negativos se anularían y cualquier recta que pase por el medio daría cero.
2. **Castiga más los errores grandes.** Errarle $20.000 pesa 4 veces más que errarle $10.000, no 2. La recta resultante "le tiene miedo" a los errores groseros.

Y una tercera, matemática: la función cuadrática es derivable, así que el mínimo tiene **fórmula exacta y solución única**. No hay que buscar a tientas; se calcula de una.

### La conexión oculta con la correlación

La pendiente de mínimos cuadrados tiene una fórmula reveladora:

```
   β₁ = r · (sy / sx)      ←  la pendiente ES la correlación,
                               reescalada a las unidades de tus variables

   β₀ = ȳ − β₁ · x̄         ←  la recta pasa SIEMPRE por el punto (x̄, ȳ)
```

Dos consecuencias elegantes: si r = 0, la pendiente es 0 (recta plana: predecí la media y listo); y la recta siempre atraviesa el punto de los promedios — el centro de gravedad de la nube.

### Cómo se obtiene

No lo programás vos: `smf.ols(...).fit()` ejecuta mínimos cuadrados por dentro. El `.fit()` es, literalmente, "encontrá los β que minimizan la suma de cuadrados".

### ⚠️ Errores comunes

1. **Creer que es el único criterio posible.** Existen alternativas (minimizar valores absolutos, regresiones robustas); OLS es el estándar por sus propiedades matemáticas, no por decreto divino.
2. **Olvidar su talón de Aquiles: los outliers.** Como todo se eleva al cuadrado, un punto extremo tira de la recta con fuerza desproporcionada — el sueldo de $1.200.000 de nuestra encuesta arrastra la recta hacia arriba igual que el penthouse de $50.000 arrastraba la media en la Clase 1. Misma enfermedad, nueva víctima.

---

## 3.3 El intercepto β₀: dónde ancla la recta

### Qué es

El valor de ŷ cuando x = 0. Geométricamente, la altura a la que la recta cruza el eje vertical.

### Para qué sirve

Matemáticamente es imprescindible: sin β₀ la recta estaría obligada a pasar por el origen, y casi ninguna relación real pasa por el origen. **Interpretativamente, en cambio, es el coeficiente más sobrevalorado**: la mayoría de las veces no significa nada del mundo real.

### La regla de oro

> **β₀ solo se interpreta si x = 0 es un caso real, posible y observado en tus datos. Si no, es un anclaje matemático y nada más.**

### Cómo se obtiene

```python
modelo.params["Intercept"]     # β₀
```

### Ejemplo en la encuesta — y contraejemplo oficial

> **β₀ = $64.251**

En nuestro caso, x = 0 **sí** existe: hay gente con 0 años de experiencia profesional (los juniors). Entonces β₀ tiene lectura: *el modelo estima unos $64.251 anuales para quien recién empieza*. Interpretable — con la enorme cautela del error que veremos en §3.7.

**El contraejemplo del material oficial del curso:** en `real_estate_price_size.csv` (precio de casas según superficie), β₀ sería "el precio de una casa de **0 m²**". Un terreno sin casa, valuado por una recta. Ahí β₀ **no tiene sentido físico**: es solo el punto donde la recta corta el eje. Eso es exactamente lo que el capítulo (§4.1.3) quiere que entiendas.

| Dataset | x = 0 significa... | ¿β₀ interpretable? |
|---|---|---|
| `data_dev.csv` | 0 años de experiencia (junior) | ✅ Sí, existe gente así |
| `real_estate_price_size.csv` | Casa de 0 m² | ❌ No existe tal cosa |
| `student_scores.csv` (tu TP) | 0 horas de estudio | ✅ Plausible (y deprimente) |

### ⚠️ Errores comunes

1. **Interpretar β₀ cuando x = 0 está fuera del rango de los datos.** Es extrapolación disfrazada de coeficiente (§3.5).
2. **Forzar la recta a pasar por el origen** ("sin intercepto") porque "suena lógico". Cambia todo el ajuste y rompe la interpretación estándar del R². No lo hagas salvo que tengas una razón física contundente.

---

## 3.4 La pendiente β₁: el corazón del modelo

### Qué es

**Cuánto cambia ŷ por cada unidad adicional de x.** Es el número que condensa toda la relación; si la regresión simple tuviera que reducirse a un solo valor, sería este.

Sus unidades son siempre **[unidades de Y] por [unidad de X]** — en nuestro caso, dólares por año.

### Para qué sirve

- Es la **respuesta cuantitativa** a "¿cómo se relacionan X e Y?": no solo "se relacionan", sino *a razón de cuánto*.
- Su signo da la dirección (positiva: a más X, más Y).
- Es el coeficiente sobre el que se hace la prueba de hipótesis central del modelo (§3.9).

### Cómo se obtiene

```python
modelo.params["experiencia"]   # β₁
```

### Ejemplo en la encuesta

> **β₁ = $3.320 por año**

Lectura correcta, palabra por palabra: *cada año adicional de experiencia profesional **se asocia** con unos $3.320 más de sueldo anual, en promedio*. Una década de experiencia se asocia con unos $33.200 más al año.

Fijate en el verbo. No dijimos "cada año te sube el sueldo $3.320". Dijimos **"se asocia"**: al comparar personas con un año más de experiencia, en promedio ganan $3.320 más. La diferencia entre esas dos frases es la diferencia entre asociación y causalidad.

### ⚠️ Errores comunes

1. **Leerlo causal.** "Quedate un año más y ganás $3.320 más" — el modelo no prometió eso. Quizás los que llevan más años están en industrias que pagan mejor, o en países más caros. La recta no distingue.
2. **Ignorar las unidades.** Un β₁ de 3.320 es enorme si Y está en dólares anuales y ridículo si estuviera en pesos por hora. La pendiente sin unidades no dice nada.
3. **Comparar pendientes de variables con escalas distintas** para decidir "cuál importa más". Para eso hay que estandarizar — y aun así, con cuidado (§3.11).

---

## 3.5 La predicción — y su trampa: la extrapolación

### Qué es

Usar la ecuación ajustada para estimar Y en un valor concreto de X. Es el momento para el que se construyó todo lo demás.

### Cómo se obtiene

```python
import pandas as pd

modelo.predict(pd.DataFrame({"experiencia": [5]}))
```

### Ejemplo en la encuesta: la pregunta ancla, respondida (versión ingenua)

> ### ¿Cuánto va a ganar un programador con 5 años de experiencia?
>
> **ŷ(5) = β₀ + β₁ × 5 ≈ $64.251 + $16.600 ≈ $80.853**

(Los coeficientes redondeados dan $80.851; el modelo con sus decimales completos da **$80.853**. La diferencia son centavos de redondeo — citá $80.853, que es lo que devuelve el código.)

Ochenta mil ochocientos cincuenta y tres dólares. Un número concreto, con olor a respuesta profesional.

**Todavía no festejes.** Esta es la respuesta *ingenua*: un número pelado, sin su error al lado. En §3.7 la volvemos a dar en versión honesta, y no te va a gustar.

### La trampa: extrapolar

El modelo se entrenó con experiencias entre **0 y 50 años**. Dentro de ese rango, la recta interpola: se apoya en datos reales que la rodean. Fuera de ese rango, la recta sigue dibujada —las rectas son infinitas— pero ya no se apoya en **nada**.

La demostración con nuestro modelo:

> **¿Cuánto gana un programador con 80 años de experiencia?**
> La recta contesta sin sonrojarse: **$329.874**.

Una persona que programa desde hace 80 años tendría unos cien años de edad y sería, según el modelo, la mejor pagada de su carrera. La recta no sabe de jubilaciones, de salud, ni de que COBOL ya no se cotiza igual: solo sabe prolongar una tendencia que aprendió entre 0 y 50.

> ## ⚠️ La regla
> ### **Predecí solo dentro del rango de X que el modelo vio.**
> ### El modelo devuelve un número para CUALQUIER x que le pongas, sin advertencia, sin error, sin sonrojo. El freno no viene en el software: el freno sos vos.

### ⚠️ Errores comunes

1. **Extrapolar sin darse cuenta** — el caso más común es predecir para un x "apenas" fuera del rango. Apenas afuera sigue siendo afuera.
2. **Reportar la predicción sin el rango de validez.** "El modelo estima $80.853 para 5 años de experiencia *(válido para 0–50 años)*" es la forma completa.
3. **Olvidar que la predicción es un promedio condicional.** ŷ(5) no es "lo que va a ganar Juan": es el promedio estimado de *todos* los que tienen 5 años. Juan puede estar lejísimos (§3.7).

---

## 3.6 Los residuos: lo que el modelo no ve

### Qué es

El **residuo** de cada observación es la diferencia entre lo que pasó y lo que el modelo dijo que pasaría:

```
   eᵢ = yᵢ − ŷᵢ        (real − predicho)

   eᵢ > 0  →  el modelo se quedó corto (la persona gana MÁS de lo predicho)
   eᵢ < 0  →  el modelo se pasó       (la persona gana MENOS de lo predicho)
```

Es la versión medible del ε del modelo teórico (§3.1). Si la recta es lo que el modelo entendió, los residuos son **todo lo que no entendió**.

### Para qué sirve

- Son la materia prima de todas las métricas de error (§3.7): MAE, MSE y RMSE son resúmenes de los residuos.
- Son el **instrumento de diagnóstico** del modelo: sus gráficos revelan problemas que ningún número resume (§3.12).
- Aplican todo lo de la Clase 1: los residuos son una variable más, con su histograma, su asimetría y sus outliers.

### Cómo se obtienen

```python
residuos = modelo.resid              # un residuo por encuestado
predichos = modelo.fittedvalues     # los ŷ de cada uno

plt.hist(residuos, bins=50)                # ¿qué forma tienen?
plt.scatter(predichos, residuos, s=8)      # ¿queda algún patrón?
plt.axhline(0, color="red")
```

### Ejemplo en la encuesta

| Estadístico de los residuos | Valor | Lectura |
|---|---:|---|
| **Media** | **0** (exactamente) | No dice NADA del modelo — ver la advertencia |
| Mínimo | **−$150.562** | A alguien el modelo le atribuyó $150.562 de más |
| Máximo | **+$1.019.539** | Alguien gana **un millón de dólares más** de lo que la recta predijo |
| Asimetría | **+4,16** | Cola derecha brutal — peor que el precio de Airbnb (+2,34) de la Clase 1 |

Ese máximo merece un segundo de silencio: hay una persona cuyo sueldo real está **$1.019.539 por encima** de su predicción. La recta pasó por abajo como si nada. Los residuos heredaron la cola derecha del salario, amplificada.

### ⚠️ Errores comunes

1. **"La media de los residuos es 0, así que el modelo es bueno."** La media de los residuos de OLS es 0 **siempre, por construcción matemática** — en el mejor modelo del mundo y en el peor. Es como felicitar a una balanza por marcar 0 sin nada encima.
2. **Mirar solo números y no los gráficos.** Un histograma de residuos y un scatter de residuos vs. predichos cuestan dos líneas y revelan lo que la tabla esconde (§3.12).
3. **No perseguir los residuos gigantes.** Un residuo de +$1.019.539 es una invitación a investigar esa fila: ¿dato real excepcional o error de carga? (La lección de outliers de la Clase 1, intacta.)

---

## 3.7 MAE, MSE y RMSE: cuánto le erra el modelo

### Qué son

Tres formas de responder la única pregunta que le importa a quien va a usar tus predicciones: **¿cuánto le erra, típicamente?** Las tres resumen los residuos; difieren en cómo los promedian.

```
              Σ |eᵢ|
   MAE  =  ──────────        el promedio de los errores, en valor absoluto
                n

              Σ eᵢ²
   MSE  =  ──────────        el promedio de los errores AL CUADRADO
                n

   RMSE =  √MSE              la raíz del MSE: vuelve a las unidades originales
```

| Métrica | Unidades | ¿Castiga errores grandes? | Uso |
|---|---|---|---|
| **MAE** | Las de Y (dólares) | No: todos pesan igual | La más fácil de explicar a un gerente |
| **MSE** | Y al cuadrado (dólares²) | Sí, al cuadrado | Paso intermedio; no se reporta |
| **RMSE** | Las de Y (dólares) | Sí (hereda del MSE) | **La métrica estándar** |

¿Te suena la historia del MSE? Es el mismo drama de la varianza en la Clase 1: unidades al cuadrado que nadie puede interpretar, resueltas con una raíz. Varianza → desviación estándar; MSE → RMSE. **Mismo truco, mismo motivo.**

El capítulo del curso recomienda el **RMSE como métrica principal**, y no está solo: Bruce y Bruce (p. 153) lo llaman *"la métrica más importante en ciencia de datos"*. ¿Por qué el RMSE y no el MAE? Porque está en las unidades de Y (interpretable) **y además** castiga los errores groseros — que suelen ser los que te cuestan plata.

### Cómo se obtienen

```python
from sklearn.metrics import mean_absolute_error, mean_squared_error
import numpy as np

real = df_modelo["salario"]
pred = modelo.fittedvalues

mae  = mean_absolute_error(real, pred)   # error absoluto promedio
mse  = mean_squared_error(real, pred)    # error cuadrático promedio
rmse = np.sqrt(mse)                      # raíz → vuelve a estar en dólares
```

### Ejemplo en la encuesta: el clímax de la clase

| Métrica | Valor |
|---|---:|
| MAE | **$50.794** |
| **RMSE** | **$78.343** |

Notá que RMSE > MAE, y por mucho. Esa brecha es la firma de los outliers: los errores gigantes (como el de +$1.019.539) pesan al cuadrado en el RMSE y solo lineal en el MAE. Cuando veas un RMSE muy por encima del MAE, ya sabés que hay errores groseros en la cola.

Ahora, el número en contexto — el momento más importante de la clase:

```
   RMSE            $78.343
   ──────────  =  ─────────  ≈  86%
   sueldo medio    $90.684
```

> ## ⚠️ El modelo le erra, típicamente, el 86% del sueldo promedio.

Y con eso, la pregunta ancla en su **versión honesta**:

> ### ¿Cuánto va a ganar un programador con 5 años de experiencia?
>
> **$80.853 ± $78.343** — es decir, algo entre **$2.510 y $159.195** al año.
>
> Entre una llanta usada y un penthouse en Equipetrol. Esa es la predicción real del modelo, dicha completa.

La primera respuesta ($80.853, §3.5) sonaba profesional. Esta suena ridícula. **Las dos salen del mismo modelo** — la diferencia es que esta incluye el error, y aquella lo escondía. Un intervalo de ±1 RMSE alrededor de la predicción es la manera mínima y honesta de reportar; si el intervalo resultante no sirve para decidir nada, el modelo no sirve para decidir nada, tenga el p-valor que tenga.

### ⚠️ Errores comunes

1. **Reportar el MSE como si fuera interpretable.** "El error es 6 mil millones de dólares cuadrados" no significa nada. Sacá la raíz.
2. **Comparar RMSE entre datasets de escalas distintas.** Un RMSE de $78.343 es enorme para sueldos de $90.684 y despreciable para precios de casas de un millón. Por eso el cociente RMSE/media (86%) comunica mejor que el RMSE pelado.
3. **Medir el error sobre los mismos datos con los que se ajustó el modelo y creer que eso es el rendimiento futuro.** Nuestro 86% es el error *sobre los datos de entrenamiento* — con datos nuevos suele ser peor. La solución (separar train/test) es la primera lección del Módulo 3.

---

## 3.8 El R²: qué proporción de la historia explica el modelo

### Qué es

El **coeficiente de determinación**: la proporción de la variación de Y que el modelo logra explicar. Va de 0 (no explica nada) a 1 (explica todo).

Su fórmula lo dice todo si se lee con calma:

```
              SSRES                Σ (yᵢ − ŷᵢ)²     ← lo que el modelo NO explica
   R²  =  1 − ──────      SSRES = 
              SSTOT                Σ (yᵢ − ȳ)²      ← la variación total de Y
                          SSTOT =
```

**SSTOT** es la suma de cuadrados alrededor de la media: cuánto varía Y por sí sola. Es, en el fondo, el error del "modelo tonto" — el que ignora X y predice la media ($90.684) para todo el mundo.

**SSRES** es la suma de cuadrados de los residuos: cuánto error queda *después* de usar la recta.

El R² compara ambos: **¿cuánto mejor que el modelo tonto es tu recta?** Si SSRES = SSTOT (la recta no mejoró nada), R² = 0. Si SSRES = 0 (predicción perfecta), R² = 1.

### La conexión con la correlación

En la regresión **simple**, R² es exactamente el cuadrado de r:

```
   r = 0,2900   →   R² = 0,29² = 0,0841
```

El mismo número por dos caminos distintos. Verificarlo en el notebook es un segundo y es la mejor vacuna contra la sensación de que R² "sale de una caja negra".

### Cómo se obtiene

```python
modelo.rsquared        # statsmodels
# o en el summary(): fila "R-squared"
```

### Ejemplo en la encuesta

> **R² = 0,0841** — el modelo explica el **8,4%** de la variación de los sueldos. El **91,6%** restante ocurre por cosas que la experiencia no captura: país, industria, empresa, rol, suerte.

¿Es un desastre? Acá viene la parte que casi nadie enseña. ISLR (p. 70) lo dice explícitamente: en dominios ruidosos —y pocos hay más ruidosos que los sueldos globales— *"un R² bien por debajo de 0,1 puede ser más realista"*. Un R² de 0,08 con 1.183 humanos reales no es un análisis fracasado: **es un dominio difícil medido con honestidad**.

El espectro completo, con los datasets del material oficial del curso:

| Dataset | R² | Mundo |
|---|---:|---|
| `student_scores.csv` (tu TP: horas de estudio → nota, 25 filas) | **0,953** | De laboratorio: una causa domina |
| `real_estate_price_size.csv` (m² → precio) | **0,745** | Ordenado: los m² mandan, pero no solos |
| `data_dev.csv` (experiencia → salario, 1.183 humanos) | **0,0841** | Real: mil factores, uno solo medido |

💡 Cuando el domingo hagas el TP y te salga un R² de 0,95, no concluyas que sos mejor modelador que hoy: concluí que te dieron un dataset de juguete. **El R² habla del dominio tanto como del modelo.**

### ⚠️ Un R² alto NO garantiza un buen modelo

Esta es la advertencia central del capítulo (§4.1.2), y tiene demostración clásica: el **cuarteto de Anscombe** (1973). Cuatro datasets construidos a propósito: en los cuatro, la regresión da prácticamente la misma recta y el mismo **R² ≈ 0,67**. Pero al graficarlos:

```
   I:  nube lineal legítima        ✅ la regresión corresponde
  II:  una CURVA perfecta          ❌ el modelo correcto no es una recta
 III:  una línea + UN outlier      ❌ un solo punto torció la recta
  IV:  una vertical + UN punto     ❌ la "relación" es un punto solo
```

Cuatro gráficos que piden modelos distintos, un solo R² que los declara iguales. **El R² no ve la forma; solo el scatter la ve.**

```python
import seaborn as sns
ans = sns.load_dataset("anscombe")     # los 4 grupos de Anscombe
for g in ["I", "II", "III", "IV"]:
    grupo = ans[ans["dataset"] == g]
    r2 = smf.ols("y ~ x", data=grupo).fit().rsquared
    print(g, round(r2, 2))             # ≈ 0.67 los cuatro
```

### ⚠️ Errores comunes

1. **"R² bajo = análisis fracasado."** Falso: depende del dominio (ISLR). Un R² de 0,08 sobre sueldos globales informa de verdad; uno de 0,95 en un dataset de juguete no te enseña nada del mundo.
2. **"R² alto = modelo correcto."** Falso: Anscombe II, III y IV. Sin el gráfico, el número es un rumor.
3. **Usar el R² para comparar modelos con distinta Y** (por ejemplo, salario vs. log(salario)). Cambia la escala de SSTOT y la comparación pierde sentido.
4. **Inflar el R² agregando variables.** El R² **siempre** sube al agregar predictores, aunque sean ruido (§3.10). Subió ≠ mejoró.

---

## 3.9 El p-valor de la pendiente: ¿la relación existe?

### Qué es

La prueba de hipótesis de la Clase 2, aplicada dentro de la regresión. La hipótesis nula es:

```
   H₀ :  β₁ = 0      "la experiencia no aporta nada; la recta verdadera es plana"
```

El p-valor responde: *si la experiencia no tuviera ninguna relación con el salario, ¿qué tan probable sería ver una pendiente como la nuestra solo por azar del muestreo?*

### Cómo se obtiene

```python
modelo.pvalues["experiencia"]
# también aparece en summary(), columna "P>|t|"
```

### Ejemplo en la encuesta

> **p = 2,32 × 10⁻²⁴**

Un 2 con veintitrés ceros antes. La probabilidad de que una pendiente de $3.320/año apareciera por azar, sin relación real, es tan ridículamente chica que la respuesta a la pregunta 1 de §1.3 es un **SÍ rotundo: la relación existe**.

**El puente con la Clase 2, cerrado:** el sábado pasado la maquinaria de hipótesis le dijo *no* a una diferencia (p = 0,3529). Hoy le dice *sí* a una pendiente (p = 2,32 × 10⁻²⁴). Misma maquinaria, otra pregunta — y otra trampa de intuición:

> ## ⚠️ Significativo ≠ útil (la demostración completa)
>
> El mismo modelo tiene **p = 2,32 × 10⁻²⁴** (la relación existe, indiscutible) y **R² = 0,0841 con error típico del 86%** (la relación es casi inservible para predecir). Las dos cosas son verdad a la vez, porque responden preguntas distintas:
>
> - El **p-valor** pregunta: ¿esto es azar? — *No.*
> - El **R²/RMSE** preguntan: ¿esto sirve? — *Casi no.*

La American Statistical Association lo dejó por escrito en su declaración de 2016 (principio 5): *"un p-valor no mide el tamaño de un efecto ni la importancia de un resultado"*. Con n = 1.183, hasta un efecto chico se detecta con p microscópico: **el p-valor premia la certeza de que el efecto existe, no su tamaño**.

Y la pinza se cierra por el otro lado: tampoco un R² alto con p chico garantiza nada — hay correlaciones espurias célebres entre series que no tienen nada que ver (hay un sitio entero dedicado a coleccionarlas, tylervigen.com). Ningún número reemplaza a la pregunta *"¿tiene sentido este modelo?"*.

### ⚠️ Errores comunes

1. **"p chiquito → modelo bueno."** No: p chiquito → pendiente distinta de cero. Nuestro modelo es la prueba viviente.
2. **"p chiquito → efecto grande."** No: con muestras grandes, efectos minúsculos dan p ínfimos. El tamaño del efecto lo da β₁ con sus unidades, no p.
3. **Olvidar que el p-valor depende de n.** El mismo r = 0,29 con 30 encuestados no habría dado un p ni parecido.

---

## 3.10 La regresión múltiple: más de una X

### Qué es

La extensión natural: en vez de una variable explicativa, varias.

```
   ŷ = β₀ + β₁·x₁ + β₂·x₂ + β₃·x₃ + ...
```

Cada βᵢ se interpreta ahora como: *cuánto cambia ŷ por unidad de xᵢ, **manteniendo las demás variables constantes***. Esa cláusula final —*ceteris paribus*, para los amigos economistas— es la que va a estallar en §3.11.

### Cómo se obtiene

```python
modelo_m = smf.ols("salario ~ experiencia + exp_laboral + anios_codigo",
                   data=df).fit()
modelo_m.summary()
```

La fórmula crece con `+`: nada más cambia en el código.

### Ejemplo en la encuesta

Agregamos a la experiencia profesional otras dos variables: la experiencia laboral total (`exp_laboral`) y los años totales programando (`anios_codigo`).

| Modelo | R² |
|---|---:|
| Simple (`experiencia`) | 0,0841 |
| Múltiple (tres variables) | **0,0900** |

¿Mejoró? Subió de 8,4% a 9,0%. Pero antes de celebrar, el dato que arruina la fiesta — Bruce y Bruce, p. 156: **el R² sube SIEMPRE que agregás variables, aunque sean puro ruido**. Sumar columnas no puede empeorar el ajuste sobre los datos ya vistos; en el peor caso el modelo les asigna casi cero. Así que "subió el R²" con más variables **no es evidencia de nada**: era inevitable.

Para eso existe el **R² ajustado** (`Adj. R-squared` en el summary): una versión penalizada que solo sube si la variable nueva aporta más que su costo. Cuando compares modelos con distinta cantidad de variables, mirá ese.

Y lo verdaderamente grave del modelo múltiple no está en el R². Está en lo que le pasó a los p-valores — sección siguiente.

### ⚠️ Errores comunes

1. **Meter variables a lo loco "porque el R² sube".** Sube siempre; no significa nada (Bruce p. 156). Usá el R² ajustado.
2. **Interpretar cada β sin revisar si las X están correlacionadas entre sí.** El "manteniendo lo demás constante" exige que "lo demás" pueda quedarse constante — con predictores gemelos, no puede (§3.11).
3. **Creer que más variables = más ciencia.** Un modelo simple que entendés le gana a uno complejo que no podés explicar.

---

## 3.11 La multicolinealidad: el problema de los gemelos

### Qué es

Ocurre cuando dos o más **predictores están fuertemente correlacionados entre sí** — llevan casi la misma información. El modelo múltiple tiene que repartir el crédito de un efecto entre variables casi idénticas, y no puede decidir a cuál dárselo.

Nuestras tres variables son un caso de manual — miralas de nuevo: años de experiencia profesional, años de experiencia laboral, años programando. **Son tres relojes midiendo casi el mismo tiempo.** Sus correlaciones entre sí:

| Par de predictores | r |
|---|---:|
| experiencia ↔ exp_laboral | **0,795** |
| experiencia ↔ anios_codigo | **0,864** |
| exp_laboral ↔ anios_codigo | **0,891** |

### El síntoma dramático (el "antes y después" de la clase)

| El p-valor de `experiencia`... | Valor |
|---|---:|
| ...sola en el modelo simple | **2,3 × 10⁻²⁴** |
| ...acompañada de sus gemelas | **0,174** |

La variable estrella de la clase —la de la pendiente indiscutible— quedó **"no significativa"** de un plumazo. ¿La experiencia dejó de importar? **No.** Lo que pasa es que la pregunta que responde el p-valor en el modelo múltiple es otra: *"¿aporta `experiencia` algo que `exp_laboral` y `anios_codigo` no aporten ya?"*. Y la respuesta honesta es: casi nada — porque son la misma información tres veces.

Técnicamente: la colinealidad **infla los errores estándar** de los coeficientes. El modelo sigue prediciendo igual de bien (el R² global no sufre), pero ya no puede atribuir el efecto a una variable en particular. La colinealidad no rompe la **predicción**; rompe la **interpretación**.

### Cómo se detecta

```python
# 1. La matriz de correlaciones entre predictores (el método visual)
df[["experiencia", "exp_laboral", "anios_codigo"]].corr()

# 2. El VIF (factor de inflación de la varianza), el método formal:
#    VIF ≈ 1 → sin problema · VIF > 5 → alerta · VIF > 10 → colinealidad seria
from statsmodels.stats.outliers_influence import variance_inflation_factor
```

### Qué se hace al respecto

| Opción | Cuándo |
|---|---|
| **Quedarte con una** de las gemelas (la de mejor lectura) | Casi siempre la respuesta correcta acá |
| Combinarlas en un índice único | Si todas aportan matices |
| Regularización (ridge/lasso) | Módulo 3 — lo van a ver con Dennis |
| No hacer nada | Si SOLO te importa predecir y jamás interpretar β |

### ⚠️ Errores comunes

1. **Descartar una variable "porque dio p > 0,05" en la múltiple**, sin sospechar colinealidad. El p-valor alto no dice "no importa"; puede decir "importa, pero su gemela ya lo dijo".
2. **Sumar variantes de la misma medida creyendo que "más features ayudan".** Tres termómetros pegados no miden mejor la fiebre.
3. **Confundir multicolinealidad con confusión (confounding).** Colinealidad: dos X redundantes entre sí. Confusión: una tercera variable causa a X y a Y. Se tratan distinto.

---

## 3.12 Los supuestos del modelo lineal (y la heterocedasticidad)

### Qué son

La regresión no es solo una recta: es una recta **más un contrato**. Los p-valores y los intervalos que reporta el summary son válidos si los residuos cumplen ciertas condiciones. El contrato tiene cuatro cláusulas — en inglés arman el acrónimo **LINE**:

| Supuesto | Qué exige | Cómo se verifica | Si falla... |
|---|---|---|---|
| **L**inealidad | La relación real es una recta | Scatter de X vs. Y; residuos vs. predichos **sin curva** | El modelo entero apunta mal (Anscombe II) |
| **I**ndependencia | Cada observación no depende de las otras | Conocer el diseño de los datos (¿hay repetidos? ¿series de tiempo?) | p-valores demasiado optimistas |
| Homocedasticidad (**N**ivel constante de varianza) | La dispersión del error es pareja a lo largo de X | Residuos vs. predichos **sin forma de embudo** | Los errores estándar quedan mal calculados |
| **E**rror normal | Los residuos siguen una distribución normal | Histograma de residuos | Los p-valores e intervalos exactos pierden precisión |

### La heterocedasticidad, en criollo

Es la violación de la tercera cláusula: **la dispersión del error crece (o cambia) con el nivel de X**. Su firma visual es el **embudo**: residuos apretados a la izquierda del gráfico y desparramados a la derecha.

Ya la conocés sin saberlo: en la Clase 1 vimos que las *tiny homes* tenían precios apretados (desviación 48) y las *homes* precios salvajes (desviación 187). Los sueldos hacen lo mismo: entre juniors, los sueldos se parecen; entre seniors, conviven el que gana $40.000 y el que gana $700.000. **A más nivel, más dispersión** — el embudo clásico de las variables de dinero.

### Nuestro modelo, contra el contrato

| Cláusula | ¿La cumplimos? |
|---|---|
| Linealidad | Aceptable — la nube no muestra una curva clara |
| Independencia | Razonable — encuestados distintos, sin duplicados |
| Homocedasticidad | **Dudosa** — sueldos: dispersión creciente, el embudo de manual |
| Normalidad de residuos | **Violada sin disimulo** — asimetría de +4,16, residuos de hasta +$1.019.539 |

¿Y entonces, todo lo que hicimos fue en vano? No. Con n = 1.183, el Teorema Central del Límite (Clase 2) le da bastante robustez a los coeficientes, y la **predicción puntual** no depende de la normalidad. Pero los p-valores e intervalos finos hay que tomarlos como aproximaciones, y decirlo. Los remedios estándar —transformar a log(salario), errores estándar robustos— quedan mencionados para tu siguiente curso.

### ⚠️ Errores comunes

1. **Verificar la normalidad de Y en vez de la de los RESIDUOS.** El contrato es sobre los errores del modelo, no sobre la variable original. Es el error clásico del examen.
2. **Ajustar, reportar y jamás mirar un gráfico de residuos.** Dos líneas de matplotlib te muestran el embudo y la curva; ninguna tabla lo hace.
3. **Tratar los supuestos como trámite religioso.** Son el aviso de *qué parte* de la salida podés creerte: con residuos feos, desconfiá de los intervalos antes que de la recta.

---

## 3.13 La regresión logística: cuando Y es sí o no

### Qué es

El modelo para variables dependientes **binarias**: ¿es de Estados Unidos o no? ¿paga o no paga? ¿spam o no spam?

**¿Por qué no usar la recta de siempre?** Probalo mentalmente: codificá "es de EE. UU." como 1 y "no" como 0, y ajustá una recta. La recta, que es infinita, devuelve **3,35** para el sueldo más alto del archivo y cruza el cero por abajo. ¿Una probabilidad de 335%? ¿Negativa? Es el mismo absurdo de la Clase 1, cuando el modelo normal le asignaba 9,9% de probabilidad a precios negativos: **el modelo equivocado opina con total confianza fuera del rango de lo posible.**

La solución es pasar la recta por una función que la doble y la encierre entre 0 y 1: la **sigmoide** (o función logística).

```
                        1
   p(x)  =  ─────────────────────────
             1 + e^−(β₀ + β₁·x)

      1 ┤                    ●●●●●●──────
        │                ●●
    0,5 ┤ · · · · · · ●· · · · · · · · ·
        │           ●
      0 ┤──────●●●●
        └────────────────────────────────→ x
```

Adentro del paréntesis vive **la misma recta de toda la clase** (β₀ + β₁·x). La sigmoide solo la traduce a probabilidad: valores muy negativos → p cerca de 0; muy positivos → p cerca de 1; y en el medio, la zona de duda. Por eso se llama *regresión* logística aunque en ML se use para *clasificar*.

### Para qué sirve

- Predice **probabilidades**, no números: la salida de la logística es "p = 0,83 de que sea de EE. UU.", no "0,83 EE. UU.".
- Es el modelo de clasificación más usado del mundo real (crédito, churn, diagnóstico, spam).
- Es la puerta de entrada a la clasificación del Módulo 3 — y su función sigmoide es la neurona del Módulo 4.

### Cómo se obtiene

```python
datos_log = df.dropna(subset=["salario", "us_or_not"]).copy()
datos_log["es_us"] = (datos_log["us_or_not"] == "US").astype(int)   # 1 = sí, 0 = no

modelo_log = smf.logit("es_us ~ salario", data=datos_log).fit()
prob = modelo_log.predict(datos_log)        # devuelve PROBABILIDADES entre 0 y 1
```

⚠️ **Detalle de herramienta que te va a ahorrar una confusión:** `sklearn.linear_model.LogisticRegression` aplica **regularización por defecto** — sus coeficientes salen corridos respecto de los de `smf.logit`, a propósito. Para clasificar a escala está muy bien (Módulo 3); para *leer* coeficientes, usá `smf.logit`.

### Ejemplo en la encuesta

Modelo: `es_us ~ salario` — ¿el sueldo delata si alguien vive en EE. UU.? (Spoiler: bastante — los sueldos de EE. UU. son otro planeta.)

| Medida | Valor |
|---|---:|
| Exactitud del modelo | **84,8%** |
| Exactitud del "modelo tonto" (decir siempre "Not US") | **74,7%** |

La segunda fila es obligatoria. Solo 1 de cada 4 encuestados vive en EE. UU. (299 de 1.183, un 25,3%), así que un modelo que responde **siempre "Not US"** sin mirar nada acierta el 74,7% de las veces. Nuestro 84,8% hay que leerlo contra ese piso: le gana por 10 puntos. Bien — pero "84,8%" a secas, sin la base al lado, habría sonado a genialidad. **Toda exactitud se reporta contra su base.**

### ⚠️ Errores comunes

1. **Leer la salida como categoría directa.** El modelo devuelve probabilidades; convertirlas en sí/no exige elegir un umbral (§3.15).
2. **Reportar exactitud sin la base de comparación.** Con clases desbalanceadas, la exactitud pelada infla cualquier modelo (§3.14).
3. **Usar regresión lineal para una Y binaria** "porque total, son números". Probabilidades de 335% y negativas.
4. **Ajustar con sklearn y comparar coeficientes con statsmodels** sin saber de la regularización. No es un bug: son filosofías distintas.

---

## 3.14 La matriz de confusión: en qué se equivoca, exactamente

### Qué es

La tabla que cruza **realidad contra predicción**. La exactitud comprime el rendimiento en un número; la matriz muestra **los cuatro destinos posibles** de cada caso — y sobre todo, los dos tipos distintos de error.

### Ejemplo en la encuesta

```python
from sklearn.metrics import confusion_matrix

pred_clase = (prob >= 0.5).astype(int)          # umbral 0,5 (ver §3.15)
confusion_matrix(datos_log["es_us"], pred_clase)
```

|  | Predijo "Not US" | Predijo "US" | Total real |
|---|---:|---:|---:|
| **Realidad: Not US** | **838** ✅ | 46 ❌ | 884 |
| **Realidad: US** | 134 ❌ | **165** ✅ | 299 |

Lectura celda por celda:

- **838** — verdaderos negativos: no eran de EE. UU. y el modelo lo dijo bien.
- **46** — falsos positivos: el modelo los mandó a EE. UU. sin serlo.
- **134** — falsos negativos: **eran** de EE. UU. y el modelo no los vio.
- **165** — verdaderos positivos: eran de EE. UU. y el modelo los encontró.

### Las métricas que salen de la matriz

| Métrica | Pregunta que responde | Cálculo con nuestra matriz | Valor |
|---|---|---|---:|
| **Exactitud** | ¿Qué fracción del total acerté? | (838 + 165) / 1.183 | **84,8%** |
| **Precisión** | De los que declaré "US", ¿cuántos lo eran? | 165 / (165 + 46) | **≈ 78,2%** |
| **Sensibilidad** (recall) | De los US reales, ¿cuántos encontré? | 165 / (165 + 134) | **≈ 55,2%** |

Y acá está la revelación que la exactitud escondía: **el modelo se pierde casi la mitad de los estadounidenses** (sensibilidad 55,2%). Su 84,8% de exactitud vive, en buena parte, de acertar la clase fácil y mayoritaria ("Not US"). La matriz no deja mentir al promedio — que es exactamente la lección de la Clase 1, reencarnada: un resumen puede esconder lo que importa.

### ⚠️ Errores comunes

1. **Quedarse solo con la exactitud.** Con clases desbalanceadas (74,7% de una sola clase), es la métrica más fácil de inflar y la menos informativa.
2. **Confundir precisión con sensibilidad.** Precisión: pureza de lo que declaraste positivo. Sensibilidad: cobertura de los positivos reales. Se parecen de nombre y responden preguntas opuestas.
3. **No preguntarse cuál de los dos errores cuesta más caro.** 46 falsos positivos y 134 falsos negativos no valen lo mismo en ningún negocio real — y decidir cuál duele más no es estadística: es el negocio (§3.15).

---

## 3.15 El umbral de decisión: dónde cortás la probabilidad

### Qué es

La logística devuelve probabilidades (0,83; 0,41; 0,07…). Para decidir, hay que convertirlas en sí/no con un **umbral de corte**: por defecto, 0,5 — probabilidad mayor a 0,5, lo declaro "US".

Pero 0,5 no es una ley: **es apenas el valor por defecto**. Y moverlo cambia la matriz de confusión entera:

| Si movés el umbral... | Ganás | Pagás |
|---|---|---|
| **Hacia abajo** (p. ej. 0,3): declarás "US" con menos evidencia | Más sensibilidad: encontrás más US reales | Más falsos positivos: más "Not US" mal etiquetados |
| **Hacia arriba** (p. ej. 0,7): exigís más evidencia | Más precisión: tus "US" son más confiables | Más falsos negativos: se te escapan US reales |

**Precisión y sensibilidad son una frazada corta**: tapás una punta, se destapa la otra. El umbral decide qué punta preferís destapar.

### El umbral es una decisión de negocio, no de estadística

- **Detección de fraude en un banco de Santa Cruz:** un fraude no detectado cuesta muchísimo más que revisar una alerta de más → umbral bajo, sensibilidad alta, y que el equipo revise falsas alarmas.
- **Tamizaje médico:** que no se escape un enfermo → sensibilidad primero; el falso positivo se resuelve con una segunda prueba.
- **Filtro de spam:** mandar un correo legítimo del jefe a spam es carísimo → precisión primero, umbral alto.

El modelo pone las probabilidades; **el costo de cada error lo pone tu problema**. Dos empresas con el mismo modelo pueden operar con umbrales distintos, y las dos tener razón.

### Cómo se explora

```python
for umbral in [0.3, 0.5, 0.7]:
    pred_u = (prob >= umbral).astype(int)
    print(umbral, confusion_matrix(datos_log["es_us"], pred_u).tolist())
```

En el Módulo 3 esto se sistematiza con las curvas ROC y precisión-recall — la herramienta que muestra *todos* los umbrales a la vez.

### ⚠️ Errores comunes

1. **Tratar el 0,5 como sagrado.** Es un default, no un óptimo.
2. **Elegir el umbral mirando solo la exactitud.** La exactitud puede quedarse casi igual mientras el balance entre los dos errores cambia por completo.
3. **Elegir el umbral sin preguntar cuánto cuesta cada error.** Esa respuesta no está en el dataset: está en el negocio.

---
---

# PARTE 4 — Python y las librerías

## 4.1 Qué se suma al arsenal

Todo lo de las clases anteriores sigue en pie —pandas carga y prepara, matplotlib grafica, numpy calcula por debajo—. Esta clase agrega **dos librerías nuevas**, cada una con su filosofía:

```
        ┌────────────────────────────────────────────┐
        │              TU NOTEBOOK                   │
        └───────────────────┬────────────────────────┘
                            │
        ┌───────────────────┼────────────────────────┐
        │                   │                        │
 ┌──────▼───────┐    ┌──────▼────────┐      ┌────────▼───────┐
 │ statsmodels  │    │ scikit-learn  │      │ pandas · numpy │
 │   (smf)      │    │  (metrics +   │      │  · matplotlib  │
 │              │    │ LinearRegr.)  │      │                │
 │ INFERENCIA:  │    │ PREDICCIÓN:   │      │  los de siempre│
 │ p-valores,   │    │ métricas, ML, │      │                │
 │ intervalos,  │    │ el dialecto   │      │                │
 │ summary()    │    │ de tu TP      │      │                │
 └──────────────┘    └───────────────┘      └────────────────┘
```

La distinción de fondo, que te va a acompañar toda la carrera:

| | statsmodels | scikit-learn |
|---|---|---|
| **Nació de** | La tradición estadística/econométrica | La comunidad de machine learning |
| **Su pregunta** | ¿Qué dice el modelo del mundo? (inferencia) | ¿Qué tan bien predice? (rendimiento) |
| **Su salida estrella** | `summary()`: tabla completa con p-valores | `.predict()` y las métricas |
| **En esta clase** | La columna vertebral | Las métricas + el dialecto del TP |
| **En el Módulo 3** | Cede el escenario | Pasa a ser LA librería |

Los dos ajustan **el mismo modelo** con **los mismos mínimos cuadrados**: β₀ y β₁ salen idénticos. Lo que cambia es qué te cuentan sobre él.

## 4.2 statsmodels: la fórmula y el summary

### La interfaz de fórmulas

```python
import statsmodels.formula.api as smf

modelo = smf.ols("salario ~ experiencia", data=df).fit()
```

La cadena `"salario ~ experiencia"` es una mini-notación heredada del lenguaje R: *Y explicado por X*. Crece con `+` para la múltiple (`"salario ~ experiencia + exp_laboral"`), y no exige preparar matrices ni hacer reshape: le das el DataFrame y los nombres.

### Cómo leer el summary() sin ahogarse

`modelo.summary()` devuelve una tabla intimidante. El orden de lectura profesional es este — cuatro paradas, en este orden:

```
 ┌──────────────────────────────────────────────────────────┐
 │ ③ R-squared: 0.084          ① No. Observations: 1183     │
 │                                                          │
 │                 coef      std err     P>|t|   [0.025 …]  │
 │ Intercept    64251.___     ····       0.000              │
 │ experiencia   3320.___     ····     ② 0.000              │
 └──────────────────────────────────────────────────────────┘
```

| Parada | Qué mirás | Por qué primero |
|---|---|---|
| ① `No. Observations` | ¿Con cuántas filas ajustó **de verdad**? | Detecta filas perdidas en silencio (ver §4.5) |
| ② `P>\|t\|` de tu X | ¿La relación existe? (pregunta 1 de §1.3) | Si es alto, lo demás importa poco |
| ③ `R-squared` | ¿Cuánto explica? (pregunta 2) | El contexto del veredicto |
| ④ `coef` | ¿Cuánto vale la relación, en unidades reales? | La historia que vas a contar |

💡 Es la misma disciplina del `describe()` de la Clase 1 — donde la primera columna que se leía era `count`. Acá, lo primero es `No. Observations`. **Primero preguntá cuántos datos sostienen el resultado; después mirá el resultado.**

### Las piezas sueltas

```python
modelo.params          # β₀ y β₁
modelo.pvalues         # los p-valores
modelo.rsquared        # el R²
modelo.fittedvalues    # los ŷ de cada fila
modelo.resid           # los residuos
modelo.predict(pd.DataFrame({"experiencia": [5]}))   # predicción puntual
```

## 4.3 scikit-learn: el dialecto de tu trabajo práctico

El TP del campus (`student_scores.csv`) está escrito en el dialecto de scikit-learn. Es el mismo modelo con otra gramática — y una exigencia extra que te va a recibir con un error si no la conocés:

```python
from sklearn.linear_model import LinearRegression
import numpy as np

x = df_tp["Hours"].values.reshape(-1, 1)    # ← la parte rara (ver abajo)
y = df_tp["Scores"].values

reg = LinearRegression()
reg.fit(x, y)                    # entrenar = ajustar por mínimos cuadrados

reg.intercept_                   # β₀
reg.coef_                        # β₁ (viene en un array)
reg.score(x, y)                  # ¡es el R²!
reg.predict(np.array([[8]]))     # predicción para x = 8
```

### El `reshape(-1, 1)`, explicado de una vez

scikit-learn está diseñado para muchas variables a la vez, así que **exige que X sea una tabla** (matriz 2-D), aunque tengas una sola columna:

```
   df["Hours"].values         →  [2.5, 5.1, 3.2, ...]        1-D: una fila de números ❌
   .values.reshape(-1, 1)     →  [[2.5],                     2-D: una COLUMNA         ✅
                                  [5.1],
                                  [3.2], ...]
```

El `-1` significa "calculá vos cuántas filas"; el `1` es "una columna". Es un peaje de formato, no un concepto: statsmodels no lo pide porque su interfaz de fórmulas arma las matrices sola.

### La celda Rosetta: los dos dialectos, lado a lado

El mismo modelo, las mismas cuentas, dos gramáticas. Esta tabla traduce 1 a 1:

| Concepto | statsmodels (la clase) | scikit-learn (tu TP) |
|---|---|---|
| Ajustar | `smf.ols("y ~ x", data=df).fit()` | `LinearRegression().fit(X, y)` |
| β₀ | `modelo.params["Intercept"]` | `reg.intercept_` |
| β₁ | `modelo.params["x"]` | `reg.coef_[0]` |
| R² | `modelo.rsquared` | `reg.score(X, y)` |
| Predecir | `modelo.predict(pd.DataFrame({"x": [8]}))` | `reg.predict(np.array([[8]]))` |
| p-valores | `modelo.pvalues` | **No los da** — no es su oficio |
| Preparación de X | Ninguna (la fórmula se encarga) | `.values.reshape(-1, 1)` |

Esa penúltima fila es la diferencia filosófica entera: sklearn no calcula p-valores porque su mundo (predicción) no los necesita. Si tu pregunta es inferencial, statsmodels; si es predictiva a escala, sklearn.

## 4.4 Los NaN: dos filosofías opuestas ante el mismo agujero

Nuestro dataset tiene **un solo valor faltante** en `anios_codigo` — 1 entre 1.183. Un agujerito. Miralo provocar dos comportamientos opuestos:

| Herramienta | Ante el NaN hace... | Cómo te enterás |
|---|---|---|
| `smf.ols(...)` | **Tira la fila en silencio** y ajusta con 1.182 (en la múltiple, que sí usa `anios_codigo`) | Solo si mirás `No. Observations` |
| `LinearRegression().fit()` | **Explota**: `ValueError: Input X contains NaN.` | Imposible no enterarse |

¿Cuál es peor? **El silencioso.** El error de sklearn es molesto pero honesto: te frena y te obliga a decidir. El descarte mudo de statsmodels es una **falla silenciosa** — la misma categoría del extractor de países de la Clase 1 que producía columnas vacías sin quejarse. Con 1 fila entre 1.183 no cambia nada; el día que sean 400 filas descartadas sin aviso, tu modelo dirá cosas de una muestra que no es la que creés tener.

> ## ⚠️ La regla profesional
> ### **Los NaN se sacan a mano, conscientemente, ANTES de ajustar:**
> ```python
> df_modelo = df.dropna(subset=["salario", "experiencia", "anios_codigo"])
> ```
> ### Y después del ajuste, verificá `No. Observations` contra `len(df_modelo)`. Que nunca te descarten filas sin tu firma.

## 4.5 La logística en código: por qué `smf.logit`

```python
modelo_log = smf.logit("es_us ~ salario", data=datos_log).fit()
prob = modelo_log.predict(datos_log)                 # probabilidades 0–1
pred_clase = (prob >= 0.5).astype(int)               # umbral explícito
```

Para las métricas sí usamos scikit-learn — es su especialidad:

```python
from sklearn.metrics import confusion_matrix, classification_report

confusion_matrix(datos_log["es_us"], pred_clase)     # la matriz de §3.14
```

Y la advertencia repetida a propósito, porque cuesta un examen: `sklearn.linear_model.LogisticRegression` **regulariza por defecto** (sus coeficientes salen encogidos a propósito, una técnica que el Módulo 3 explica y aprovecha). No está roto — pero si querés coeficientes estadísticos "puros" con sus p-valores, la herramienta es `smf.logit`.

## 4.6 Las librerías que rondaron la clase

| Librería | Papel hoy | Su momento |
|---|---|---|
| **seaborn** | Una sola aparición: `sns.load_dataset("anscombe")` | Gráficos estadísticos, cuando quieran |
| **scipy.stats** | Descansó (fue la estrella de la Clase 2) | Pruebas de hipótesis |
| **scikit-learn** completo | Solo `metrics` y `LinearRegression` | **Módulo 3, con Dennis: LA librería** |

⚠️ `sns.load_dataset("anscombe")` **descarga el dataset de internet la primera vez** y lo deja en caché. Si vas a practicar sin conexión, corré esa celda una vez antes.

## 4.7 Tabla maestra de funciones

Para imprimir y tener al lado mientras practicás.

| Concepto | Función | Resultado en la encuesta |
|---|---|---|
| Correlación | `df[["salario","experiencia"]].corr()` | 0,2900 |
| Ajustar modelo simple | `smf.ols("salario ~ experiencia", data=df).fit()` | la recta |
| Tabla completa | `modelo.summary()` | — |
| Observaciones usadas | `No. Observations` (en el summary) | 1.183 en la simple; **1.182 en la múltiple** (el NaN está en `anios_codigo`) |
| **Intercepto β₀** | `modelo.params["Intercept"]` | **$64.251** |
| **Pendiente β₁** | `modelo.params["experiencia"]` | **$3.320/año** |
| p-valor de la pendiente | `modelo.pvalues["experiencia"]` | 2,32 × 10⁻²⁴ |
| **R²** | `modelo.rsquared` | **0,0841** |
| Predicciones del modelo | `modelo.fittedvalues` | los ŷ |
| Residuos | `modelo.resid` | media 0, asimetría 4,16 |
| Predicción puntual | `modelo.predict(pd.DataFrame({"experiencia": [5]}))` | **$80.853** |
| MAE | `mean_absolute_error(real, pred)` | $50.794 |
| MSE | `mean_squared_error(real, pred)` | (dólares²) |
| **RMSE** | `np.sqrt(mean_squared_error(real, pred))` | **$78.343** |
| Modelo múltiple | `smf.ols("salario ~ experiencia + exp_laboral + anios_codigo", ...)` | R² = 0,0900 |
| Correlación entre predictores | `df[[...]].corr()` | 0,795 / 0,864 / 0,891 |
| Ajustar (dialecto TP) | `LinearRegression().fit(x.reshape(-1,1), y)` | — |
| β₀ / β₁ (dialecto TP) | `reg.intercept_` / `reg.coef_` | — |
| R² (dialecto TP) | `reg.score(x, y)` | — |
| Predecir (dialecto TP) | `reg.predict(np.array([[8]]))` | — |
| Logística | `smf.logit("es_us ~ salario", data=datos_log).fit()` | exactitud 84,8% |
| Probabilidades | `modelo_log.predict(...)` | valores 0–1 |
| Matriz de confusión | `confusion_matrix(real, pred_clase)` | [[838, 46], [134, 165]] |
| Anscombe | `sns.load_dataset("anscombe")` | R² ≈ 0,67 los 4 grupos |

---
---

# PARTE 5 — Bibliografía comentada

Criterio de selección: **regresión enseñada con código y con honestidad sobre sus límites**, en fuentes académicas reconocidas — varias gratuitas y legales, y las interactivas, en español donde existe versión.

## 5.1 Núcleo — los tres imprescindibles

### 📕 1. James, G., Witten, D., Hastie, T. y Tibshirani, R. (2021). *An Introduction to Statistical Learning with Applications in R* (2.ª ed.). Springer.

**El libro de referencia mundial para este tema**, escrito por autores que son historia viva de la estadística moderna. El Capítulo 3 (*Linear Regression*) es la mejor explicación disponible de todo lo que vimos: las dos preguntas (¿hay relación? / ¿qué tan fuerte?), mínimos cuadrados, R², los problemas potenciales del modelo — y el Capítulo 4 cubre la clasificación y la regresión logística.

**De este libro sale la absolución de nuestro R²:** la observación (p. 70) de que en dominios ruidosos *"un R² bien por debajo de 0,1 puede ser más realista"*.

**Gratis y legal:** el PDF completo se descarga de **https://www.statlearning.com** (los autores lo liberaron). Existe también la versión con laboratorios en Python (*ISLP*), en el mismo sitio.

---

### 📗 2. Bruce, P., Bruce, A. y Gedeck, P. (2020). *Practical Statistics for Data Scientists: 50+ Essential Concepts Using R and Python* (2.ª ed.). O'Reilly Media.

Ya lo conocés de las clases anteriores; para esta clase son los **Capítulos 4** (*Regression and Prediction*) **y 5** (*Classification*). Es el libro que mejor separa los dos oficios del modelo (su sección *Prediction vs. Explanation* inspiró nuestra §2.3), el que declara al RMSE *"la métrica más importante en ciencia de datos"* (p. 153) y el que advierte que el R² sube siempre al agregar variables (p. 156).

> **🇪🇸 Edición en español:** *Estadística práctica para ciencia de datos con R y Python*. Marcombo, 2022. ISBN 978-84-267-3443-3.

---

### 📘 3. Çetinkaya-Rundel, M. y Hardin, J. *Introduction to Modern Statistics* (OpenIntro).

Libro abierto, gratuito y de calidad universitaria. Para esta clase: **Capítulos 7 a 9** — regresión lineal con un predictor, con varios predictores, y regresión logística. Explicaciones pausadas, ejercicios con solución y cero costo.

**Gratis en línea:** https://openintro-ims.netlify.app

---

## 5.2 Cursos universitarios abiertos

| Recurso | Qué aporta |
|---|---|
| **Penn State — STAT 501: Regression Methods** (https://online.stat.psu.edu/stat501) | Un curso universitario completo de regresión, con las notas abiertas. El tratamiento de supuestos, residuos y multicolinealidad va mucho más profundo que cualquier libro introductorio. Para cuando §3.11 y §3.12 te queden cortas. |
| **UCLA OARC Statistical Methods** (https://stats.oarc.ucla.edu) | Célebre por sus salidas **anotadas**: agarran un `summary()` real y explican qué significa cada número, casilla por casilla. El complemento perfecto de nuestra §4.2. |

## 5.3 Para VER la regresión (interactivos y video)

| Recurso | Qué aporta |
|---|---|
| **Seeing Theory** (Brown University) — capítulo de análisis de regresión, **en español**: https://seeing-theory.brown.edu/regression-analysis/es.html | Mínimos cuadrados arrastrando puntos con el mouse, y el cuarteto de Anscombe interactivo. Diez minutos acá valen una hora de texto. |
| **PhET** (University of Colorado) — simulador *"Regresión de mínimos cuadrados"*, **en español**: https://phet.colorado.edu | Ponés puntos, movés la recta a mano, y ves los cuadrados de los residuos crecer y achicarse en vivo. La imagen mental de §3.2, hecha juguete. |
| **StatQuest con Josh Starmer** (YouTube, inglés con subtítulos) | Los videos de regresión lineal, R² y regresión logística son el estándar de oro de la explicación visual. Tono ridículo, rigor total. |
| **DotCSV** (YouTube, **en español**) | Su video de regresión lineal y mínimos cuadrados es la mejor versión en castellano del tema, y su serie de descenso del gradiente es el puente ideal al Módulo 4. |

## 5.4 Documentación oficial

| Recurso | URL |
|---|---|
| statsmodels — fórmulas (la sintaxis `"y ~ x"`) | https://www.statsmodels.org/stable/example_formulas.html |
| scikit-learn — `LinearRegression` | https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LinearRegression.html |

La página de fórmulas de statsmodels es especialmente útil cuando quieras ir más allá del `+`: interacciones, transformaciones y variables categóricas dentro de la fórmula.

## 5.5 Los artículos citados en el texto

| Referencia | Por qué importa |
|---|---|
| **Anscombe, F. J. (1973).** "Graphs in Statistical Analysis". *The American Statistician*, 27(1), 17–21. | El cuarteto de §3.8. La demostración canónica de que el mismo R² puede esconder cuatro realidades distintas. |
| **Wasserstein, R. L. y Lazar, N. A. (2016).** "The ASA Statement on p-Values: Context, Process, and Purpose". *The American Statistician*, 70(2). | La declaración oficial de la asociación de estadísticos de EE. UU. Su principio 5 es el corazón de §3.9: el p-valor no mide tamaño ni importancia. |

## 5.6 🚀 Si querés ir más lejos (con una advertencia honesta)

### DeepLearning.AI — *Mathematics for Machine Learning and Data Science* (Coursera)

Especialización de **3 cursos** (~94 horas en total) dictada por **Luis Serrano** — uno de los mejores divulgadores de matemática para ML, en un inglés muy claro. Cubre álgebra lineal, cálculo y probabilidad/estadística orientadas a machine learning. Es **de pago** en Coursera, aunque cada curso se puede **auditar gratis** (ver el contenido sin certificado ni tareas calificadas).

⚠️ **La advertencia honesta:** esta especialización **NO cubre la regresión como tema** — la usa apenas como excusa para enseñar otra cosa (el descenso del gradiente, la forma en que las máquinas *encuentran* los β cuando no hay fórmula exacta). No la tomes para reforzar esta clase ni para el examen: no es eso. Tomala, si te entusiasmó este mundo, como **preparación matemática para los Módulos 3 y 4** (machine learning y redes neuronales, con Dennis). Para esta clase, ISLR y OpenIntro son el camino.

## 5.7 Los datasets

- **`data_dev.csv`** — muestra de la **Stack Overflow Developer Survey 2023** (1.183 respuestas, 24 columnas), el dataset de las Clases 2 y 3. Sin sanear a propósito: el dev de $3/año y el de $1.200.000 vienen de fábrica.

- **Repositorio oficial del módulo:** https://github.com/Khipus-ai/Applied_Statistics_Python — carpeta `4 Advanced_Statistical_Methods`:
  - `real_estate_price_size.csv` — el case study de regresión lineal (R² = 0,745; el intercepto sin sentido físico de §3.3)
  - `student_scores.csv` — **tu trabajo práctico** (25 filas, R² = 0,953: el mundo de laboratorio)
  - `Admittance.csv` — el ejemplo canónico de regresión logística del material oficial (la curva S del deck)

---
---

# ANEXO — Tabla maestra de valores del dataset

Todos los valores verificados sobre `data_dev.csv` (1.183 filas), ejecutados en el notebook de la clase. Para consulta rápida durante el estudio.

## La variable dependiente: `salario` (anual, USD)

| Estadístico | Valor |
|---|---:|
| n | 1.183 |
| **Media** | **$90.684** |
| **Mediana** | **$72.714** |
| Mínimo | **$3** (el "dev fantasma") |
| Máximo | **$1.200.000** |
| Diagnóstico Clase 1 | media > mediana → cola derecha |

## El modelo simple: `salario ~ experiencia`

| Pieza | Valor |
|---|---:|
| **β₀ (intercepto)** | **$64.251** |
| **β₁ (pendiente)** | **$3.320 por año** |
| p-valor de la pendiente | **2,32 × 10⁻²⁴** |
| Correlación r | 0,2900 |
| **R²** | **0,0841** (= 0,29², exacto) |
| MAE | $50.794 |
| **RMSE** | **$78.343** |
| **RMSE ÷ sueldo medio** | **86%** |
| Observaciones usadas | 1.183 (acá no se descarta nada: el único NaN está en `anios_codigo`, que este modelo no usa) |

## La pregunta ancla

| Versión | Respuesta |
|---|---|
| Ingenua | ŷ(5 años) = **$80.853** |
| **Honesta** (± 1 RMSE) | **$2.510 – $159.195** ("de una llanta usada a un penthouse") |

## Los residuos

| Estadístico | Valor |
|---|---:|
| Media | 0 (exactamente, por construcción — no informa calidad) |
| Mínimo | −$150.562 |
| Máximo | **+$1.019.539** |
| Asimetría | **+4,16** (Airbnb Clase 1 tenía +2,34) |

## La múltiple y la colinealidad

| Pieza | Valor |
|---|---:|
| Fórmula | `salario ~ experiencia + exp_laboral + anios_codigo` |
| R² múltiple | **0,0900** (desde 0,0841 — subir era inevitable: Bruce p. 156) |
| p de `experiencia`: sola → acompañada | **2,3 × 10⁻²⁴ → 0,174** |
| Correlaciones entre predictores | **0,795 · 0,864 · 0,891** |

## La extrapolación

| Pieza | Valor |
|---|---:|
| Rango observado de experiencia | 0 – 50 años |
| Predicción a 80 años | **$329.874** (ficción con decimales) |

## La logística: `es_us ~ salario`

| Pieza | Valor |
|---|---:|
| Exactitud | **84,8%** |
| Base (siempre "Not US": 884 de 1.183) | **74,7%** |
| Matriz de confusión [real × predicho] | [[838, 46], [134, 165]] |
| Precisión (US) = 165/211 | ≈ 78,2% |
| Sensibilidad (US) = 165/299 | ≈ 55,2% |
| Encuestados de EE. UU. | 299 de 1.183 (25,3%) |

## Los contraejemplos y referencias cruzadas

| Dataset / caso | Valor |
|---|---:|
| Anscombe (4 grupos) | R² ≈ **0,67 los cuatro** |
| `real_estate_price_size.csv` | R² = 0,745 |
| `student_scores.csv` (25 filas — tu TP) | R² = 0,953 |
| Clase 2, la diferencia que no fue | p = 0,3529 |

## El NaN y las dos filosofías

| Herramienta | Ante 1 NaN en 1.183 filas |
|---|---|
| `smf.ols` | En la múltiple ajusta con 1.182 en silencio (mirá `No. Observations`) |
| `LinearRegression().fit()` | `ValueError: Input X contains NaN.` |

---

<div align="center">

**Módulo 2 — Applied Statistics with Python**
Material de estudio · Métodos Estadísticos Avanzados: Regresión Lineal y Logística
Docente: Walter J. Méndez · UTEPSA / Khipus.ai

*"Una recta siempre se puede ajustar. La pregunta profesional es cuánto se equivoca.*
*El p-valor te dice si la relación existe; el RMSE, si te conviene usarla.*
*La honestidad es reportar los dos."*

</div>
