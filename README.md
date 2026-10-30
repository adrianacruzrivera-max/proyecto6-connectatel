# Análisis ConnectaTel

## Objetivo
Evaluar el comportamiento de los clientes de ConnectaTel (Latinoamérica, datos hasta 2024) para construir un perfil estadístico, detectar comportamientos atípicos y segmentar a los clientes, con el fin de sugerir estrategias de retención y mejoras en los planes.

## Datasets
- `plans.csv`: planes actuales (precio, minutos, mensajes y GB incluidos, costo por extra).
- `users_latam.csv`: clientes (edad, ciudad, fecha de registro, plan, churn).
- `usage.csv`: uso real (llamadas y mensajes) registrado en 2024 (enero a junio). No incluye consumo de datos (GB).

## Etapas del análisis
1. Carga y exploración de los datos.
2. Identificación de problemas de calidad: nulos, sentinels (`age = -999`, `city = "?"`, `duration = 120`, `length = 1490`) y fechas imposibles.
3. Limpieza básica:
   - 3.1 Corregir sentinels y fechas imposibles (edad → mediana, ciudad → nulo, fechas futuras → `NaT`).
   - 3.2 Verificación de nulos MAR en `duration` y `length` (según el tipo de evento).
   - 3.3 (Complemento) Marcar como nulos los sentinels de `duration` (120) y `length` (1,490), sin imputar.
4. Estadísticas de uso por usuario (mensajes, llamadas y minutos) y distribución de planes.
5. Visualización de distribuciones (histogramas) y outliers con boxplots e IQR.
6. Segmentación por uso, por edad y, de forma opcional, por país (derivado de `city`).
7. Insights ejecutivos y recomendaciones.
8. Cierre y repositorio.

## Cómo ejecutar el notebook
1. Abre `S7_Version-Estudiante-Project-ConnectaTel.ipynb` en Google Colab (o Jupyter).
2. Sube `plans.csv`, `users_latam.csv` y `usage.csv` a la carpeta `/datasets/`.
3. Ejecuta todo en orden: en Jupyter, **Kernel → Restart Kernel and Run All Cells**; en Colab, **Runtime → Run all**.

El orden de las celdas importa: la revisión de fechas (2.3) debe correr antes de la limpieza del paso 3, y el paso 3.3 debe correr antes de agrupar el uso por usuario (4.1).

Librerías: pandas, numpy, seaborn, matplotlib.

## Guía de reproducción
El análisis es determinista: los mismos archivos de entrada, ejecutados en el orden indicado, producen los mismos resultados.

## Hallazgos principales
- **Problemas de datos:** `age = -999` (55 filas, 1.4%), `city` desconocida (565 filas, 14.1%), 40 fechas de registro en 2026 y nulos estructurales en `duration`/`length`; además, 30 registros con `duration = 120` y 30 con `length = 1490` (sentinels repetidos, marcados como nulos).
- **Uso homogéneo:** la edad, el plan y el país no explican diferencias de consumo (Premium se comporta casi igual que Básico).
- **Segmentos por uso:** Uso medio 73.6%, Bajo uso 19.5% y Alto uso 7.0%.
- **Valor:** Premium es ~35% de la base pero ~53% del ingreso mensual recurrente. Alto uso tiene mayor tasa de baja (14.0% vs 11.5%).
- **País:** Colombia 46.2% y México 39.7% de los clientes; sin diferencias de uso entre ambos.
- **Outliers:** solo hacia arriba y se mantienen por ser plausibles (84 usuarios en minutos, 46 en mensajes y 30 en llamadas; no hay outliers hacia abajo).
- **Limitaciones:** no hay consumo de datos (GB) y las diferencias de baja entre segmentos son pequeñas, por lo que se presentan como indicio.

## Repositorio
https://github.com/adrianacruzrivera-max/proyecto6-connectatel
