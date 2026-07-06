# Focalización de programas de apoyo educativo mediante predicción de pobreza

Taller de Machine Learning aplicado a política pública.
Curso: Aprendizaje de Máquinas y Análisis de Datos, Bogotá Summer School in Economics 2026.
Autora: Daniela Solano.

---

## Pregunta

¿Se puede identificar a los hogares colombianos en situación de pobreza a partir de
características socioeconómicas observables (educación, composición del hogar, vivienda,
empleo, privaciones), **sin usar la medición directa del ingreso**, para apoyar la
focalización de programas de apoyo educativo?

---

## Idea central

El modelo funciona como un *proxy means test*: predice la condición de pobreza del hogar
usando variables fáciles de observar, que es el mecanismo con que Colombia focaliza
programas sociales (SISBÉN, y con él becas como Generación E o Jóvenes en Acción). El
umbral de decisión es la palanca de política: equilibra el error de exclusión (dejar por
fuera a un hogar vulnerable) contra el error de inclusión (dar el apoyo a quien no lo
necesita).

---

## Datos

- **Fuente:** GEIH (DANE), procesada a nivel de hogar.
- **Unidad de observación:** el hogar.
- **Tamaño:** 154.393 hogares.
- **Variable objetivo:** `pobre` (1 = hogar pobre según la línea de pobreza del DANE).
- **Nota:** el ingreso monetario que define la pobreza (ingreso total, per cápita y líneas)
  no está en la base; se eliminó en la limpieza. Por eso el ejercicio no es circular.

Los datos provienen del trabajo previo del Equipo 04 (Problem Set 2). Este taller es un
desarrollo individual sobre esa base.

---

## Modelos

Se comparan tres modelos vistos en el curso:

- **Ridge** (`glmnet`, alpha = 0): regresión logística penalizada.
- **Lasso** (`glmnet`, alpha = 1): penalización que además selecciona variables.
- **Random Forest**: ensamble de árboles, captura no linealidades.

Se maneja el desbalance de clase con re-muestreo (`ROSE`) y se ajusta el umbral de decisión
a partir de la curva ROC.

---

## Cómo ejecutar

### Requisitos
- R y RStudio.
- Los paquetes se instalan solos al ejecutar (tidyverse, glmnet, randomForest, caret,
  pROC, ROSE, knitr).

### Pasos
1. Clona o descarga este repositorio.
2. Abre `taller_focalizacion.Rmd` en RStudio.
3. Asegúrate de que el directorio de trabajo sea la carpeta del repositorio
   (menú Session, Set Working Directory, To Source File Location).
4. Pulsa **Knit** (o `Cmd/Ctrl + Shift + K`).

El script lee los datos desde `datos/train_final.rds`, corre el análisis completo y genera
el reporte `taller_focalizacion.html`, además de guardar las gráficas en `outputs/`.

---

## Estructura del repositorio

```
Focalizacion-pobreza/
├── README.md
├── .gitignore
├── taller_focalizacion.Rmd        # Análisis reproducible
├── taller_focalizacion.html       # Reporte generado
├── datos/
│   ├── train_final.rds
│   ├── test_final.rds
│   └── DiccionarioDatos.md
└── outputs/                        # Gráficas generadas al hacer Knit
```

---

## Ver el reporte

GitHub no renderiza los archivos HTML (muestra el código). Para ver el reporte
`taller_focalizacion.html`, descárgalo (botón "Download raw file") y ábrelo en el navegador,
o reprodúcelo con Knit.

---

## Limitaciones

Es un modelo predictivo, no causal. La pobreza monetaria es solo una dimensión de la
vulnerabilidad. El desempeño depende de la calidad y vigencia de la GEIH, por lo que
requeriría reentrenamiento periódico.
