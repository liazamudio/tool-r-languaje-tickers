## 15-Proy-Prog-R

Proyecto del Módulo Programación y estadística con R

Se analizan los precios históricos de un conjunto de FIBRAS (Fideicomisos de Inversión en Bienes Raíces) que cotizan en la Bolsa Mexicana de Valores: rendimientos, riesgo (desviación estándar), medidas de tendencia, correlaciones, regresión lineal y modelado de series de tiempo (ARIMA) sobre el precio de una FIBRA.

## Estructura del repositorio

- [proyecto-prog-r_v0.2.R](proyecto-prog-r_v0.2.R) — script exploratorio principal, pensado para correrse por bloques (no de un jalón): limpieza de datos, análisis de rendimiento/riesgo a 5 y 2 años, medidas de tendencia, correlaciones, regresión lineal simple y múltiple, y análisis de series de tiempo (descomposición, AR, ARIMA) sobre la FIBRA `FNOVA17`.
- [app.R](app.R) — dashboard interactivo con Shiny que expone parte de ese análisis (tabla de precios, comparativos a 5 años, medidas de tendencia y resumen estadístico).
- [dataset/fibras2014-2024.csv](dataset/fibras2014-2024.csv) — precios mensuales de cierre de 15 FIBRAS, mayo 2014 a mayo 2024 (121 registros).
- [INSTRUCCIONES-VSCODE-R.md](INSTRUCCIONES-VSCODE-R.md) — cómo dejar listo el entorno de R en VS Code para trabajar con este proyecto.
- [LICENSE](LICENSE) — GNU GPL v3.

## Dataset

`dataset/fibras2014-2024.csv` tiene una fila por mes (`Fecha`) y una columna por ticker de FIBRA. No todas las FIBRAS cotizaban desde 2014, así que varias columnas tienen valores vacíos en sus primeros meses (antes de su colocación en bolsa): `STORAGE18` y `FNOVA17` tienen 52 valores faltantes, `FCFE18` 46, `FPLUS16` 31, `FIBRAHD15` 14, `FMTY14` 8, `FHIPO14` 7 y `FIHO12` 2. El resto (`DANHOS13`, `FIBRAMQ12`, `FIBRAPL14`, `FINN13`, `FUNO11`, `TERRA13`) están completas en todo el rango. Los análisis a 5 y 2 años del script solo usan las ventanas recientes del dataset, donde la mayoría de estas columnas ya tienen historial completo.

## Cómo ejecutar

Ver [INSTRUCCIONES-VSCODE-R.md](INSTRUCCIONES-VSCODE-R.md) para instalar R y dejar la extensión de VS Code lista. Ambos scripts leen el dataset con una ruta relativa (`dataset/fibras2014-2024.csv`), así que hay que abrir la **carpeta raíz del repositorio** como workspace en VS Code (o correr R con ese directorio como working directory) para que las lecturas de archivo funcionen.

## Estado del proyecto y hallazgos de la revisión

- El script `proyecto-prog-r_v0.2.R` ya corre de principio a fin sin errores (verificado ejecutándolo completo). Antes tenía un error de sintaxis y una variable mal referenciada en la sección de búsqueda de mejor modelo ARIMA que impedían llegar al final del script; además esa búsqueda podía abortar por completo si alguna combinación de órdenes no convergía (frecuente al usar órdenes altos sobre solo 60 observaciones mensuales). Ambos problemas quedaron corregidos.
- En `app.R`, las secciones del dashboard **"Comparativo a 2 años"** y **"Comparativo 5 vs 2 años"** están en el menú pero sin contenido (marcadas como `# Pendiente` en el servidor) — el análisis a 2 años y la comparación entre ventanas sí existen en `proyecto-prog-r_v0.2.R`, pero aún no se trasladaron al dashboard.
- `app.R` incluye además tabs de un dashboard de ejemplo sobre el dataset `mtcars` (histograma, dispersión, e imagen de correlación) que quedaron del boilerplate inicial: no están enlazados en el menú lateral (sus `menuItem` están comentados) y una de ellas referencia una imagen (`cor_mtcars.png`) que no existe en el repositorio. Se puede limpiar ese código si ya no se necesita como referencia.
- `dataTableOutput()` (usado en `app.R`) está deprecado desde Shiny 1.8.1 a favor de `DT::DTOutput()`; sigue funcionando pero conviene migrarlo en algún momento.
- Los `setwd()` con rutas absolutas de un equipo específico se reemplazaron por rutas relativas al dataset (ver historial de commits), para que el proyecto corra en cualquier equipo que abra la carpeta como workspace.
