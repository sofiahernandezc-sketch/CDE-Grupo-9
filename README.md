# Predicción de la resistencia del hormigón para el control de calidad

*Proyecto Integrador · Caso 1: Regresión* · Estadística Aplicada · Departamento de Ingeniería Industrial, USACH

*Grupo 09:* Isidora Barrera · Natalia Rojas · Sofía Hernández · Emilia Vásquez

## Problema

Una planta de hormigón premezclado necesita saber si una dosificación nueva cumplirá la resistencia especificada *antes de fabricarla*, sin esperar el ensayo de 28 días. El sistema predice la resistencia a compresión (MPa) a partir de los 7 componentes de la mezcla (kg/m³) y de la edad a la que se quiere conocer.

## Resultado principal

| | RMSE validación cruzada | RMSE test |
| --- | --- | --- |
| *Sistema final: promedio GAM + Random Forest + Boosting* | 5,46 ± 0,97 MPa | *5,31 MPa* |
| Baseline (media por grupo de edad) | 13,46 ± 1,63 MPa | 12,56 MPa |

Intervalo de predicción al 90%: predicción ± 9,1 MPa (cobertura en test: 92%).

## Contenido del repositorio


├── proyecto_final.R     # script único y reproducible (pasos 1 a 5)
├── Datos.zip            # contiene Datos/Concrete_Data.xls (dataset original de UCI)
├── resultados.zip       # contiene resultados/ (tablas y gráficos generados por el script)
└── README.md


## Datos

Yeh, I.-C. (1998). Concrete Compressive Strength [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5PK67 · Licencia CC BY 4.0.

Si Datos/Concrete_Data.xls no está, se descarga desde https://archive.ics.uci.edu/dataset/165/concrete+compressive+strength (archivo Concrete_Data.xls dentro del zip).

## Cómo reproducir

1. Instalar R (≥ 4.3) y RStudio.
2. Instalar los paquetes una sola vez:
   r
   install.packages(c("readxl", "car", "sandwich", "lmtest", "glmnet", "mgcv",
                      "rpart", "rpart.plot", "ranger", "gbm"))
   
3. Descargar el repositorio completo (botón *Code → Download ZIP*) y descomprimirlo, o clonarlo.
4. *Descomprimir Datos.zip y resultados.zip dentro de la misma carpeta*, de modo que queden las carpetas Datos/ y resultados/ junto a proyecto_final.R. Sin este paso el script se detiene con el mensaje "No se encuentra Datos/Concrete_Data.xls".
5. Abrir proyecto_final.R en RStudio y fijar la carpeta de trabajo: *Session → Set Working Directory → To Source File Location*.
6. Ejecutar todo el script (Ctrl + A y luego Ctrl + Enter). Demora entre 4 y 10 minutos.

Todos los resultados y gráficos se guardan en la carpeta resultados/. Con la semilla fija (2026), la partición, los folds y los resultados se reproducen idénticos al volver a ejecutar el script; al final se imprime RMSE_test 5.31 y "REPRODUCIBILIDAD OK".

## Decisiones metodológicas clave

- *Partición por mezcla:* cada receta aparece a varias edades; todas sus filas van completas a entrenamiento (342 mezclas) o a test (85 mezclas), para que el test simule recetas nunca vistas.
- *Sin fuga de información:* el test se guarda aparte y se evalúa con el sistema ya congelado; el escalado y los hiperparámetros se eligen dentro de cada fold (validación cruzada anidada por mezcla).
- *Modelos comparados:* baseline, OLS, ridge, lasso, elastic-net, GAM (splines), árbol de regresión, Random Forest, boosting y su combinación.

### Sobre el registro resultados/test_abierto.txt

El script anota la fecha cada vez que se ejecuta la evaluación en test. La *primera apertura fue el 22-10-2026* y ese es el resultado reportado en el informe y la presentación. Las fechas siguientes corresponden a re-ejecuciones del mismo sistema congelado, sin cambiar modelos ni hiperparámetros, para verificar la reproducibilidad; todas entregan el mismo RMSE de test (5,31 MPa).

## Uso de herramientas de IA

Se usaron ChatGPT (OpenAI) y Claude (Anthropic) como apoyo para estructurar el código y el informe, mejorar la redacción y revisar la coherencia entre la rúbrica, el código y los resultados. Los resultados numéricos no fueron generados por estas herramientas: provienen de ejecutar el script R del equipo, que revisó los resultados y verificó la coherencia entre código, informe y presentación.

