[README.md](https://github.com/user-attachments/files/33361489/README.md)
# ConnectaTel | Análisis de uso de telecomunicaciones

## Objetivo

Analizar llamadas y mensajes de clientes de ConnectaTel en México y Colombia, detectar valores atípicos y segmentar el consumo para orientar decisiones comerciales. Periodo de referencia: datos disponibles hasta 2024.

## Archivos del proyecto

- `ConnectaTel_corregido.ipynb`: notebook con exploración, limpieza, análisis, gráficos y conclusiones.
- `plans.csv`: catálogo de planes y beneficios.
- `users_latam.csv`: datos de clientes, edad, ciudad, plan, registro y cancelación.
- `usage.csv`: eventos de llamadas y mensajes.

Los tres CSV deben descargarse de la consigna del curso; no están incluidos en este repositorio salvo que se incorporen expresamente.

## Cómo ejecutar el notebook

1. Instala Python 3 y Jupyter Notebook.
2. Instala las dependencias:

   ```bash
   python -m pip install notebook pandas numpy matplotlib seaborn
   ```

3. Descarga los archivos de datos desde la consigna: [plans.csv](https://drive.google.com/uc?export=download&id=17Mkcs9rRWwiC_gaqVBYuFieON7s9v7Bn), [users_latam.csv](https://drive.google.com/uc?export=download&id=17wuqxalUsUnw9PXvCN2_UaAz6xeS9B0T) y [usage.csv](https://drive.google.com/uc?export=download&id=11T8MQf-ouxJu9tia4F8aNpY7M_fb9O4h).
4. Guarda los CSV en una carpeta `datasets/` junto al notebook. Cambia la celda de carga a:

   ```python
   import pandas as pd
   plans = pd.read_csv('datasets/plans.csv')
   users = pd.read_csv('datasets/users_latam.csv')
   usage = pd.read_csv('datasets/usage.csv')
   ```

   Si trabajas en el entorno del curso, puedes conservar las rutas originales `/datasets/...`.
5. Abre `ConnectaTel_corregido.ipynb` con Jupyter y ejecuta **Kernel → Restart & Run All**. Revisa los resultados y gráficos antes de publicar.

## Metodología

1. Exploración de dimensiones, tipos, datos ausentes y categorías.
2. Limpieza: `age = -999` por la mediana válida, `city = '?'` por ausente y fechas posteriores a 2024 por `NaT`.
3. Verificación de ausencias estructurales en `duration` (mensajes) y `length` (llamadas).
4. Agregación por `user_id` de mensajes, llamadas y minutos; unión con datos de clientes.
5. Estadísticas descriptivas, histogramas por plan, boxplots y detección de outliers mediante IQR.
6. Segmentación por edad y nivel de uso; interpretación comercial.

## Resultados observados

- 4,000 clientes y 40,000 eventos de uso.
- En clientes: 469 ciudades nulas (11.73 %), 96 ciudades registradas como `?` (2.40 %) y 40 fechas de registro de 2026 fuera del periodo de estudio.
- La fecha de cancelación falta en 3,534 clientes (88.35 %); estos nulos pueden corresponder a clientes activos y no deben eliminarse automáticamente.
- En actividad: 50 fechas nulas (0.125 %); 22,092 mensajes y 17,908 llamadas.
- Distribución de planes: 2,595 Básico (64.88 %) y 1,405 Premium (35.12 %).
- En 3,999 usuarios con métricas de actividad: promedios de 5.52 mensajes, 4.48 llamadas y 23.32 minutos acumulados.
- IQR: 46 valores atípicos en mensajes (máximo 17), 30 en llamadas (máximo 15) y 109 en minutos (máximo 155.69). Se recomienda revisar los extremos, no eliminarlos automáticamente.

## Conclusión y límites

El consumo de llamadas y mensajes presenta colas hacia valores altos, especialmente en los minutos de llamada. Conviene evaluar ofertas según el nivel de uso y contrastarlas con los beneficios de cada plan. Los outliers no demuestran fraude. La información disponible no permite concluir qué segmento es más rentable sin ingresos y costos por cliente. Las cifras mostradas proceden de las salidas disponibles del notebook; la reproducción completa debe verificarse ejecutándolo con los CSV.
