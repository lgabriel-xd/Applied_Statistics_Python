# Guía visual — los 11 gráficos de la Clase 3

> Módulo 2 · Clase 3 — Métodos Estadísticos Avanzados (Regresión lineal y logística) · UTEPSA / Khipus.ai
> **Para el docente antes de dictar** (poder responder cualquier pregunta sobre el gráfico) y **para
> el alumno después** (volver sobre algo que pasó rápido).
>
> Todas las cifras de este documento fueron **calculadas ejecutando código contra `data_dev.csv`**,
> el mismo archivo de la Clase 2. Ninguna está estimada. Las afirmaciones históricas están
> verificadas contra las fuentes listadas al final.

---

## Prólogo: ¿por qué esta clase es tan visual?

La Clase 2 usaba los gráficos para ver cosas que **no existen**: 2000 mundos simulados donde nada
tenía efecto. Hoy es exactamente al revés: los usamos **para auditar números que sí existen y están
bien calculados.**

El `summary()` de hoy entrega un p-valor de 10⁻²⁴ — real, impecable, indiscutible. Y la conclusión
que cualquiera sacaría de esa tabla ("el modelo sirve") es **falsa**. No hay ningún número en la
tabla que lo delate: se ve únicamente dibujando. La dispersión vertical de la nube (gráfico 1), el
largo de las líneas grises (gráfico 4), las cuatro caras de un mismo R² (gráfico 7) — ahí vive la
frase de la clase, **estadísticamente significativo ≠ útil**, que no se puede demostrar con otra
tabla. Se demuestra con imágenes.

⚠️ **La advertencia que atraviesa toda esta guía.** La seducción de los números-resumen corre para
los dos lados. Nuestro p-valor de 10⁻²⁴ no vuelve útil al modelo (primera trampa), y el r² de 0,814
entre las películas de Nicolas Cage y los inspectores de la TSA en Dakota del Norte no vuelve real
esa relación (segunda trampa). La clase entera camina entre esas dos zanjas, y los gráficos son el
pasamanos. El patrono formal de la guía es Anscombe (1973): cuatro conjuntos de datos, los mismos
números, cuatro historias distintas. Es el gráfico 7 — no lo saltees.

---

## Cómo usar esta guía

Los 11 gráficos, en el orden en que aparecen en la clase:

| # | Gráfico | Bloque | Qué tiene que quedar |
|---|---|---|---|
| 1 | La nube de puntos (scatter) | A | La pregunta se ve antes de calcular: sube, pero con muchísimo ruido |
| 2 | La nube con la recta | A | Un modelo son dos números — y la primera respuesta: $80.853 |
| 3 | Mapa de calor de correlaciones | A *(completa)* | Los predictores se parecen más entre sí que al salario |
| 4 | **Los residuos como líneas grises** | B | **El error es un objeto: se dibuja y se mide** |
| 5 | Histograma de residuos | B *(completa)* | Los errores no forman campana: tienen cola |
| 6 | Residuos vs. predichos | B *(completa)* | El modelo falla más justo donde predice sueldos altos |
| 7 | **El cuarteto de Anscombe** | B | **El mismo R² puede venir de cuatro realidades distintas** |
| 8 | Zona con datos vs. extrapolación | D-extra *(completa)* | El modelo contesta igual donde no sabe nada |
| 9 | La recta que se escapa | D | Una recta no sirve cuando la respuesta es sí o no |
| 10 | La sigmoide | D | La curva S convierte sueldos en probabilidades — y funciona |
| 11 | Matriz de confusión | D *(completa)* | La exactitud esconde en qué dirección te equivocás |

Los gráficos **4 y 7 son los irrenunciables**: el 4 sostiene el clímax del RMSE y el 7 es la regla
final de la clase hecha imagen. El guion ya trae el orden de sacrificio si el reloj aprieta: primero
el VIF, después el TU TURNO 2, después la tabla de Anscombe (dejando el dibujo), y recién al final
el gráfico 9, que se puede contar sin proyectar.

**Seis se dictan en vivo** (1, 2, 4, 7, 9, 10 — la versión Mini) y **cinco viven solo en la versión
completa** (3, 5, 6, 8, 11), que es con la que los alumnos estudian después. Tenés que poder
responder por los once igual.

Detalle de nomenclatura, porque los bloques **no se llaman igual en las dos versiones**: la Mini
tiene cuatro (A–D) y la completa cinco, porque intercala **«D · Hasta dónde le podés creer»** —la
extrapolación y los datos sucios— antes de la logística. Por eso el bloque de logística es el **D
en la Mini y el E en la completa**. En esta guía lo nombramos «E (D en la Mini)».

---

# 1. La nube de puntos

**Bloque A · `plt.scatter(df["experiencia"], df["salario"], alpha=0.35)`**

## 1.1 ¿Qué es?

Cada punto es un encuestado: su posición horizontal dice cuántos años lleva programando
profesionalmente, la vertical cuánto gana al año. 1.183 personas, 1.183 puntos. Los histogramas de
las Clases 1 y 2 miraban **una** variable por vez; el scatter mira **dos a la vez**, y la pregunta
cambia de "¿cómo se reparte?" a "¿se acompañan?".

## 1.2 Anatomía

- **`alpha=0.35`:** cada punto es 65% transparente. Con 1.183 puntos encimados, sin transparencia
  la zona densa sería una mancha sólida; con ella, donde el celeste es más intenso hay más gente
  apilada. El gráfico gana un tercer eje implícito: la densidad.
- **`edgecolor="none"`:** saca el borde de cada punto, para que el apilado no se ensucie.
- **Las columnas verticales de puntos** no son un defecto: la experiencia se declara en años
  enteros, así que los puntos forman columnas en 0, 1, 2, 3… La variable es discreta en la práctica.
- **El eje Y dice `1e6` arriba:** notación de matplotlib. El "1.2" de la esquina es $1.200.000.

## 1.3 Para qué sirve en la vida real

Es el primer gráfico de cualquier pregunta de dos variables: precio vs. superficie, ventas vs.
publicidad, latencia vs. carga del servidor. Se hace **antes** de ajustar cualquier modelo, porque
es el detector más barato de sorpresas: relaciones curvas, grupos separados, y datos imposibles —
como nuestro dev que declaró ganar $3 al año.

## 1.4 Para qué lo usamos en esta clase

Es la pregunta de la clase hecha imagen (el título lo dice: *¿el sueldo sube con la experiencia?*).
La predicción 🤔 A/B/C del Bloque A se contesta acá, y la respuesta honesta es **B**: sube, pero muy
desordenada. Lo que hay que señalar **no es la tendencia sino la dispersión vertical**: para
cualquier experiencia hay gente ganando 20 mil y gente ganando 200 mil. Esa dispersión ES el RMSE
que se revela cuarenta minutos después — el gráfico ya contiene el final de la clase, solo que
todavía nadie sabe mirarlo.

## 1.5 Los hallazgos (verificados)

| Dato | Valor |
|---|---|
| Puntos dibujados | 1.183 |
| Rango del eje X | 0 a 50 años |
| Rango del eje Y | **$3** a **$1.200.000** |
| Correlación salario ~ experiencia | **0,2900** |
| Sueldo: media / mediana | $90.684 / $72.714 |

💡 **El dato que sorprende:** el punto más alto del gráfico ($1.200.000) está sobre la columna de
los **35 años** de experiencia — pero el segundo más alto (~$920.000, leído del gráfico) pertenece a
alguien con apenas **4 años**. La cima del gráfico no la explica la experiencia. Y el dev de $3 está
ahí también, pegado al piso, indistinguible a simple vista entre los cientos de puntos de abajo:
ningún gráfico te lo va a señalar solo.

## 1.6 Curiosidades e historia

El scatter es sorprendentemente joven comparado con otros gráficos. Los historiadores Friendly y
Denis (2005) atribuyen el primero a **John Herschel, en 1833**: puntos dibujados a mano para
estudiar la órbita de estrellas dobles en la constelación de Virgo. La humanidad tuvo mapas,
barras y series de tiempo durante siglos antes de que a alguien se le ocurriera poner **dos
mediciones como coordenadas de un punto** — que es, visto desde hoy, la idea que hizo posible la
correlación, la regresión y buena parte del machine learning.

## 1.7 Errores comunes

- **Leer la densidad de la izquierda como "los junior ganan menos".** La izquierda tiene más puntos
  porque hay **más gente** con poca experiencia en la muestra, no porque les vaya peor.
- **Confundir "no se ve forma clara" con "no hay relación".** La correlación de 0,29 existe y es
  real (p = 10⁻²⁴); solo que es floja, y el ojo humano no ve correlaciones flojas.
- **Leer mal el multiplicador del eje.** "0.2" en este eje Y son $200.000. Es el error de lectura
  más silencioso del gráfico.

## 1.8 El código

```python
plt.figure(figsize=(9, 5.5))
plt.scatter(df["experiencia"], df["salario"], alpha=0.35, color="#4C9BE8", edgecolor="none")
plt.title("¿El sueldo sube con la experiencia?", fontsize=14, fontweight="bold")
```

---

# 2. La nube con la recta

**Bloque A · `smf.ols("salario ~ experiencia")` + `plt.plot(x_linea, b0 + b1 * x_linea)`**

## 2.1 ¿Qué es?

La misma nube, más **la única recta que hace más chica la suma de los residuos al cuadrado**. La
leyenda trae la fórmula completa: *Modelo: $64.251 + $3.320 × años*. Eso —dos números— es todo el
modelo. Acabamos de comprimir 1.183 sueldos en un intercepto y una pendiente; la nube que queda
alrededor es todo lo que esa compresión pierde.

## 2.2 Anatomía

- **`x_linea = np.linspace(min, max, 100)`:** la recta se dibuja con 100 puntos equiespaciados
  **solo dentro del rango real de los datos** (0 a 50 años). Es un detalle deliberado: no dibujamos
  el modelo donde no hay datos. El gráfico 8 rompe esa regla a propósito.
- **β₀ = $64.251:** la altura donde la recta corta el eje Y (la predicción para 0 años).
- **β₁ = $3.320:** la pendiente — cuánto sube la predicción por cada año más.
- ⚠️ **La leyenda formatea los miles con coma, a la gringa** (`{:,.0f}` de Python): "64,251" es
  sesenta y cuatro mil doscientos cincuenta y uno, no sesenta y cuatro coma dos. Avisalo antes de
  que alguien lo lea como decimal.

## 2.3 Para qué sirve en la vida real

Es el modelo mínimo viable de cualquier relación numérica: presupuesto según headcount, consumo
según temperatura, costo según volumen. Dos números que caben en una frase — *"arrancás en tanto y
subís tanto por unidad"* — que cualquier gerente entiende y cualquier planilla implementa. La mitad
del valor de la regresión lineal es esa comunicabilidad.

## 2.4 Para qué lo usamos en esta clase

Produce la **primera respuesta a la pregunta ancla**: $64.251 + $3.320 × 5 = **$80.853** para 5
años de experiencia. Y produce el enganche con la Clase 2: la pendiente tiene p = 2,32e-24, así que
con el criterio del sábado pasado **rechazamos H₀** — la relación existe de verdad. El guion pide
decir la respuesta con seguridad y rematar: *"si la clase terminara acá, te irías con una respuesta
equivocada"*.

## 2.5 Los hallazgos (verificados)

| Dato | Valor |
|---|---|
| β₀ (intercepto) | $64.251 |
| β₁ (pendiente) | **$3.320 por año** |
| p-valor de la pendiente | 2,32e-24 |
| R² | 0,0841 |
| Predicción a 5 años | **$80.853** |
| Correlación salario ~ experiencia | 0,2900 — y **0,29² = 0,0841 = R², exacto** |

💡 **El dato que explica el gráfico entero:** a lo largo de sus 50 años, la recta sube $166.014 (de
$64.251 a $230.265, sus dos extremos verificados). La nube, a la altura de **un solo año**
cualquiera, ya abarca más que eso en vertical. La pendiente es real; el ruido es más grande que la
pendiente. Esa desproporción **es** la clase.

## 2.6 Curiosidades e historia

La palabra "regresión" es un accidente histórico. **Francis Galton** la encontró midiendo primero
arvejas dulces (1877, lo llamó *reversion*) y después estaturas humanas: en su paper de 1886,
*"Regression towards Mediocrity in Hereditary Stature"*, mostró que los hijos de padres altos salen
en promedio más bajos que sus padres, y los de padres bajos, más altos — todos "regresan" hacia el
promedio. El nombre describe **ese fenómeno biológico**, no la técnica; la técnica lo heredó de
contrabando y ya nadie lo va a devolver. De esa época sobrevive también la letra: la **r** de la
correlación viene de *reversion/regression*. Bonus: Galton era medio primo de Charles Darwin —
compartían abuelo, Erasmus Darwin.

## 2.7 Errores comunes

- **Leer β₁ causalmente.** "Un año más de experiencia te *da* $3.320 más" es falso. El modelo
  **predice** una diferencia entre personas distintas; no dice que acumular un año la *cause*.
- **Tratar la recta como "el sueldo esperado, confiable".** La recta pasa por el medio de una nube
  de ±$78.000. Es un centro, no una promesa.
- **Esperar que los puntos toquen la recta.** Casi ninguno cae encima. Una predicción puntual casi
  nunca acierta exacto — por eso el Bloque B mide *cuánto* le erra.

## 2.8 El código

```python
x_linea = np.linspace(df["experiencia"].min(), df["experiencia"].max(), 100)
plt.scatter(df["experiencia"], df["salario"], alpha=0.30, color="#4C9BE8", edgecolor="none",
            label="Cada punto = un encuestado")
plt.plot(x_linea, b0 + b1 * x_linea, color="red", linewidth=2.5,
         label=f"Modelo: ${b0:,.0f} + ${b1:,.0f} × años")
```

---

# 3. El mapa de calor de correlaciones

**Bloque A (versión completa) · `sns.heatmap(df[columnas].corr(), annot=True)`**

*En la Mini que se dicta en vivo, las correlaciones aparecen como tabla impresa en el Bloque C; el
mapa de calor vive en la versión completa, dentro del Bloque A.*

## 3.1 ¿Qué es?

La matriz de correlaciones de las cuatro variables numéricas (salario, experiencia, exp_laboral,
anios_codigo) pintada como mapa de calor: cada celda es la correlación entre un par de variables,
un número entre −1 y 1, con un color encima.

## 3.2 Anatomía

- **`annot=True, fmt=".2f"`:** escribe el número sobre el color. El color llama la atención; el
  número decide. Nunca entregues un heatmap sin `annot`.
- **`cmap="RdBu_r", center=0, vmin=-1, vmax=1`:** escala divergente **anclada al rango teórico**:
  blanco en 0, rojo hacia +1, azul hacia −1. Sin `vmin/vmax`, seaborn estira la escala a los datos
  presentes y un 0,28 puede pintarse intenso como si fuera enorme.
- **La diagonal siempre da 1,00** (toda variable correlaciona perfecto consigo misma). Sirve de
  control de cordura: si la diagonal no da 1, algo está roto.
- **La matriz es simétrica:** el triángulo de arriba espeja el de abajo. Solo hace falta leer uno.

## 3.3 Para qué sirve en la vida real

Es el primer vistazo estándar de cualquier dataset multivariable: en selección de variables para
ML detecta predictores redundantes **antes** de modelar; en finanzas, la correlación entre activos
decide una diversificación; en calidad industrial, delata dos sensores que miden lo mismo.

## 3.4 Para qué lo usamos en esta clase

Siembra las dos bombas de la clase con cuarenta minutos de anticipación. Primera: salario ~
experiencia da apenas **0,29** — floja — y ese número al cuadrado va a reaparecer como el R² del
modelo. Segunda: los tres predictores se correlacionan **entre sí** (0,80–0,89) muchísimo más que
con el salario que quieren predecir (0,28–0,29). Eso es exactamente lo que explota en el Bloque C,
cuando la variable estrella se apaga.

## 3.5 Los hallazgos (verificados)

| Par | Correlación |
|---|---|
| salario ~ experiencia | **0,2900** |
| salario ~ exp_laboral / salario ~ anios_codigo | 0,28 / 0,29 (como se ven impresas en el gráfico) |
| experiencia ~ anios_codigo | **0,891** |
| experiencia ~ exp_laboral | **0,864** |
| exp_laboral ~ anios_codigo | **0,795** |

💡 **La lectura que importa:** el gráfico tiene dos zonas de color. La columna del salario, pálida
(0,28–0,29); el triángulo de los tres predictores, rojo oscuro (0,80–0,89). **Los predictores se
parecen entre sí el triple de lo que se parecen a lo que quieren predecir.** Es la trampa de los
gemelos, visible acá antes de que tenga nombre — y por eso el p-valor de `experiencia` va a pasar
de 2,3e-24 a **0,174** cuando las metamos juntas.

## 3.6 Curiosidades e historia

La idea de correlación es de Galton, pero la fórmula moderna la formalizó **Karl Pearson en 1896**
— por eso el coeficiente lleva su nombre. El heatmap, en cambio, tiene dos cumpleaños: como técnica,
los historiadores rastrean matrices sombreadas hasta **Toussaint Loua (1873)**, que pintó
estadísticas sociales de los barrios de París; como palabra, *"heat map"* la acuñó y **registró
como marca** el diseñador de software **Cormac Kinney en 1991**, para paneles financieros en tiempo
real. La marca pasó a otra empresa en 1998 y caducó en 2006 — por eso hoy todo el mundo puede
llamar heatmap a un heatmap sin pagar regalías.

## 3.7 Errores comunes

- **Leer correlación como causa.** El clásico entre los clásicos. El heatmap no sabe qué causa qué;
  solo sabe qué se mueve junto.
- **Creer que correlación 0 significa "no hay relación".** Solo mide relaciones **lineales**: una
  relación en forma de U puede dar 0 y ser perfectísima. El panel II de Anscombe (gráfico 7) es el
  dibujo de esta advertencia.
- **Elegir predictores mirando solo la primera columna.** "Las tres dan ~0,29 con salario, metamos
  las tres" ignora el triángulo rojo de abajo — y es literalmente el error que el Bloque C comete a
  propósito.
- **Dejar que el color autoescale.** Sin `vmin=-1, vmax=1`, la paleta se estira a lo que haya y
  cualquier correlación mediocre se ve dramática.

## 3.8 El código

```python
sns.heatmap(df[["salario", "experiencia", "exp_laboral", "anios_codigo"]].corr(),
            annot=True, fmt=".2f", cmap="RdBu_r", center=0, vmin=-1, vmax=1,
            square=True, linewidths=0.5)
```

---

# 4. ⭐ Los residuos como líneas grises

**Bloque B · el gráfico más importante de la clase**

## 4.1 ¿Qué es?

Los primeros 60 encuestados, con tres capas: su sueldo real (punto celeste), la predicción del
modelo (recta roja) y, para cada uno, **una línea gris vertical que une las dos cosas**. Esa línea
es el residuo: `real − predicho`, el error del modelo para esa persona, dibujado a escala. Se
grafican 60 y no 1.183 porque con todos, las líneas taparían el gráfico entero.

## 4.2 Anatomía

- **`sub = dm.head(60)`:** los primeros 60 del archivo, no una muestra al azar — reproducible sin
  semilla.
- **El `for` con `plt.plot([x, x], [real, predicho])`:** dibuja cada segmento vertical de a uno.
  Mismo x dos veces, los dos extremos en y.
- **`zorder=3` / `zorder=2`:** los puntos van encima de las líneas, para que se lean.
- **Arriba de la recta** el residuo es positivo (gana más de lo predicho); **abajo**, negativo.
- Una línea larga = un error grande. En este recorte de 60 ya hay líneas grises de más de $100.000.

## 4.3 Para qué sirve en la vida real

Es la imagen mental correcta de "mi modelo se equivoca". Todo pronóstico operativo —demanda,
ventas, latencia, producción— lleva una línea gris invisible colgando de cada predicción, y
presupuestar sin conocer el largo típico de esas líneas es el origen de la mitad de los "el sistema
no dio lo que prometía". La rotación por perfiles del guion sale sola: latencia real vs. esperada,
retorno real vs. proyectado, saldo real vs. declarado, producción real vs. la de diseño.

## 4.4 Para qué lo usamos en esta clase

Instala el residuo como **objeto físico antes de resumirlo en un número**. La predicción más
importante del día (🤔 *"¿de cuánto es el error típico?"*, A/B/C/D) se contesta mirando este
gráfico, y el revelado que sigue — RMSE = $78.343, el **86%** del sueldo promedio — es
literalmente *"el tamaño típico de una línea gris"*. Si este gráfico no respiró, el 💥 del 86% cae
en el vacío. El guion pide dejarlo en pantalla y bajar la velocidad.

## 4.5 Los hallazgos (verificados)

| Dato | Valor |
|---|---|
| Media de los residuos | **0,0000** — siempre, por construcción |
| Desvío de los residuos | $78.376 |
| RMSE / MAE | **$78.343** / $50.794 |
| Residuo mínimo / máximo | −$150.562 / **+$1.019.539** |
| Asimetría de los residuos | **4,16** |

💡 **Tres lecturas que el notebook no hace y conviene tener a mano:**

1. **RMSE ≈ desvío de los residuos** ($78.343 vs. $78.376). No es casualidad: como la media de los
   residuos es 0, el RMSE *es*, en la práctica, el desvío estándar del error.
2. **El residuo máximo (+$1.019.539) es la persona de $1.200.000** (35 años de experiencia, el
   punto más alto del gráfico 1): el modelo le predijo unos $180.000 y la realidad le pasó por
   encima por un millón entero. No aparece en este recorte de 60 — pero domina todas las métricas.
3. **La asimetría (4,16) tiene una explicación física:** hacia abajo hay piso — para tener un
   residuo de −$1.000.000 el modelo tendría que predecir $1.000.000, y su predicción máxima es
   $230.265 —, hacia arriba no hay techo. Los errores grandes solo pueden ser hacia arriba.

## 4.6 Curiosidades e historia

El criterio que elige la recta —**mínimos cuadrados**— protagoniza la disputa de prioridad más
famosa de la estadística: **Legendre lo publicó primero (1805**, en un apéndice de un libro sobre
órbitas de cometas**), y Gauss declaró en 1809** que lo venía usando desde 1795. La leyenda de
Gauss tiene fecha: el 1 de enero de 1801 Piazzi descubrió Ceres y la siguió 40 días antes de
perderla tras el sol; Gauss, con 24 años, calculó dónde iba a reaparecer, y von Zach la reencontró
en diciembre casi exactamente donde Gauss dijo. El método que esa noche lo hizo famoso es el mismo
que hoy corre adentro de `smf.ols` en milisegundos.

## 4.7 Errores comunes

- **Usar "el residuo promedio da 0" como señal de buen modelo.** Da 0 **siempre**, en cualquier
  regresión, hasta en una pésima: es consecuencia matemática de cómo se elige la recta, no un
  mérito. Para medir el error están el MAE y el RMSE, que le sacan el signo.
- **Medir el residuo como distancia perpendicular a la recta.** Es **vertical**: solo el error en
  el sueldo importa, porque la experiencia no se predice — se conoce.
- **Creer que el modelo "solo les erra a los raros".** El error típico es $78.343 en un dataset
  cuyo sueldo **mediano** es $72.714: el error típico es más grande que el sueldo típico. Las
  líneas grises son la regla, no la excepción.

## 4.8 El código

```python
sub = dm.head(60)
plt.scatter(sub["experiencia"], sub["salario"], color="#4C9BE8", zorder=3, label="Sueldo real")
plt.plot(x_linea, b0 + b1 * x_linea, color="red", linewidth=2, label="Predicción del modelo")
for _, f in sub.iterrows():
    plt.plot([f["experiencia"]] * 2, [f["salario"], f["predicho"]],
             color="gray", linewidth=0.9, alpha=0.7, zorder=2)
```

---

# 5. El histograma de residuos

**Bloque B (versión completa) · `plt.hist(dm["residuo"], bins=50)` + `plt.axvline(0)`**

## 5.1 ¿Qué es?

Los 1.183 residuos del modelo, repartidos en 50 canastas. El eje X ya no son sueldos: son
**errores**, en dólares, con el cero (predicción perfecta) marcado con línea roja punteada. A la
izquierda del cero, gente que gana menos de lo que el modelo predijo; a la derecha, gente que gana
más.

## 5.2 Anatomía

- **`bins=50` y `edgecolor="black"`:** canastas finas con borde, para que la cola rala se vea.
- **`axvline(0)`:** el cero es la referencia moral del gráfico; sin esa línea, el ojo no tiene
  ancla.
- **La forma:** la barra más alta (~260 personas, leída del gráfico) está apenas **a la izquierda**
  del cero — el error más común del modelo es quedarse un poco largo: predecirle a la gente algo
  más de lo que gana. Y la cola derecha se estira hasta +$1.019.539 mientras la izquierda muere en
  −$150.562: el gráfico es brutalmente asimétrico.

## 5.3 Para qué sirve en la vida real

Es el chequeo estándar de un modelo antes de entregarlo: ¿los errores son una campana centrada, o
tienen cola? Un modelo cuyos residuos tienen cola larga a la derecha **subestima sistemáticamente
los casos extremos** — en pricing, crédito o seguros, esa cola es plata. El RMSE no distingue un
modelo que erra parejito de uno que casi siempre pega y a veces yerra por un millón; este
histograma sí.

## 5.4 Para qué lo usamos en esta clase

En la versión completa es el chequeo del supuesto de **normalidad de los residuos**: lo que
querrías ver es campana centrada en cero; lo que hay es campana con cola kilométrica — supuesto
violado. Y reconecta con la Clase 2: esta forma es casi calcada del histograma de sueldos crudos de
ese sábado, porque un modelo que solo explica el 8,4% de la variación **les hereda a sus errores
casi toda la forma de los datos**.

## 5.5 Los hallazgos (verificados)

| Dato | Valor |
|---|---|
| Media | **0,0000** (por construcción) |
| Desvío | $78.376 |
| Mínimo / máximo | −$150.562 / +$1.019.539 |
| Asimetría | **4,16** |
| Asimetría de los sueldos crudos (Clase 2, verificada) | 4,19 |

💡 **El par 4,19 → 4,16 es el hallazgo.** Toda la regresión —el ajuste, los mínimos cuadrados, el
p-valor de 10⁻²⁴— le limó a la asimetría de los datos **tres centésimas**. El modelo explica tan
poco que sus errores son, en la práctica, los datos originales corridos de lugar.

## 5.6 Curiosidades e historia

El supuesto de residuos normales no es un capricho: **la campana nació siendo una teoría de los
residuos**. Gauss introdujo la distribución normal en 1809 justamente para justificar mínimos
cuadrados sobre errores de medición astronómica, y durante todo el siglo XIX se la llamó "ley de
los errores". En astronomía el supuesto era razonable: medís de más o de menos con la misma chance.
En sueldos no existe esa simetría — hacia abajo hay piso y hacia arriba no hay techo — y este
histograma es la demostración de por qué una ley nacida midiendo estrellas no se traslada gratis a
la economía.

## 5.7 Errores comunes

- **Exigirles normalidad a los DATOS.** El supuesto clásico es sobre los **residuos**, no sobre los
  sueldos. Es de los errores de manual más repetidos.
- **Creer que residuos no normales invalidan todo.** Los β siguen siendo los de mínimos cuadrados;
  lo que se degrada es la confiabilidad fina de p-valores e intervalos. Como dice el notebook: el
  modelo es todavía menos confiable de lo que ya sabíamos, y reportarlo es parte del trabajo.
- **Confundir este histograma con el de sueldos de la Clase 2.** Se parecen muchísimo (por el R²
  bajo), pero el eje X acá es el **error**, está centrado en cero y tiene valores negativos. Son
  objetos distintos que se parecen por un motivo que vale la pena explicar.

## 5.8 El código

```python
plt.hist(dm["residuo"], bins=50, color="#4C9BE8", edgecolor="black")
plt.axvline(0, color="red", linestyle="--", linewidth=2, label="Residuo = 0")
```

---

# 6. Residuos contra valores predichos

**Bloque B (versión completa) · EL gráfico de diagnóstico de la regresión**

## 6.1 ¿Qué es?

Un punto por encuestado: en el eje X, **lo que el modelo predijo** para esa persona; en el eje Y,
**cuánto le erró** (el residuo). La línea roja horizontal en cero es la predicción perfecta. Es el
gráfico que un estadístico mira antes que el R².

## 6.2 Anatomía

- **El eje X es angosto a propósito de la realidad:** va de $64.251 (la predicción para 0 años, o
  sea β₀) a $230.265 (50 años). El modelo solo "se anima" a predecir en esa franja, aunque los
  sueldos reales vayan de $3 a $1.200.000.
- **Las columnas verticales reaparecen:** el predicho es función determinística de la experiencia,
  que es entera — hay a lo sumo 51 valores posibles de X.
- **Lo que querrías ver:** una banda pareja, sin forma, mitad arriba y mitad abajo.
- **Lo que se ve:** un embudo que se abre hacia la derecha — **heterocedasticidad**: la varianza
  del error no es constante.
- **El borde inferior en diagonal** (abajo a la izquierda) no es casualidad: como nadie gana menos
  de $0, el residuo no puede ser más negativo que el propio predicho (`residuo ≥ −predicho`). Es la
  frontera "sueldo cero" dibujada en coordenadas de residuos.

## 6.3 Para qué sirve en la vida real

Es el detector universal de patologías del modelo: una curva en U delata que la relación no era
lineal; un embudo, que el error crece con el nivel; puntos aislados, outliers con influencia. En ML
es la versión estadística de "¿dónde le erra más mi modelo?" — la pregunta que decide si podés
usarlo para *ese* segmento de clientes o solo para el promedio.

## 6.4 Para qué lo usamos en esta clase

Certifica la segunda violación de supuestos: **el modelo es menos confiable justo donde los sueldos
son más altos** — que suele ser donde las decisiones importan más. Y deja una lección sobre
métricas: el RMSE global ($78.343) es un solo número y no puede decirte *dónde* fallás; este
gráfico sí.

## 6.5 Los hallazgos (verificados)

| Dato | Valor |
|---|---|
| Rango del eje X (predichos) | $64.251 a $230.265 |
| Rango del eje Y (residuos) | −$150.562 a +$1.019.539 |
| El punto extremo de arriba | la persona de $1.200.000: predicho ~$180.000, residuo +$1.019.539 |

💡 **La comparación que lo resume:** el modelo *predice* dentro de una franja de $166.014 de ancho,
pero se *equivoca* dentro de un rango de $1.170.101 (de −$150.562 a +$1.019.539). El rango del
error es **siete veces** el rango de la predicción. Ese es el retrato numérico de un R² de 0,084.

## 6.6 Curiosidades e historia

El paper que convirtió "mirar los residuos" en disciplina se llama, literalmente, *"The Examination
and Analysis of Residuals"* (Technometrics, 1963) — y lo firmaron **Anscombe y Tukey**, que además
de colegas eran **cuñados**: Anscombe se casó con Phyllis Rapp, hermana de Elizabeth, la esposa de
Tukey. Diez años después, Anscombe solo, publicó el cuarteto del gráfico 7. O sea: el señor que te
dice "mirá los residuos" y el señor que te dice "mirá el gráfico antes de creer el número" son la
misma persona, y su cuñado inventó buena parte del análisis exploratorio moderno.

## 6.7 Errores comunes

- **Buscar patrones con lupa.** En una nube de mil puntos siempre se ve "algo". Lo que cuenta son
  los patrones gruesos —embudo, curva, borde— no las manchitas.
- **Graficar residuos contra los valores REALES en vez de los predichos.** Contra el valor real
  aparece una correlación espuria por construcción (el residuo es parte del valor real) y el
  gráfico "muestra" un patrón que no significa nada. Siempre contra los **predichos**.
- **"Corregir" la heterocedasticidad porque el gráfico quedó feo.** El embudo es información, no
  suciedad: acá dice que para modelar sueldos altos hacen falta otras variables, no otro truco.

## 6.8 El código

```python
plt.scatter(dm["predicho"], dm["residuo"], alpha=0.3, color="#4C9BE8", edgecolor="none")
plt.axhline(0, color="red", linestyle="--", linewidth=2)
```

---

# 7. ⭐ El cuarteto de Anscombe

**Bloque B · `sns.lmplot(data=ans, x="x", y="y", col="dataset")`**

## 7.1 ¿Qué es?

Cuatro conjuntos de 11 puntos cada uno, fabricados a mano para compartir (casi) las mismas
estadísticas — misma media de x, misma media de y, misma recta ajustada, mismo R² — y contar cuatro
historias completamente distintas. Es la demostración formal de la regla 4 del cierre: **nunca
creas un número de regresión sin haber mirado el gráfico.**

## 7.2 Anatomía

- **`lmplot`:** scatter + recta de regresión por panel, en una sola llamada. `col="dataset"` parte
  en cuatro paneles; `col_wrap=2` los acomoda en grilla 2×2; `ci=None` apaga la banda de confianza
  para que la recta se vea limpia; `hue` le da un color a cada panel.
- Es una función *figure-level* (devuelve un `FacetGrid`): por eso en el script de figuras se
  guarda con `g.figure.savefig(...)`, distinto de todos los demás gráficos de la clase.
- **Los cuatro paneles:** (I) una relación lineal legítima con ruido; (II) una **curva** perfecta —
  relación fuerte pero no lineal; (III) una línea impecable con **un dato mal cargado** que tuerce
  la recta; (IV) todos los x iguales salvo **un punto** que fabrica la pendiente él solito.

## 7.3 Para qué sirve en la vida real

Es el argumento definitivo contra los dashboards de puro KPI: cuatro realidades operativas
distintas pueden producir el mismo número agregado. Cada vez que alguien apruebe un modelo, una
campaña o un presupuesto mirando solo el resumen, este es el gráfico que hay que poner sobre la
mesa. Funciona igual de bien con gerentes que con alumnos.

## 7.4 Para qué lo usamos en esta clase

Completa la pinza con Nicolas Cage: Cage muestra un R² alto **sin sentido**; Anscombe muestra **el
mismo R²** con cuatro realidades distintas. Entre los dos cierran las dos formas de mentirse con
una regresión. ⚠️ Logística docente verificada en el guion: `sns.load_dataset("anscombe")`
**descarga de internet** la primera vez — correr el notebook entero antes de las 10:00 deja el
dataset cacheado; si la red del aula falla, se saltea la celda (la slide 14 ya lo explicó).

## 7.5 Los hallazgos (verificados)

| Grupo | R² |
|---|---|
| I | 0,6665 |
| II | 0,6662 |
| III | 0,6663 |
| IV | 0,6667 |

Los cuatro ≈ **0,67** — idénticos hasta la tercera cifra. Según la tabla clásica del paper, los
cuatro comparten además media de x (9,0), media de y (~7,5), la recta ajustada (y = 3 + 0,5x) y la
correlación (~0,816).

💡 **El contraste con nuestra clase:** Anscombe eligió una correlación de 0,816, que da un R² de
0,67 — **ocho veces el nuestro** (0,0841). Un R² que cualquiera firmaría contento… y dos de los
cuatro paneles que lo producen son datos patológicos. Ni el R² alto te salva de mirar.

## 7.6 Curiosidades e historia

**Francis Anscombe** publicó el cuarteto en 1973 (*"Graphs in Statistical Analysis"*, The American
Statistician, 27(1), 17–21) para pelear contra un prejuicio que describió así: la creencia de que
"los cálculos numéricos son exactos, y los gráficos son burdos". Dos detalles deliciosos y
verificados: **nadie sabe cómo construyó los cuatro conjuntos** — no dejó el método por escrito — y
fundó el departamento de estadística de Yale en 1963, el mismo año del paper de residuos con su
cuñado Tukey. La secuela moderna existe: en 2017, Matejka y Fitzmaurice (Autodesk) generaron por
computadora docenas de datasets con las mismas estadísticas y formas arbitrarias — el famoso
**Datasaurus**: mismos números, y el scatter es un dinosaurio.

## 7.7 Errores comunes

- **Quedarse en "qué curioso".** La regla operativa no es "los números mienten": es "los números
  **resumen**, y un resumen idéntico puede venir de realidades distintas". Sin esa frase, el
  cuarteto es un truco de salón.
- **Creer que R² + p-valor juntos ya te cubren.** Con la misma correlación y el mismo n, el
  p-valor también sale igual en los cuatro paneles. Agregar números-resumen no reemplaza mirar.
- **Pensar que son datos reales.** Son fabricados con propósito didáctico; los datasets reales
  fallan de formas más sutiles — la nuestra, por ejemplo: R² honesto pero inútil.

## 7.8 El código

```python
ans = sns.load_dataset("anscombe")
g = sns.lmplot(data=ans, x="x", y="y", col="dataset", hue="dataset", col_wrap=2,
               height=3, ci=None, scatter_kws={"s": 60, "alpha": 0.8})
```

---

# 8. La zona con datos y la extrapolación

**Bloque D "Hasta dónde le podés creer" (solo versión completa) · la recta en dos colores**

## 8.1 ¿Qué es?

La misma nube y la misma recta del gráfico 2, pero la recta cambia de disfraz donde se acaban los
datos: **verde sólida de 0 a 50 años** (zona con datos) y **roja punteada de 50 a 80** (la leyenda
lo dice sin eufemismos: *"Extrapolación: pura fe"*).

## 8.2 Anatomía

- **Dos `linspace`:** `x_linea` cubre del mínimo al máximo real; `x_fuera` sigue del máximo hasta
  80. La fórmula es **la misma** (`b0 + b1 * x`) en los dos tramos: la matemática no distingue.
- **El color y el punteado son convención humana agregada a mano.** El modelo no sabe dónde
  termina su conocimiento; la frontera la tenés que dibujar vos. Esa es la moraleja técnica del
  gráfico.
- **El eje X ahora llega a 80** (en los gráficos 1 y 2 moría en 50). Estirar el eje crea el espacio
  vacío de la derecha: esa zona sin un solo punto celeste **es** el mensaje.

## 8.3 Para qué sirve en la vida real

Pandemias proyectadas con exponenciales eternas, ventas proyectadas fuera de temporada, y el
ejemplo de la literatura que cita el notebook: un modelo de precios de casas al que le piden el
precio de un lote vacío y contesta **−$522.202**. En ML de producción tiene nombre propio — *data
drift*: el mundo se corre fuera del rango de entrenamiento y el modelo sigue contestando con la
misma cara de seguro.

## 8.4 Para qué lo usamos en esta clase

Cierra la pregunta del bloque extra de la versión completa: *¿hasta dónde le podés creer?* El
modelo contesta **siempre** — para −5 años y para 80 — y la regla queda enunciada: un modelo solo
vale dentro del rango en el que fue entrenado. Fuera de ahí no está prediciendo: **está
inventando.**

## 8.5 Los hallazgos (verificados)

| Años de experiencia | Predicción del modelo |
|---|---|
| **−5** (no existe) | **$47.651** |
| 0 | $64.251 |
| 25 | $147.258 |
| 50 (el máximo real) | $230.265 |
| **80** (no existe) | **$329.874** |

Rango real de los datos: **0 a 50 años**.

💡 **Dos lecturas para el aula.** La predicción para **−5 años** ($47.651) es el absurdo perfecto:
el modelo le pone sueldo a alguien que va a empezar a programar dentro de cinco años — y lo
peligroso es que el número *suena creíble*. Y la extrapolación a 80 años ($329.874) ni siquiera
alcanza a los datos reales: en la tabla ya hay alguien ganando $1.200.000 con 35 años de
experiencia — menos de un tercio de ese sueldo es lo máximo que la recta se anima a imaginar para
una carrera imposible de 80 años. La dispersión vertical le gana a la pendiente hasta en el
disparate.

## 8.6 Curiosidades e historia

La burla más vieja y más citada contra la extrapolación no es de un estadístico: es de **Mark
Twain**, en *Life on the Mississippi* (1883). Twain toma la tasa a la que el bajo Mississippi venía
acortándose por los cortes de meandros, la proyecta linealmente, y "demuestra" que en unos
setecientos años el río va a medir menos de dos millas — con Cairo y Nueva Orleans compartiendo
vereda. Y remata con la frase que esta clase podría poner de epígrafe: *"la ciencia tiene algo
fascinante: una inversión tan insignificante de hechos rinde ganancias tan mayoristas de
conjetura"* (traducción nuestra). Todo gráfico de extrapolación dibujado desde entonces es una
nota al pie de ese párrafo.

## 8.7 Errores comunes

- **Creer que el peligro arranca en el año 51.** En los bordes del rango (0–2 años, 40–50) ya hay
  poquísimos datos sosteniendo la recta — mirá qué ralos son los puntos después de 30. La
  extrapolación es un continuo, no un interruptor.
- **Confiar porque "la fórmula sigue funcionando".** La fórmula funciona siempre; **ese es el
  problema.** Ni statsmodels ni sklearn tiran error ni warning por predecir a 80 años: es el fallo
  silencioso del Bloque C, otra vez.
- **El gemelo temporal.** "Si las ventas crecieron 10% por mes, en tres años…" es la misma trampa
  con otra ropa: extrapolar una tendencia en el tiempo fuera del rango observado.

## 8.8 El código

```python
x_fuera = np.linspace(df["experiencia"].max(), 80, 100)
plt.plot(x_linea, b0 + b1 * x_linea, color="darkgreen", linewidth=3, label="Zona con datos")
plt.plot(x_fuera, b0 + b1 * x_fuera, color="red", linewidth=3, linestyle="--",
         label="Extrapolación: pura fe")
```

---

# 9. La recta que se escapa

**Bloque E (D en la Mini) · `smf.ols("es_us ~ salario")` — el error a propósito, en versión dibujo**

## 9.1 ¿Qué es?

La variable a predecir ya no es un sueldo: es **`es_us`, que solo vale 0 o 1** (¿vive en Estados
Unidos?). Los puntos forman dos "rieles" horizontales — todo el mundo vive en el riel de abajo o en
el de arriba — y encima hay una regresión **lineal** ajustada a eso, deliberadamente mal: la
leyenda la confiesa (*"Una RECTA (mal)"*).

## 9.2 Anatomía

- **Los dos rieles:** los puntos solo pueden vivir en dos alturas. El `alpha` vuelve a trabajar de
  densímetro: el riel de abajo (no-US) es denso hasta ~$200.000; el de arriba (US) se puebla más a
  la derecha.
- **Las `axhline` punteadas en 0 y 1** marcan la cancha donde una probabilidad tiene permitido
  vivir.
- **La recta roja ignora la cancha:** cruza el techo del 1 y sigue de largo; en el extremo derecho
  del eje ($1.200.000) llega a **3,35** — un "335% de probabilidad". Y estirada apenas hacia
  sueldos más bajos cruza el cero (−0,0007 en `salario = 0`): apenas, pero lo cruza, y una
  probabilidad negativa tampoco existe.

## 9.3 Para qué sirve en la vida real

El problema binario es medio machine learning aplicado: ¿paga o no paga?, ¿se va o se queda?, ¿es
fraude o no? Este gráfico enseña el instinto de chequear **qué rango de salida** tiene sentido
antes de elegir la herramienta. (Nota honesta: en econometría existe el "modelo de probabilidad
lineal", que usa esta recta **a sabiendas** de sus límites — la diferencia entre un error y un
método es saber exactamente qué estás haciendo.)

## 9.4 Para qué lo usamos en esta clase

Es el **error a propósito** del bloque de logística, en versión visual: la casa siempre muestra el fallo antes
que la herramienta. La pregunta 🤔 (*"¿qué sale mal si una recta predice algo que solo vale 0 o
1?"*) se contesta con este dibujo, y la necesidad de la sigmoide queda montada. Por diseño del
guion, además, es el primer gráfico del bloque que se sacrifica si el reloj aprieta: se puede
contar sin proyectarlo.

## 9.5 Los hallazgos (verificados)

| Dato | Valor |
|---|---|
| Personas | 1.183 — **884 no-US y 299 US** (sumas de la matriz del gráfico 11) |
| Proporción no-US (la "base") | **74,7%** |
| La recta en el extremo derecho del eje ($1.200.000) | **+3,3529** ("335% de probabilidad") |
| La recta en `salario = 0` | **−0,0007** — cruza el cero, apenas |

💡 **El detalle que nadie ve solo:** el sueldo más alto de toda la tabla — $1.200.000 — está en el
riel de **abajo**: esa persona **no** vive en Estados Unidos. Como la sigmoide del gráfico
siguiente es creciente y a $250.000 ya asigna 93,5%, para $1.200.000 prácticamente jura que es
estadounidense — y no lo es. Sirve para el remate del bloque: hasta el modelo que funciona se
equivoca con soltura en los casos individuales.

## 9.6 Curiosidades e historia

El problema de meter un sí/no en una regresión tiene su propia guerra de escuelas. **Chester Bliss
(1934)** inventó el *probit* ("probability unit") para curvas de dosis-mortalidad de insecticidas —
la pregunta era, literalmente, con cuánto veneno muere la mitad de los bichos. Diez años después,
**Joseph Berkson (1944)** propuso la alternativa basada en la curva logística y la bautizó *logit*
("logistic unit"), calcando el nombre a propósito para pelearle al probit. La pelea logit-vs-probit
duró décadas; el logit terminó ganando en buena parte por comodidad de cálculo, y hoy es el corazón
de la regresión logística que usás en la celda siguiente.

## 9.7 Errores comunes

- **Creer que la recta "está rota".** La recta hace exactamente lo que se le pidió: minimizar
  residuos al cuadrado. Lo roto es **el pedido**. El error fue nuestro, de elección de herramienta.
- **Arreglarlo recortando** ("si da más de 1, lo dejo en 1"). Es un parche sin justificación que
  amontona predicciones en los bordes. Para este problema existe una función diseñada a medida — es
  el gráfico 10.
- **Leer el riel de arriba como "los que ganan más de X viven en EEUU".** Hay estadounidenses
  ganando poco y no-estadounidenses ganando muchísimo (el de $1.200.000, sin ir más lejos). Los
  rieles se solapan en casi todo el rango.

## 9.8 El código

```python
dl["es_us"] = (dl["us_or_not"] == "US").astype(int)
recta = smf.ols("es_us ~ salario", data=dl).fit()
x_s = np.linspace(0, dl["salario"].max(), 200)
plt.plot(x_s, recta.params["Intercept"] + recta.params["salario"] * x_s, color="red",
         linewidth=2.5, label="Una RECTA (mal)")
```

---

# 10. La sigmoide

**Bloque E (D en la Mini) · `smf.logit("es_us ~ salario")` — la curva S**

## 10.1 ¿Qué es?

Los mismos dos rieles de puntos, pero la curva verde ahora es una **regresión logística**: arranca
pegada al 0, sube, y se aplana pegada al 1. La forma de S se llama *sigmoide*. Ya no predice
sueldos: predice **la probabilidad de vivir en EEUU dado el sueldo**, y esa probabilidad vive,
siempre, entre 0 y 1.

## 10.2 Anatomía

- **`smf.logit(...)`:** misma notación de fórmula que `ols`; cambia la máquina de adentro.
  `disp=0` solo silencia el log del optimizador.
- **La grilla:** 300 sueldos equiespaciados de $0 a $400.000, con su probabilidad predicha. Por
  eso **la curva se corta en $400.000** aunque el eje siga hasta $1.200.000 — a la derecha quedan
  puntos sin curva encima. No es un bug: es hasta dónde decidimos dibujarla.
- **Las asíntotas:** la curva nunca toca el 0 ni el 1, solo se les acerca. El modelo jamás firma
  una certeza absoluta.
- **La pendiente máxima está en el medio** (donde P ≈ 50%): ahí cada dólar mueve la probabilidad
  al máximo. En las puntas, la curva es casi plana: cerca de la certeza, más evidencia casi no
  mueve nada.

## 10.3 Para qué sirve en la vida real

La sigmoide es LA función de los clasificadores: scoring crediticio, spam, riesgo médico, churn. Y
es el puente directo al resto de la certificación: en el Módulo 4 la van a reencontrar como
**función de activación** de las redes neuronales — una regresión logística es, en términos
prácticos, una red neuronal de una sola neurona. Lo que hoy es una curva verde va a ser, en
septiembre, el ladrillo de un deep learning.

## 10.4 Para qué lo usamos en esta clase

Resuelve el fallo del gráfico 9 y produce **el único modelo de la clase que funciona**: exactitud
del 84,8% contra una base del 74,7%. Y deja el cierre incómodo del guion: que funcione no lo vuelve
causal — los sueldos de EEUU son más altos, y eso es un hecho, no una relación causa-efecto.

## 10.5 Los hallazgos (verificados)

| Sueldo | P(vivir en EEUU) |
|---|---|
| $20.000 | **4,0%** |
| $60.000 | 10,4% |
| $150.000 | **53,2%** |
| $250.000 | 93,5% |

Exactitud del modelo: **84,8%** · base (decir siempre "no-US"): **74,7%**.

💡 **La curva leída con números:** entre $60.000 y $150.000 la probabilidad **se quintuplica**
(10,4% → 53,2%) — esa es la zona de pendiente máxima, y el cruce del 50% cae justo antes de los
$150.000. De $250.000 en adelante, en cambio, la curva ya está saturada: quedan 6,5 puntos de techo
por ganar aunque cuadrupliques el sueldo. La misma evidencia (un dólar más) vale muchísimo en el
medio y casi nada en las puntas: eso es tener forma de S.

## 10.6 Curiosidades e historia

La sigmoide nació **contando belgas**. Pierre-François Verhulst, matemático de Bruselas que
trabajaba con Adolphe Quetelet, propuso en 1838 su ecuación para el crecimiento de poblaciones —
contra la exponencial eterna: las poblaciones crecen hasta un techo. Hacia 1845 la bautizó
*logistique* y **nunca explicó por qué**; el origen del nombre sigue siendo un misterio de la
historia de la matemática. Con su curva estimó el techo de población de Bélgica: **9.400.000
habitantes** (hoy pasa de los 11 millones — nada mal para un pronóstico de 1845). Murió a los 44 y
su trabajo quedó olvidado unos ochenta años, hasta que Pearl y Reed (1920) reinventaron la misma
curva para la población de Estados Unidos sin conocerlo; recién en 1922 Pearl descubrió que un
belga había llegado casi un siglo antes. La función que van a ver como neurona en el Módulo 4
empezó su carrera prediciendo censos.

## 10.7 Errores comunes

- **Leer la salida como clase ("US"/"no-US").** El modelo devuelve una **probabilidad**; el corte
  en 0,5 es una convención nuestra, y la versión completa lo demuestra moviéndolo de 0,20 a 0,80.
- **Creer que la curva "dibuja los datos".** Los puntos siguen viviendo en 0 y 1. La curva dibuja
  P(US | sueldo), un objeto que no se ve en ningún punto individual — igual que la recta del
  gráfico 2 no pasaba por casi ningún punto.
- **Interpretar el coeficiente como en la lineal** ("$1 más = tanto % más"). El efecto de un dólar
  **depende de dónde estés en la curva**: enorme cerca del 50%, casi nulo en las colas. Los cuatro
  P(US) verificados de arriba son la demostración.
- **Aplaudir el 84,8% sin la base.** Adivinando siempre "no-US" ya sacás 74,7%. La exactitud sin
  su base es un número sin contexto — y esa es la puerta de entrada al gráfico 11.

## 10.8 El código

```python
logistica = smf.logit("es_us ~ salario", data=dl).fit(disp=0)
grilla = pd.DataFrame({"salario": np.linspace(0, 400_000, 300)})
grilla["prob"] = logistica.predict(grilla)
plt.plot(grilla["salario"], grilla["prob"], color="darkgreen", linewidth=3,
         label="Regresión LOGÍSTICA")
```

---

# 11. La matriz de confusión

**Bloque E (solo versión completa) · `confusion_matrix(...)` + `sns.heatmap(mc, fmt="d")`**

## 11.1 ¿Qué es?

Una tabla de 2×2 que cruza **la realidad** (filas) contra **la predicción** del modelo logístico
con corte en 0,5 (columnas). Cuatro casilleros: los dos de la diagonal son los aciertos; los otros
dos son **los dos tipos de error**, que casi nunca cuestan lo mismo.

## 11.2 Anatomía

- **Filas = verdad** ("Era: no-US" / "Era: US"); **columnas = predicción**. Es la convención de
  sklearn (fila 0 = clase 0). ⚠️ Otras herramientas y libros la transponen: **leé siempre las
  etiquetas antes que los números.**
- **`fmt="d"`:** enteros — acá se cuentan personas, no proporciones.
- **`cbar=False`:** con cuatro celdas, la barra de color no aporta nada.
- **El color como delator:** la celda 838 es azul oscurísimo y arrastra la escala entera; las otras
  tres quedan pálidas. El desbalance de clases se ve como desbalance de color antes de leer un solo
  número.

## 11.3 Para qué sirve en la vida real

Es el tablero de control de todo clasificador en producción, porque permite ponerle **costo** a
cada error por separado: el falso negativo del banco (fraude que pasó) no cuesta lo mismo que el
falso positivo (cliente honesto molestado); en un diagnóstico médico, al revés de un filtro de
spam. Decidir cuál error tolerás **es una decisión de negocio, no de estadística** — y sin esta
tabla ni siquiera podés plantearla.

## 11.4 Para qué lo usamos en esta clase

Desarma el "84,8% suena bien" de la versión en vivo. La exactitud es una compresión de esta tabla,
y la tabla descomprimida cuenta otra cosa: el modelo es excelente detectando no-US (838 de 884) y
mediocre detectando US (165 de 299 — poco más de la mitad). De estas cuatro celdas nacen precisión
y sensibilidad (el `classification_report` que sigue en el notebook), y la lección final del
módulo: **hay que saber en qué dirección te equivocás.**

## 11.5 Los hallazgos (verificados)

Matriz verificada: `[[838, 46], [134, 165]]` (filas = real no-US/US; columnas = predicho no-US/US).

| Celda | Cantidad | Nombre |
|---|---|---|
| Era no-US, predijo no-US | **838** | verdaderos negativos |
| Era no-US, predijo US | 46 | **falsos positivos** |
| Era US, predijo no-US | 134 | **falsos negativos** |
| Era US, predijo US | **165** | verdaderos positivos |

Sumas (aritmética de la propia matriz): 884 no-US reales, 299 US reales, 1.183 en total; aciertos
838 + 165 = **1.003 de 1.183 = 84,8%** — la exactitud verificada sale de la diagonal.

💡 **El hallazgo es la asimetría:** 134 falsos negativos contra 46 falsos positivos. El modelo es
conservador: ante la duda dice "no-US", que es la clase mayoritaria. De los 299 estadounidenses
reales se le escapan 134 — **encuentra apenas 165 de 299, el 55%**. El 84,8% global esconde un
modelo que a la clase minoritaria la detecta poco mejor que una moneda. Esa frase es la respuesta
modelo del Foro 3.

## 11.6 Curiosidades e historia

La tablita tiene **dos genealogías** que se fundieron. El nombre viene de la psicología
experimental: matrices de confusión para registrar **qué estímulos se confunden con cuáles** — el
clásico es Miller y Nicely (1955), que les hicieron escuchar 16 consonantes del inglés con ruido a
sus sujetos y tabularon cada confusión (ese George Miller es el mismo del "número mágico 7±2"). Y
los conceptos de los casilleros — hit, miss, falsa alarma — vienen de los **operadores de radar de
la Segunda Guerra Mundial** (desde 1941): decidir si el punto en la pantalla era un bombardero o
una bandada de gansos, balanceando detecciones reales contra falsas alarmas. De ahí salieron
también las curvas ROC que van a ver en el Módulo 3. Psicólogos y militares, cuatro casilleros.

## 11.7 Errores comunes

- **Leer la matriz sin mirar las etiquetas.** La misma tabla transpuesta cuenta otra historia:
  nuestros 134 falsos negativos pasarían a leerse como falsos positivos. Primero las etiquetas,
  después los números — siempre.
- **Reportar solo exactitud con clases desbalanceadas.** Con 74,7% de no-US, un "modelo" que dice
  siempre no-US ya saca 74,7% sin haber aprendido nada. La exactitud sola es publicidad.
- **Creer que el corte de 0,5 es parte del modelo.** Movés el umbral y **toda la matriz cambia**
  sin re-entrenar nada — la versión completa lo muestra con umbrales de 0,20 a 0,80. Elegir ese
  punto es de las decisiones más importantes y más ignoradas del oficio.
- **Mezclar precisión con sensibilidad.** Una divide por columna ("de los que marqué como US,
  ¿cuántos eran?") y la otra por fila ("de los que eran US, ¿a cuántos encontré?"). Son preguntas
  distintas, salen de la misma tabla, y confundirlas arruina reportes enteros.

## 11.8 El código

```python
mc = confusion_matrix(dl["es_us"], (logistica.predict(dl) > 0.5).astype(int))
sns.heatmap(mc, annot=True, fmt="d", cmap="Blues", cbar=False,
            xticklabels=["Predijo: no-US", "Predijo: US"],
            yticklabels=["Era: no-US", "Era: US"])
```

---

## Anexo — tabla maestra de cifras de los gráficos

Todas verificadas ejecutando contra `data_dev.csv` (1.183 filas). **Si algún documento de esta
clase dice otra cosa, el documento está mal.**

| Gráfico | Cifra | Valor |
|---|---|---|
| 1 | Puntos / rango X / rango Y | 1.183 / 0–50 años / $3–$1.200.000 |
| 1 | Sueldo: media / mediana | $90.684 / $72.714 |
| 1–2 | Correlación salario ~ experiencia | **0,2900** (y 0,29² = 0,0841 = R²) |
| 2 | β₀ / β₁ / p / R² | $64.251 / $3.320 por año / 2,32e-24 / 0,0841 |
| 2 | Predicción a 5 años / intervalo honesto | **$80.853** / $2.510 – $159.195 |
| 3 | Correlaciones entre predictores | 0,891 · 0,864 · 0,795 |
| 3 | R² múltiple / p de experiencia en la múltiple | 0,0900 / **0,174** |
| 4 | RMSE / MAE / RMSE ÷ promedio | **$78.343** / $50.794 / **86%** |
| 4–5 | Residuos: media / desvío / mín / máx / skew | 0,0000 / $78.376 / −$150.562 / +$1.019.539 / **4,16** |
| 6 | Rango de predichos (0 a 50 años) | $64.251 – $230.265 |
| 7 | R² de Anscombe I / II / III / IV | 0,6665 / 0,6662 / 0,6663 / 0,6667 |
| 8 | Extrapolación: −5 / 0 / 25 / 50 / 80 años | $47.651 / $64.251 / $147.258 / $230.265 / $329.874 |
| 9–10 | Exactitud / base | **84,8%** / 74,7% |
| 10 | P(US): $20k / $60k / $150k / $250k | 4,0% / 10,4% / 53,2% / 93,5% |
| 11 | Matriz de confusión | [[838, 46], [134, 165]] → 1.003 aciertos de 1.183 |
| — | Cage vs. inspectores TSA (Dakota del Norte) | r² = 0,814 · p = 0,00014 |

---

## Fuentes de las curiosidades y advertencias

- **Anscombe, F. J. (1973).** "Graphs in Statistical Analysis." *The American Statistician*, 27(1),
  17–21. — el cuarteto, y la cita sobre cálculos "exactos" y gráficos "burdos".
- **Anscombe, F. J. & Tukey, J. W. (1963).** "The Examination and Analysis of Residuals."
  *Technometrics*, 5(2), 141–160. — el paper fundacional de los gráficos 5 y 6.
- **Galton, F. (1886).** "Regression towards Mediocrity in Hereditary Stature." *Journal of the
  Anthropological Institute*, 15, 246–263. — el origen del término "regresión".
- **Stigler, S. M. (1981).** "Gauss and the Invention of Least Squares." *The Annals of
  Statistics*, 9(3). — la disputa Legendre-Gauss y la historia de Ceres.
- **Friendly, M. & Denis, D. (2005).** "The Early Origins and Development of the Scatterplot."
  *Journal of the History of the Behavioral Sciences*, 41(2), 103–130. — Herschel 1833.
- **MacTutor History of Mathematics (Univ. St Andrews),** biografía de P.-F. Verhulst. — el techo
  de 9.400.000 belgas y el redescubrimiento de Pearl y Reed.
- **Bliss, C. I. (1934)** en *Science* (el probit) y **Berkson, J. (1944).** "Application of the
  Logistic Function to Bio-Assay." *JASA*, 39(227). — el bautismo del logit.
- **Miller, G. A. & Nicely, P. E. (1955).** "An Analysis of Perceptual Confusions Among Some
  English Consonants." *JASA*, 27, 338–352. — la matriz de confusión en psicología.
- **Matejka, J. & Fitzmaurice, G. (2017).** "Same Stats, Different Graphs." *CHI 2017* (Autodesk
  Research). — el Datasaurus.
- **Twain, M. (1883).** *Life on the Mississippi*, cap. 17. — la extrapolación del río.
- **James, G., Witten, D., Hastie, T. & Tibshirani, R.** *An Introduction to Statistical Learning.*
  — la "absolución" del R² bajo en dominios ruidosos (citada en el notebook).
- **Bruce, P., Bruce, A. & Gedeck, P. (2020).** *Practical Statistics for Data Scientists*, 2ª ed.
  O'Reilly. Cap. 4. — el lote vacío de −$522.202.
- **tylervigen.com** — Cage vs. TSA Dakota del Norte (r² = 0,814, p = 0,00014, verificado).
