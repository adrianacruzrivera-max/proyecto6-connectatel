## Etapas del análisis
1. Carga y exploración de los datos.
2. Identificación de problemas de calidad: nulos, sentinels (`age = -999`, `city = "?"`, `duration = 120`, `length = 1490`) y fechas imposibles.
3. Limpieza básica:
   - 3.1 Corregir sentinels y fechas imposibles (edad → mediana, ciudad → nulo, fechas futuras → `NaT`).
   - 3.2 Verificación de nulos MAR en `duration` y `length` (según el tipo de evento).
4. Estadísticas de uso por usuario (mensajes, llamadas y minutos) y distribución de planes.
5. Visualización de distribuciones (histogramas) y outliers con boxplots e IQR.
   - 5.3 (Complemento) Análisis de sensibilidad: efecto de tratar como nulos los sentinels de `duration` (120) y `length` (1,490), sobre una copia de los datos.
6. Segmentación por uso, por edad y, de forma opcional, por país (derivado de `city`).
7. Insights ejecutivos y recomendaciones.
8. Cierre y repositorio.

## Cómo ejecutar el notebook
1. Abre `S7_Version-Estudiante-Project-ConnectaTel.ipynb` en Google Colab (o Jupyter).
2. Sube `plans.csv`, `users_latam.csv` y `usage.csv` a la carpeta `/datasets/`.
3. Ejecuta todo en orden: en Jupyter, **Kernel → Restart Kernel and Run All Cells**; en Colab, **Runtime → Run all**.

El orden de las celdas importa: la revisión de fechas (2.3) debe correr antes de la limpieza del paso 3.

Librerías: pandas, numpy, seaborn, matplotlib.

## Guía de reproducción
El análisis es determinista: los mismos archivos de entrada, ejecutados en el orden indicado, producen los mismos resultados.

## Hallazgos principales
- **Problemas de datos:** edad = -999 (55 filas, 1.4%), ciudad desconocida (565 filas, 14.1%), 40 fechas de registro en 2026 y nulos estructurales en duration y length; además, 30 registros con duration = 120 y 30 con length = 1490 (posibles sentinels). Se mantuvieron en el análisis principal y su efecto se midió aparte (5.3): solo cambian 14 usuarios y no alteran las conclusiones.
- **Uso homogéneo:** la edad, el plan y el país no explican diferencias de consumo (Premium se comporta casi igual que Básico).
- **Segmentos por uso:** Uso medio 73.6%, Bajo uso 19.5% y Alto uso 7.0%.
- **Valor:** Premium es ~35% de la base pero ~53% del ingreso mensual recurrente. Alto uso tiene mayor tasa de baja (14.0% vs 11.5%).
- **País:** Colombia 46.2% y México 39.7% de los clientes; sin diferencias de uso entre ambos.
- **Outliers:** solo hacia arriba y se mantienen por ser plausibles (96 usuarios en minutos, 46 en mensajes y 30 en llamadas; no hay outliers hacia abajo). Si las llamadas de 120 min se tratan como nulas, los outliers en minutos bajan a 84 (ver 5.3).
- **Limitaciones:** no hay consumo de datos (GB) y las diferencias de baja entre segmentos son pequeñas, por lo que se presentan como indicio.
