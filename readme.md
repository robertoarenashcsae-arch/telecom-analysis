# ConnectaTel — Análisis de uso de telecomunicaciones

## Objetivo

Analizar el comportamiento de llamadas y mensajes de clientes de ConnectaTel, empresa con operaciones en México y Colombia, para identificar patrones de consumo, valores atípicos y segmentos útiles para orientar decisiones comerciales. El análisis se centra en los registros disponibles hasta 2024.

## Datos

| Archivo | Contenido |
|---|---|
| `plans.csv` | Planes, precios y beneficios (2 registros). |
| `users_latam.csv` | Información y contratación de clientes (4,000 registros). |
| `usage.csv` | Eventos de llamadas y mensajes (40,000 registros). |

Los archivos se relacionan mediante los identificadores de usuario y plan, según corresponda. Los CSV no están incluidos en este README; deben estar disponibles para ejecutar el notebook.

## Herramientas

Python, Jupyter Notebook, pandas, NumPy, Matplotlib y Seaborn.

## Metodología

1. Carga y exploración de dimensiones, tipos y valores ausentes.
2. Revisión de sentinels y fechas inconsistentes; sustitución de `age = -999` por la mediana calculada sin ese sentinel, `city = '?'` por nulo y fechas de registro posteriores a 2024 por `NaT`.
3. Verificación de que los nulos de `duration` y `length` se relacionan principalmente con eventos donde esos campos no aplican.
4. Agregación de mensajes, llamadas y duración por `user_id`, seguida de unión con los clientes mediante `left merge`.
5. Estadística descriptiva, histogramas por plan, boxplots y detección de outliers con IQR (1.5 × rango intercuartílico).
6. Creación de segmentos por edad (`Joven`, `Adulto`, `Adulto Mayor`) y volumen de llamadas y mensajes (`Bajo uso`, `Uso medio`, `Alto uso`).

## Hallazgos principales

- En `users_latam.csv`, `city` tiene 469 ausentes (11.73 %) y `churn_date`, 3,534 (88.35 %). Se identificaron 40 fechas de registro en 2026, fuera del corte del proyecto.
- En `usage.csv`, 50 fechas están ausentes (0.125 %). La ausencia de `duration` ocurre fundamentalmente en mensajes y la de `length`, en llamadas; se conservan como ausencias estructurales, no se imputan arbitrariamente.
- El 64.875 % de los clientes tiene plan `Basico` y el 35.125 % plan `Premium`.
- En los 3,999 perfiles con métricas de uso, los promedios son 5.52 mensajes, 4.48 llamadas y 23.32 minutos acumulados.
- El método IQR detecta 46 observaciones atípicas en mensajes (máximo 17), 30 en llamadas (máximo 15) y 109 en minutos (máximo 155.69). Se recomienda conservarlas provisionalmente y validar los casos extremos.

Estos resultados describen patrones y no demuestran fraude ni rentabilidad por segmento. Las comparaciones de consumo por plan requieren considerar también el tamaño de cada grupo.

## Cómo reproducir

1. Instala Python y las dependencias:

   ```bash
   pip install notebook pandas numpy matplotlib seaborn
   ```

2. Descarga `plans.csv`, `users_latam.csv` y `usage.csv` desde los enlaces suministrados en la consigna del proyecto.
3. Ubica los tres archivos en `/datasets/` en el entorno de ejecución, o ajusta las rutas de `pd.read_csv()` al directorio local donde los guardaste.
4. Abre `S7 Version-Estudiante-Project-ConnectaTel.ipynb` en Jupyter Notebook y ejecuta las celdas en orden, desde el inicio.

## Alcance y limitaciones

El notebook utiliza datos observacionales de uso, sin prueba de causalidad. Las métricas de actividad agregada corresponden a los registros disponibles y no necesariamente a un año completo de exposición comparable para cada usuario. No se calcularon ingresos netos ni rentabilidad por cliente. Algunas celdas de la plantilla de exploración de valores inválidos y el bloque del análisis ejecutivo conservan marcadores pendientes: conviene completarlos antes de presentar una versión final.

## Recomendaciones

- Evaluar ofertas diferenciadas para perfiles de consumo bajo, medio y alto.
- Investigar los patrones de consumo extremo antes de modificar los datos o proponer cambios de plan.
- Contrastar el consumo con los beneficios contratados y, cuando estén disponibles, los ingresos y el abandono para priorizar acciones de retención.
