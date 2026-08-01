# Guía visual — los 6 gráficos de la Clase 2

> Módulo 2 · Clase 2 — Estadística Inferencial · UTEPSA / Khipus.ai
> **Para el docente antes de dictar** (poder responder cualquier pregunta sobre el gráfico) y **para
> el alumno después** (volver sobre algo que pasó rápido).
>
> Todas las cifras de este documento fueron **calculadas ejecutando código contra `data_dev.csv`**,
> con las mismas semillas que fija el notebook. Ninguna está estimada.

---

## Prólogo: ¿por qué esta clase es tan visual?

La Clase 1 usaba gráficos para **describir** lo que ya teníamos: así es el precio, así se reparte el
rating. Hoy los usamos para algo distinto y más raro: **para ver cosas que no existen en los datos.**

El histograma del Bloque G no muestra sueldos. Muestra **2000 mundos alternativos** en los que "usar
IA" no tiene ningún efecto, y qué diferencia produciría el puro azar en cada uno. Ninguno de esos
2000 mundos es real. Y sin embargo, es mirándolos como decidimos si lo que vimos en el mundo real
significa algo.

Esa es la idea que la clase entera intenta instalar, y es difícil de explicar con palabras. Por eso
la estrategia de hoy es: **primero se ve, después se nombra.** Cuando el alumno ya vio aparecer la
campana con sus propios ojos, decirle "esto se llama Teorema del Límite Central" cuesta diez
segundos. Al revés, cuesta una clase entera y no queda.

⚠️ **La advertencia que atraviesa toda esta guía.** Hay un estudio publicado (Watkins, Bargagliotti
& Franklin, *Journal of Statistics Education*, 2014) que documenta algo incómodo: **simular
distribuciones muestrales puede enseñar activamente una idea falsa**. Nueve profesores con
experiencia dando estadística cayeron en la trampa. Está detallada en el gráfico 4, que es donde
aparece. No la saltees.

---

## Cómo usar esta guía

Los 6 gráficos, en el orden en que aparecen en la clase:

| # | Gráfico | Bloque | Qué tiene que quedar |
|---|---|---|---|
| 1 | Histograma binomial (caras en 20 tiradas) | B | Que el azar repetido produce forma |
| 2 | Histograma normal estándar | B | Cómo se ve una campana "de verdad" |
| 3 | Histograma de sueldos crudos | D | Que los datos reales NO son normales |
| 4 | **Panel de 3 histogramas apilados** | D | **El corazón conceptual de la clase** |
| 5 | Histograma bootstrap + intervalo de confianza | F | Que una estimación es un rango, no un punto |
| 6 | **Histograma de permutación + línea observada** | G | **La definición visual del p-value** |

Los gráficos **4 y 6 son los irrenunciables**. Si la clase se desborda y hay que apurar algo, apurá
los otros cuatro.

---

# 1. El histograma binomial

**Bloque B · `sns.histplot(caras_por_experimento, discrete=True)`**

## 1.1 ¿Qué es?

Un histograma cuenta **cuántas veces cayó cada resultado**. Acá, cada uno de los 5000 experimentos
consistió en tirar una moneda 20 veces y anotar cuántas caras salieron. El gráfico muestra cuántos
de esos 5000 experimentos dieron 7 caras, cuántos dieron 8, y así.

## 1.2 Anatomía

- **Eje X:** cuántas caras salieron (de 0 a 20 posibles).
- **Eje Y:** en cuántos de los 5000 experimentos pasó eso.
- **`discrete=True`:** le dice a seaborn que los valores son números enteros contables, así que
  dibuja **una barra separada por valor** en vez de agrupar en rangos continuos. Sin ese parámetro,
  seaborn parte el eje en intervalos arbitrarios y aparecen huecos o barras dobles que confunden.

## 1.3 Para qué sirve en la vida real

Cualquier proceso de "cuántos éxitos en N intentos" tiene esta forma: cuántos clientes de 200
compran, cuántos servidores de 50 fallan esta semana, cuántos correos de 1000 rebotan. Si conocés la
tasa base, sabés qué es normal y qué es una anomalía que merece una llamada.

## 1.4 Para qué lo usamos en esta clase

Es la **primera vez que aparece la campana**, y aparece de la nada: nadie la puso ahí, salió sola de
tirar una moneda muchas veces. Sirve para que el Bloque D no sea el primer contacto con la idea de
que la repetición produce forma.

## 1.5 Los hallazgos (verificados)

| Dato | Valor |
|---|---|
| Rango realmente observado | **3 a 17 caras** — nunca salieron 0, 1, 2, ni 18, 19, 20 |
| Barras distintas dibujadas | 15 (de 21 valores posibles) |
| Moda | **10 caras**, en 903 de los 5000 experimentos |
| Concentración | **73,5%** de los experimentos dieron entre 8 y 12 caras |

💡 **El dato que sorprende:** en 5000 experimentos **nunca** salieron 20 caras seguidas. Tiene
sentido — la probabilidad es 1 en 1.048.576 — pero verlo hace concreto por qué "improbable" no es
"imposible": si hiciéramos un millón de experimentos, aparecería.

## 1.6 Curiosidades e historia

La distribución binomial la formalizó **Jacob Bernoulli** en *Ars Conjectandi*, publicado en 1713 —
ocho años después de su muerte. El libro también contiene la primera versión de la ley de los
grandes números. Bernoulli tardó veinte años en escribirlo y nunca lo terminó.

## 1.7 Errores comunes

- **Creer que "20 tiradas" es el eje X.** No: el eje X es *cuántas caras*, y cada barra resume un
  experimento completo de 20 tiradas. Es el error de lectura más frecuente en este gráfico.
- **Esperar que la barra de 10 tenga exactamente 5000/21 experimentos.** No es un reparto parejo:
  la campana concentra en el centro.
- **Confundir esta campana con la normal.** Esta es discreta (solo enteros) y acotada (0 a 20). La
  normal es continua e infinita. Se parecen, no son la misma.

## 1.8 El código

```python
np.random.seed(2)
caras_por_experimento = [np.random.binomial(n=20, p=0.5) for _ in range(5000)]
plt.figure(figsize=(7, 4))
sns.histplot(caras_por_experimento, discrete=True, color="#2E5496")
```

---

# 2. El histograma de la normal estándar

**Bloque B · `sns.histplot(normal_sim, kde=True)`**

## 2.1 ¿Qué es?

10.000 valores generados de una distribución normal con media 0 y desviación 1 — la **normal
estándar**, la campana de referencia contra la que se comparan todas las demás.

## 2.2 Anatomía

- **`kde=True`:** superpone una curva suavizada (*Kernel Density Estimate*). No es la fórmula
  teórica de la campana: es una estimación suavizada **de los datos simulados**. Si los datos fueran
  torcidos, la curva saldría torcida.
- **El centro en 0 y el ancho de 1** son los dos únicos parámetros que definen la forma.

## 2.3 Para qué sirve en la vida real

Es la escala universal de comparación. Convertir cualquier medición a "cuántas desviaciones estándar
está del promedio" permite comparar cosas de unidades distintas: un alumno que sacó 2σ por encima en
matemática y 0,5σ en lengua rindió mejor en matemática, aunque las notas crudas digan otra cosa.

## 2.4 Para qué lo usamos en esta clase

Como **contraste**. Se muestra la campana perfecta justo antes de mostrar el histograma de sueldos
reales (gráfico 3), para que el choque sea obvio.

## 2.5 Los hallazgos (verificados)

La famosa **regla 68-95-99.7** sobre nuestros 10.000 valores simulados:

| Intervalo | Real | Teórico | Diferencia |
|---|---|---|---|
| ±1σ | **68,61%** | 68,27% | +0,34 |
| ±2σ | **95,65%** | 95,45% | +0,20 |
| ±3σ | **99,74%** | 99,73% | +0,01 |

💡 **El callback a la Clase 1 que vale oro.** En la Clase 1 hicimos esta misma cuenta sobre los
precios de Airbnb y dio **83,6% donde debía dar 68,3%** — un desastre. Acá da casi perfecto. La
diferencia no es la fórmula: es que **estos datos sí son normales porque los generamos normales, y
los de Airbnb no lo eran.** La regla no es mágica; es condicional, y la condición se verifica.

## 2.6 Curiosidades e historia

Se la llama "gaussiana" por Gauss (1809), pero **Abraham de Moivre la había derivado en 1733**, casi
ochenta años antes, como aproximación a la binomial — o sea, exactamente el vínculo entre el gráfico
1 y este. De Moivre publicó el resultado en un panfleto privado en latín y murió pobre.

## 2.7 Errores comunes

- **Creer que la curva `kde` es "la teoría".** No lo es: es un suavizado de los datos. Si simulás
  algo torcido con `kde=True`, la curva sale torcida.
- **Pensar que "normal" significa "común" o "correcto".** Es un nombre desafortunado: la mayoría de
  los fenómenos económicos y sociales **no** son normales.
- **Aplicar la regla 68-95-99.7 sin verificar normalidad.** Es el error que la Clase 1 dejó expuesto.

## 2.8 El código

```python
np.random.seed(3)
normal_sim = np.random.normal(loc=0, scale=1, size=10000)
plt.figure(figsize=(7, 4))
sns.histplot(normal_sim, kde=True, color="#2E7D32")
```

---

# 3. El histograma de sueldos crudos

**Bloque D · `sns.histplot(df["converted_comp_yearly"], bins=40)`**

## 3.1 ¿Qué es?

Los 1183 sueldos anuales reales de la encuesta, sin ningún tratamiento, repartidos en 40 intervalos.

## 3.2 Anatomía

- **`bins=40`:** parte el rango completo (de $3 a $1.200.000) en 40 tramos iguales de ~$30.000 cada
  uno. La elección del número de bins **cambia cómo se ve** el gráfico: con 10 bins la asimetría se
  ve menos, con 100 aparece ruido.
- **La cola larga a la derecha** es la firma visual de la asimetría positiva.

## 3.3 Para qué sirve en la vida real

Es el primer gráfico que hay que hacer con cualquier variable de dinero: sueldos, ventas por cliente,
tiempos de espera, tamaños de transacción. Todas comparten esta forma, y todas rompen los métodos
que asumen simetría.

## 3.4 Para qué lo usamos en esta clase

Para **establecer el problema** antes de resolverlo. El alumno tiene que ver con sus ojos que estos
datos no se parecen en nada al gráfico 2, y preguntarse cómo se hace estadística seria con esto. El
gráfico 4 responde.

## 3.5 Los hallazgos (verificados)

| Dato | Valor |
|---|---|
| Rango | **$3** a **$1.200.000** (sí, hay alguien que reportó $3) |
| Mediana | $72.714 |
| Media | $90.684 — **$18.000 por encima de la mediana** |
| Asimetría (skew) | **4,19** (una normal perfecta da 0) |
| Primera barra (hasta ~$30.000) | **203 personas, el 17,2%** |
| Por encima de $300.000 | 21 personas, **1,78%** |

💥 **El hallazgo que no está en el notebook y conviene tener a mano:** de las **40 barras del
histograma, 20 están completamente vacías.** La mitad derecha del gráfico es aire. Ese espacio en
blanco no es un defecto del gráfico — **es el dato**: significa que hay un puñado de sueldos tan
extremos que estiran el eje X hasta un punto donde ya no hay casi nadie. Si algún alumno pregunta
"¿por qué el gráfico se ve tan vacío a la derecha?", esa es la respuesta, y es una de las mejores
preguntas que puede hacer.

## 3.6 Curiosidades e historia

La forma de las distribuciones de ingreso la estudió **Vilfredo Pareto** en 1896, y de ahí sale el
"principio de Pareto" (el 80/20). Pareto era ingeniero y economista, y llegó a la estadística
mirando registros de propiedad en Italia.

## 3.7 Errores comunes

- **Reportar la media como "el sueldo típico".** Con skew 4,19, la media está inflada por unos pocos.
  Es exactamente la lección de la Clase 1, ahora con otro dataset.
- **Cambiar `bins` hasta que el gráfico "se vea bien".** Eso es elegir la conclusión antes del dato.
- **Borrar los outliers porque "arruinan el gráfico".** Los 21 sueldos por encima de $300.000 son
  personas reales con trabajos reales.

## 3.8 El código

```python
plt.figure(figsize=(7, 4))
sns.histplot(df["converted_comp_yearly"], bins=40, color="#B45309")
print(f"Asimetría (skew): {stats.skew(df['converted_comp_yearly']):.2f}")
```

---

# 4. ⭐ El panel de 3 histogramas apilados

**Bloque D · el gráfico más importante de la clase**

## 4.1 ¿Qué es?

Tres histogramas, uno debajo del otro, **de los mismos datos**:

1. Los 1183 sueldos individuales.
2. Las medias de 1000 muestras de **5** personas cada una.
3. Las medias de 1000 muestras de **20** personas cada una.

## 4.2 Anatomía

Es el único gráfico de la clase que usa `fig, axes = plt.subplots(3, 1)`. Hace falta porque son tres
paneles y hay que decirle a cada histograma en cuál dibujarse (`ax=axes[0]`, `axes[1]`, `axes[2]`).
Para un gráfico solo alcanza `plt.figure()`, que es lo que usa el resto de la clase y toda la Clase 1.

**Lo que hay que mirar, en este orden:**
1. La **forma** — cómo el sesgo del panel 1 se va enderezando hacia el 3.
2. El **ancho** — cómo se va angostando.
3. El **centro** — que **no se mueve**. Este es el punto sutil (ver 4.7).

## 4.3 Para qué sirve en la vida real

Es la justificación de por qué una encuesta a 1000 personas puede decir algo sobre un país de
millones. No porque 1000 sea "suficiente" en abstracto, sino porque **la media de una muestra se
comporta de forma predecible aunque los datos individuales no**.

## 4.4 Para qué lo usamos en esta clase

Es el Teorema del Límite Central **antes de nombrarlo**. Todo el diseño de la clase depende de que
este gráfico funcione: si el alumno ve la campana aparecer, el resto de la clase tiene dónde
apoyarse. Si no la ve, nada de lo que sigue se sostiene.

## 4.5 Los hallazgos (verificados)

**La asimetría, achicándose:**

| Qué se grafica | Asimetría (skew) |
|---|---|
| Los 1183 sueldos individuales | **4,19** |
| Medias de muestras de 5 | **1,81** |
| Medias de muestras de 20 | **0,85** |

💡 **Y acá el hallazgo nuevo, que el notebook no muestra: la ley σ/√n, medida.** Esta clase decidió
**no** enseñar esa fórmula — pero se cumple igual, y comprobarlo es una buena respuesta si algún
alumno avanzado pregunta si hay matemática detrás de lo que está viendo:

| Tamaño de muestra | Desvío observado en el gráfico | σ/√n (la fórmula clásica) | Error |
|---|---|---|---|
| n = 1 (los datos crudos) | $81.896 | $81.896 | exacto por definición |
| n = 5 | **$37.771** | $36.625 | 3,1% |
| n = 20 | **$19.049** | $18.312 | 4,0% |

La simulación reproduce la fórmula con ~3-4% de error, sin haberla usado nunca. **Eso es lo que
significa que el bootstrap y la permutación "no necesitan la teoría": no la contradicen, llegan al
mismo lugar por otro camino.**

## 4.6 Curiosidades e historia

El teorema lo enunció **Pierre-Simon Laplace** en 1810, pero el nombre "central" no viene de que sea
importante: viene de **George Pólya**, que en 1920 lo llamó *"zentraler Grenzwertsatz"* — teorema
central del límite — porque ocupaba el lugar *central* en la teoría. La traducción al español movió
la palabra y quedó "teorema del límite central", que suena a que el límite es central. Es un error de
traducción de hace un siglo que ya nadie va a corregir.

## 4.7 Errores comunes — ⚠️ leé esto antes de dictar

**El error más importante de toda la guía, y lo comete casi todo el mundo:**

Mirando el panel 3, es tentador concluir *"a medida que la muestra crece, la media se acerca más a la
media real"*. **Es falso.** La media de la distribución muestral es μ **para cualquier n**, incluso
n=1. Lo único que cambia con n es la **forma** (se vuelve más normal) y el **ancho** (se angosta).

Watkins, Bargagliotti & Franklin (2014) documentaron que la simulación **causa** este error: al
promediar 1000 repeticiones, el promedio se acerca a μ por el número de repeticiones, no por el
tamaño de cada muestra — y el alumno atribuye el efecto a la variable equivocada. En su estudio,
**nueve profesores de secundaria con experiencia enseñando estadística cayeron**. Uno escribió:
*"As expected when the sample size increases the mean approaches the true mean."*

El enunciado correcto, para leer en voz alta:

> **No importa cuál sea el tamaño de muestra n, la distribución muestral de la media tiene media μ y
> desviación σ/√n. Lo único que cambia con n es la forma — se vuelve más parecida a una campana — y
> qué tan angosta es. El centro no se mueve.**

Otros errores:
- **Confundir "distribución de la muestra" con "distribución muestral".** La primera son los datos de
  una muestra; la segunda son las medias de muchas. Son objetos distintos y el nombre no ayuda.
- **Creer que el TCL "vuelve normales a los datos".** Los datos del panel 1 siguen igual de torcidos
  después del TCL que antes. Lo que se vuelve normal es el panel 3.

## 4.8 El código

```python
np.random.seed(7)
medias_de_5  = [df["converted_comp_yearly"].sample(5).mean()  for _ in range(1000)]
medias_de_20 = [df["converted_comp_yearly"].sample(20).mean() for _ in range(1000)]
fig, axes = plt.subplots(3, 1, figsize=(7, 9))
sns.histplot(df["converted_comp_yearly"], bins=40, ax=axes[0], color="#B45309")
sns.histplot(medias_de_5,  bins=40, ax=axes[1], color="#2E5496")
sns.histplot(medias_de_20, bins=40, ax=axes[2], color="#2E7D32")
```

---

# 5. El histograma bootstrap con el intervalo de confianza

**Bloque F · `sns.histplot(medias_boot)` + `plt.axvline(...)`**

## 5.1 ¿Qué es?

Se simula tener **solo 20 personas** (no las 1183). De esas 20 se remuestrea con reemplazo 1000
veces, se calcula la media de cada remuestreo, y se grafican esas 1000 medias. Las líneas rojas
cortan el 5% de cada punta; lo que queda en el medio es el **intervalo de confianza del 90%**.

## 5.2 Anatomía

- **`plt.axvline(x, ...)`:** dibuja una línea vertical en la posición `x`. Se usan tres: las dos
  rojas punteadas (los límites) y la negra sólida (la media real de los 1183, que en la vida real no
  conoceríamos — está solo para verificar).
- **`.quantile([0.05, 0.95])`:** literalmente "dame el valor por debajo del cual está el 5% de los
  datos, y el que deja el 95%". Cortar percentiles **es** construir el intervalo. No hay fórmula.

## 5.3 Para qué sirve en la vida real

Todo número estimado a partir de una muestra debería reportarse con un rango. "La conversión subió
2,3%" es una afirmación incompleta; "subió entre 0,4% y 4,1% con 90% de confianza" es una afirmación
que se puede usar para decidir.

## 5.4 Para qué lo usamos en esta clase

Para convertir el intervalo de confianza de "una fórmula con t de tabla" en **"cortar las puntas del
histograma"**. Es la operación más simple de toda la clase y produce el objeto que más se usa en el
trabajo real.

## 5.5 Los hallazgos (verificados)

| Dato | Valor |
|---|---|
| Media de la muestra de 20 | $89.282 |
| IC 90% | **[$68.813 · $113.901]** — ancho $45.088 |
| IC 99% | [$59.703 · $126.404] — ancho **$66.701** |
| Media real de los 1183 | $90.684 — **cae dentro** de ambos |

⚠️ **El hallazgo más importante de esta guía, y es una advertencia.** Repetí el experimento completo
200 veces (200 muestras distintas de 20 personas, cada una con su IC del 90%) y conté cuántas
capturaron la media real:

> **168 de 200 = 84%.** No 90%.

**El intervalo del "90%" cubre menos de lo que promete cuando la muestra es chica.** No es un error
del código: es una limitación conocida del método. Hesterberg (2015), que es especialista en
bootstrap, lo advierte explícitamente — el intervalo percentil bootstrap **es menos preciso que un
intervalo t clásico en muestras chicas**, justo al revés de lo que la intuición sugiere.

**Qué hacer con esto en clase:** no lo desarrolles si el tiempo aprieta, pero **no vendas el
bootstrap como infalible**. Si un alumno pregunta "¿entonces siempre conviene el bootstrap?", la
respuesta honesta es: con muestras grandes sí, con muestras chicas hay que tener cuidado, y este es
un ejemplo medido de por qué.

## 5.6 Curiosidades e historia

El bootstrap lo introdujo **Bradley Efron** en 1979. El nombre viene de la expresión inglesa *"to
pull yourself up by your own bootstraps"* — levantarte tirando de los cordones de tus propias botas,
o sea hacer algo aparentemente imposible sin ayuda externa. Es exactamente lo que hace el método:
estimar la incertidumbre de una muestra usando únicamente esa muestra.

## 5.7 Errores comunes

- **Decir "hay 90% de probabilidad de que la media real esté en este intervalo".** Técnicamente
  incorrecto: la media real es un número fijo, está o no está. Lo que tiene 90% es **el
  procedimiento**: si repitieras todo muchas veces, el 90% de los intervalos capturarían el valor.
  (Y como acabamos de medir, a veces ni eso.)
- **Creer que un intervalo más ancho es "peor".** Un intervalo ancho es una afirmación honesta sobre
  poca información.
- **Remuestrear de la población en vez de la muestra.** El bootstrap remuestrea **de los 20**, no de
  los 1183. Si tuvieras los 1183 no necesitarías bootstrap.

## 5.8 El código

```python
muestra_20 = df["converted_comp_yearly"].sample(20, random_state=3)
medias_boot = pd.Series([muestra_20.sample(20, replace=True).mean() for _ in range(1000)])
ic_90 = medias_boot.quantile([0.05, 0.95])
plt.axvline(ic_90.iloc[0], color="#B91C1C", linestyle="--", lw=2)
```

---

# 6. ⭐ El histograma de permutación con la línea observada

**Bloque G · el clímax de la clase**

## 6.1 ¿Qué es?

Los 1183 sueldos se juntan en una sola bolsa, ignorando quién usa IA y quién no. Se reparten al azar
en dos grupos del mismo tamaño que los reales (789 y 394), se mide la diferencia de medias, y se
repite **2000 veces**. El histograma muestra esas 2000 diferencias producidas **solo por azar**. La
línea negra marca la diferencia que observamos de verdad.

## 6.2 Anatomía

- **El histograma completo** = el mundo donde "usar IA" no tiene ningún efecto.
- **La línea negra sólida** = lo que efectivamente medimos ($4.215).
- **La línea negra punteada** = el espejo del otro lado, para que se vea que el test mira las dos
  colas (una diferencia igual de grande pero al revés también contaría).
- **El p-value** = qué fracción del histograma queda tan lejos del centro, o más, que la línea.

## 6.3 Para qué sirve en la vida real

Es la pregunta de todo test A/B, todo estudio clínico y toda evaluación de política pública: la
diferencia que veo entre estos dos grupos, ¿es real o es el ruido de haber elegido a estas personas
y no a otras?

## 6.4 Para qué lo usamos en esta clase

**Este gráfico ES la definición del p-value.** Si el alumno se lleva la imagen, no necesita
memorizar la definición: puede reconstruirla mirando. Es la apuesta central del diseño de la clase.

## 6.5 Los hallazgos (verificados)

| Dato | Valor |
|---|---|
| Centro de las 2000 diferencias al azar | **$61** — prácticamente cero, como debe ser |
| Desviación de esas diferencias | $4.952 |
| Rango completo | de −$22.117 a +$14.350 |
| Asimetría del histograma | −0,112 (**casi perfectamente simétrico**) |
| Diferencia observada | $4.215 |

💡 **El número que mejor explica el resultado, y que el notebook no calcula:** la diferencia
observada de $4.215 está a **0,85 desviaciones del centro**. Menos de una.

Eso es lo que significa "no es raro", dicho sin p-values: *el azar solo, rutinariamente, produce
diferencias más grandes que la que encontramos.* Si algún alumno no termina de entender el p-value,
este número suele destrabarlo.

**El centro en $61 y no en $0** es otro detalle que vale la pena si alguien lo nota: es ruido de
haber hecho 2000 repeticiones y no infinitas. Con 20.000 se acercaría más a cero. Es la misma idea
del error de simulación que aparece en el gráfico 4.

## 6.6 Curiosidades e historia

El test de permutación lo inventó **Ronald Fisher** en 1935, en *The Design of Experiments*, con el
famoso ejemplo de **la dama que decía distinguir si el té se sirvió antes o después de la leche**.
Fisher calculó a mano todas las formas posibles de ordenar 8 tazas. Es el mismo razonamiento que
nuestras 2000 permutaciones, solo que él no tenía computadora y nosotros sí.

Fisher lo consideraba el método **fundamentalmente correcto**, y a los tests como el t-test una
aproximación práctica para cuando no se podía calcular a mano. La historia le dio vuelta el orden por
razones puramente computacionales — y esta clase se lo devuelve.

## 6.7 Errores comunes

- **Leer el histograma como "los sueldos".** No lo son. Son diferencias entre grupos inventados.
- **Creer que el histograma centrado en cero prueba que no hay efecto.** El histograma está centrado
  en cero **por construcción**: lo armamos asumiendo que no hay efecto. Es la hipótesis, no el
  hallazgo. (Case & Jacobbe, 2018, documentan este error como típico de la enseñanza por simulación.)
- **Concluir "no hay diferencia".** La conclusión correcta es "no hay **evidencia suficiente** de
  diferencia con estos datos". La diferencia podría existir; no la pudimos demostrar.
- **Pensar que más permutaciones cambiarían la conclusión.** Con 20.000 en vez de 2000 el p-value se
  estabilizaría en el tercer decimal, no se movería a otro orden de magnitud.

## 6.8 El código

```python
def perm_fun(x, nA, nB):
    n = nA + nB
    idx_A = set(np.random.choice(n, nA, replace=False))
    idx_B = set(range(n)) - idx_A
    return x.iloc[list(idx_A)].mean() - x.iloc[list(idx_B)].mean()

diferencias_al_azar = np.array([perm_fun(combinado, len(usando), len(planea))
                                for _ in range(2000)])
plt.axvline(diferencia_observada, color="black", lw=2.5)
p_valor = np.mean(np.abs(diferencias_al_azar) >= abs(diferencia_observada))
```

---

## Anexo — tabla maestra de cifras de los gráficos

Todas verificadas ejecutando contra `data_dev.csv` con las semillas del notebook. **Si algún
documento de esta clase dice otra cosa, el documento está mal.**

| Gráfico | Cifra | Valor |
|---|---|---|
| 1 | Rango observado / moda / concentración 8-12 | 3–17 / 10 (903 veces) / 73,5% |
| 2 | Regla 68-95-99.7 sobre la simulación | 68,61% / 95,65% / 99,74% |
| 3 | Rango / mediana / media / skew | $3–$1.200.000 / $72.714 / $90.684 / 4,19 |
| 3 | Primera barra / barras vacías / >$300k | 203 (17,2%) / **20 de 40** / 21 (1,78%) |
| 4 | Skew: crudos / medias de 5 / medias de 20 | 4,19 / 1,81 / 0,85 |
| 4 | Desvío observado vs σ/√n (n=5) | $37.771 vs $36.625 |
| 4 | Desvío observado vs σ/√n (n=20) | $19.049 vs $18.312 |
| 5 | Media de la muestra de 20 | $89.282 |
| 5 | IC 90% / IC 99% | [$68.813 · $113.901] / [$59.703 · $126.404] |
| 5 | **Cobertura real del IC 90%** | **168/200 = 84%** |
| 6 | Centro / desvío / skew del histograma nulo | $61 / $4.952 / −0,112 |
| 6 | Diferencia observada / en desvíos | $4.215 / **0,85σ** |
| 6 | p-value (permutación / t-test) | 0,398 / 0,353 |

---

## Fuentes de las advertencias

- **Watkins, A. E., Bargagliotti, A. & Franklin, C. (2014).** "Simulation of the Sampling
  Distribution of the Mean Can Mislead." *Journal of Statistics Education*, 22(3).
  https://jse.amstat.org/v22n3/watkins.pdf — el hallazgo del gráfico 4.
- **Hesterberg, T. C. (2015).** "What Teachers Should Know About the Bootstrap." *The American
  Statistician*, 69(4), 371–386. — la advertencia sobre la cobertura del gráfico 5.
- **Case, C. & Jacobbe, T. (2018).** "A framework to characterize student difficulties in learning
  inference from a simulation-based approach." *SERJ*, 17(2), 9–29. — los errores del gráfico 6.
- **Bruce, P., Bruce, A. & Gedeck, P. (2020).** *Practical Statistics for Data Scientists*, 2ª ed.
  O'Reilly. Caps. 2 y 3.
