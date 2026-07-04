# oaxaca-spatial-autocorrelation-geoda


# Autocorrelación Espacial e Inseguridad Alimentaria en Oaxaca

Análisis de estadística espacial inferencial sobre inseguridad alimentaria y feminización de hogares a nivel municipal en Oaxaca, México (2020), usando **GeoDa**.

> Proyecto académico realizado en el curso *Estadística Espacial*, Maestría en Población y Desarrollo, FLACSO-México (2025). Las capturas, resultados e interpretación son trabajo propio del autor.

## Resumen

¿Se distribuye la inseguridad alimentaria de forma aleatoria en el territorio, o existen agrupamientos geográficos? ¿Qué relación espacial guarda con la feminización de los hogares? Este proyecto responde estas preguntas combinando:

- **Autocorrelación espacial univariada** (I de Moran global) para inseguridad alimentaria y proporción de hogares encabezados por mujeres.
- **Autocorrelación espacial bivariada** e **indicadores locales (LISA)** para identificar clústeres geográficos específicos donde ambas variables interactúan.
- **Modelos de regresión espacial** (rezago espacial y error espacial) comparados contra un modelo OLS clásico, para determinar si la dependencia espacial debe incorporarse explícitamente al modelo explicativo.

## Fuente de datos

Base de datos "Oaxaca Development" del [GeoDa Center](https://geodacenter.github.io/data-and-lab/Oaxaca-Development/), con indicadores municipales de pobreza, inseguridad alimentaria, composición de hogares y características demográficas.

## Metodología y resultados

### 1. I de Moran global — Inseguridad alimentaria y feminización de hogares

Con una matriz de contigüidad tipo torre, ambas variables muestran autocorrelación espacial positiva y estadísticamente significativa (pseudo p-value = 0.01, 99 permutaciones):

| Variable | Moran's I | Z-value |
|---|---|---|
| Inseguridad alimentaria 2020 | 0.432 | 15.79 |
| % Hogares encabezados por mujeres 2020 | 0.247 | 8.64 |

La inseguridad alimentaria muestra un nivel de agrupamiento espacial considerablemente más alto que la feminización de hogares, lo que indica zonas homogéneas de alta/baja inseguridad geográficamente próximas.

![I de Moran - Inseguridad alimentaria](images/01_moran_scatter_pfood20.png)
![Distribución de permutaciones - Inseguridad alimentaria](images/02_moran_hist_pfood20.png)
![I de Moran - Hogares encabezados por mujeres](images/03_moran_scatter_hhf20.png)
![Distribución de permutaciones - Hogares encabezados por mujeres](images/04_moran_hist_hhf20.png)

### 2. I de Moran bivariado y LISA

El índice de Moran bivariado (**-0.075**, p = 0.01) revela una asociación espacial inversa significativa: los municipios con alta proporción de hogares encabezados por mujeres tienden a estar rodeados de municipios con **baja** inseguridad alimentaria, y viceversa.

El análisis LISA bivariado permite ir más allá del promedio global y localizar en el mapa **dónde** ocurre esa asociación:

![I de Moran bivariado](images/05_moran_bivariado_scatter.png)
![Distribución de permutaciones - bivariado](images/06_moran_bivariado_hist.png)
![Mapa de clústeres BiLISA](images/07_bilisa_cluster_map.png)
![Mapa de significancia BiLISA](images/08_bilisa_significance_map.png)

- **Clústeres Bajo-Alto** (frecuentes en el municipio de Oaxaca): baja feminización de hogares en un entorno de fuerte inseguridad alimentaria.
- **Clústeres Alto-Bajo** (los más frecuentes): fuerte presencia de hogares feminizados en entornos con menor inseguridad alimentaria.
- **Clústeres Alto-Alto** (más raros): bolsas locales donde ambas condiciones coinciden.

La heterogeneidad de estos clústeres muestra que la relación global (-0.075) esconde patrones locales muy distintos entre sí — el valor agregado de LISA frente al Moran global.

### 3. Modelo OLS vs. modelos espaciales (rezago y error)

Se estimó un modelo de regresión lineal (OLS) de la inseguridad alimentaria en función de rezago educativo, feminización de hogares, población no nacida en el municipio, población indígena y población de 65 años y más:

![Salida del modelo OLS](images/09_ols_regression_output.png)

El modelo OLS es significativo globalmente (R² = 0.22), pero la autocorrelación espacial detectada en los pasos anteriores sugiere que un modelo no espacial no captura toda la estructura del fenómeno. Se compararon entonces dos alternativas espaciales:

![Modelo de rezago espacial (SAR)](images/10_spatial_lag_model_output.png)
![Modelo de error espacial (SEM)](images/11_spatial_error_model_output.png)

| Modelo | AIC | Parámetro espacial | Significancia |
|---|---|---|---|
| OLS | 4,542.46 | — | — |
| Rezago espacial (SAR) | 4,406.79 | ρ = 0.572 | p < 0.001 |
| Error espacial (SEM) | **4,394.13** | λ = 0.641 | p < 0.001 |

El **modelo de error espacial (SEM)** presenta el menor AIC y un coeficiente λ alto y significativo, y elimina la autocorrelación residual observada en el OLS. Esto indica que la inseguridad alimentaria en Oaxaca está determinada no solo por las características observables de cada municipio, sino también por **factores no observados y estructurados espacialmente** que afectan simultáneamente a municipios vecinos — evidencia a favor de intervenciones de política pública con enfoque territorial y regional, en lugar de exclusivamente municipal.

## Conclusiones

- La inseguridad alimentaria y la feminización de hogares en Oaxaca **no se distribuyen al azar**: ambas presentan agrupamientos geográficos estadísticamente significativos.
- Su relación espacial es heterogénea: el análisis LISA revela que la asociación global (negativa y moderada) oculta configuraciones locales muy distintas entre municipios.
- La comparación de modelos confirma la necesidad de **incorporar la dependencia espacial** al analizar determinantes de la inseguridad alimentaria; ignorarla (como en un OLS estándar) puede llevar a estimaciones sesgadas o incompletas de los factores relevantes para el diseño de política.

## Herramientas

- **GeoDa** — cálculo de I de Moran (global y bivariado), LISA, regresión OLS y modelos espaciales (SAR/SEM).
- Matriz de pesos espaciales: contigüidad tipo torre (*queen contiguity*).

## Autor

Wisnel François — [LinkedIn](http://www.linkedin.com/in/Wisnel-Francois) | [GitHub](https://github.com/wisnel600)
