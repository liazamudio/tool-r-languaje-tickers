# Cómo trabajar con este proyecto de R en Visual Studio Code

Estas instrucciones documentan cómo quedó configurado el entorno en este equipo y cómo replicarlo en otro.

## 1. Estado actual de este equipo

Ya está todo instalado y probado:

- **R 4.6.1** instalado en `C:\Program Files\R\R-4.6.1` (vía `winget install RProject.R`).
- El directorio `bin\x64` de R se agregó al **PATH del usuario**, así que `R` y `Rscript` funcionan desde cualquier terminal.
- Extensiones de VS Code instaladas:
  - `REditorSupport.r` — soporte principal de R (terminal, ejecución de código, variables, plots).
  - `REditorSupport.r-syntax` — resaltado de sintaxis adicional.
- Paquetes de R instalados en la librería de usuario (`C:\Users\<usuario>\AppData\Local\R\win-library\4.6`):
  - `languageserver` — autocompletado, diagnósticos y "ir a definición" dentro de VS Code.
  - `httpgd` — visor de gráficos embebido en VS Code (en vez de la ventana externa de R).
  - Paquetes que usa el proyecto: `shiny`, `shinydashboard`, `shinythemes`, `ggplot2`, `dplyr`, `corrplot`, `reshape2`, `quantmod`.
- Se creó `.vscode/settings.json` en la raíz del proyecto apuntando al ejecutable de R para que la extensión lo detecte automáticamente.

## 2. Cómo replicar el entorno en otro equipo

1. Instalar R (recomendado vía winget):
   ```powershell
   winget install --id RProject.R -e
   ```
2. Abrir una terminal **nueva** (para que tome el R recién instalado) y confirmar:
   ```powershell
   R --version
   ```
3. Instalar en R los paquetes necesarios para el proyecto y para la integración con VS Code:
   ```r
   install.packages(c(
     "languageserver", "httpgd",
     "shiny", "shinydashboard", "shinythemes",
     "ggplot2", "dplyr", "corrplot", "reshape2", "quantmod"
   ))
   ```
4. Instalar en VS Code las extensiones `REditorSupport.r` y `REditorSupport.r-syntax` (Marketplace de VS Code).
5. Abrir la carpeta del proyecto **como carpeta raíz** en VS Code (`File > Open Folder`), no un archivo suelto — los scripts leen el dataset con una ruta relativa (`dataset/fibras2014-2024.csv`) que asume que la raíz del workspace es la raíz del repo.

## 3. Cómo trabajar día a día

- **Terminal de R**: abre cualquier archivo `.R` y ejecuta `Ctrl+Enter` sobre una línea o selección para enviarla a la terminal de R integrada (la extensión la abre automáticamente la primera vez).
- **Panel de variables**: la extensión muestra un explorador de variables del entorno de R (ícono de R en la barra lateral).
- **Gráficos**: con `httpgd` instalado, los `plot()` / `ggplot()` se muestran en un panel embebido de VS Code en vez de una ventana aparte.
- **Autocompletado y diagnósticos**: los provee `languageserver` automáticamente al abrir un archivo `.R`.

## 4. Cómo correr cada script

### `app.R` — Dashboard Shiny de FIBRAS

Con el archivo `app.R` abierto, usa el botón **Run App** que aparece arriba del editor (lo agrega la extensión de R al detectar una app Shiny), o desde la terminal de R:

```r
shiny::runApp("app.R")
```

Esto abre el dashboard en el navegador/panel embebido con las pestañas de precios, comparativos y resumen de las FIBRAS.

### `proyecto-prog-r_v0.2.R` — Script de análisis exploratorio

Es un script paso a paso (EDA, correlaciones, regresión, series de tiempo). Ejecútalo por bloques con `Ctrl+Enter` en vez de correrlo completo de un jalón: contiene gráficas y `View()` intermedios pensados para revisarse uno a uno, y una sección al final (`get.best.arima`, línea ~701) que tiene un error de sintaxis pendiente de corregir por quien lo escribió originalmente (una coma de más antes de `maxord`).

## 5. Cambio aplicado a los scripts

Ambos scripts (`app.R` y `proyecto-prog-r_v0.2.R`) tenían un `setwd()` hardcodeado a una ruta local de OneDrive que no existe en otros equipos. Se reemplazó por una ruta relativa (`dataset/fibras2014-2024.csv`), por lo que **es indispensable abrir el proyecto con la carpeta raíz del repo como workspace** en VS Code para que la terminal de R arranque en esa ubicación y la ruta relativa funcione.

## 6. Problemas comunes

- **"no se pudo encontrar el paquete"**: falta instalar ese paquete, revisa la lista en la sección 2.
- **La extensión de R no detecta R**: revisa que `.vscode/settings.json` tenga la ruta correcta en `r.rpath.windows`, o reinstala R y actualiza esa ruta.
- **`read.csv` no encuentra el archivo**: confirma que abriste la **carpeta** del proyecto (no un archivo suelto) y que la terminal de R está ubicada en la raíz del repo (`getwd()` debe mostrar la ruta del proyecto).
