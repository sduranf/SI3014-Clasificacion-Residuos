# Clasificación automática de residuos reciclables con transfer learning

Proyecto final de **SI3014 – Redes Neuronales y Aprendizaje Profundo** (Universidad EAFIT, 2026-2).
Lineamientos oficiales: [`Project/README.md`](https://github.com/jdmartinev/SI3014---Redes-Neuronales-y-Aprendizaje-Profundo/blob/main/Project/README.md) del repositorio del curso.

**Integrantes:** Nicolás Zapata Jurado, Sebastián Durán Fernández y Thomas Buitrago.

## Anteproyecto (entrega preliminar, 30 de octubre de 2026)

**Título:** Clasificación automática de residuos reciclables mediante transfer learning con redes neuronales convolucionales

**Objetivo general:** Desarrollar un modelo de clasificación de imágenes basado en redes neuronales convolucionales preentrenadas (transfer learning) que identifique seis categorías de residuos sólidos (cartón, vidrio, metal, papel, plástico y residuo no reciclable) a partir de fotografías, alcanzando un F1-score macro de al menos 0,85 en el conjunto de prueba.

**Objetivos específicos:**

1. Preparar el conjunto de datos TrashNet (2527 imágenes, 6 clases) mediante redimensionamiento a 224 × 224 píxeles, normalización con las estadísticas de ImageNet, aumento de datos (*data augmentation*) y una división estratificada en entrenamiento, validación y prueba (70/15/15).
2. Entrenar y comparar dos estrategias de transfer learning sobre una CNN preentrenada (ResNet-18): extracción de características con el backbone congelado y ajuste fino (*fine-tuning*) de las últimas capas, analizando las curvas de pérdida y desempeño y el efecto de los hiperparámetros.
3. Evaluar el mejor modelo en el conjunto de prueba y en un conjunto externo de fotografías propias de residuos tomadas con celular en condiciones reales, cuantificando la variación del desempeño (accuracy, F1-score y matriz de confusión) y analizando las causas de los errores.

Versión en PDF: [`docs/Anteproyecto_SI3014_Residuos.pdf`](docs/Anteproyecto_SI3014_Residuos.pdf)

## Estructura del repositorio

| Archivo | Contenido |
| --- | --- |
| `proyecto_residuos.ipynb` | Notebook completo: datos, entrenamiento, evaluación, generalización y Grad-CAM |
| `docs/Anteproyecto_SI3014_Residuos.pdf` | Entrega preliminar (título y objetivos) |

## Cómo correr el notebook

1. Subir `proyecto_residuos.ipynb` a [Google Colab](https://colab.research.google.com/) (*Archivo → Subir notebook*).
2. Activar GPU: *Entorno de ejecución → Cambiar tipo de entorno de ejecución → T4 GPU*.
3. *Entorno de ejecución → Ejecutar todas*. El dataset (~43 MB) y los pesos de ResNet-18 se descargan solos.
4. Para la evaluación externa: tomar las fotos, organizarlas en `externas/cardboard`, `externas/glass`, `externas/metal`, `externas/paper`, `externas/plastic` y `externas/trash`, comprimir la carpeta como `externas.zip`, subirla al panel *Archivos* de Colab y volver a ejecutar desde la sección 9.
5. La última celda descarga `resultados.zip` con todas las figuras y el modelo entrenado.

`MODO_RAPIDO = True` (primera celda) entrena con 30 imágenes por clase y 1 época. Sirve para comprobar en un par de minutos que todo corre, pero sus métricas no valen para el informe.

| Sección del notebook | Qué produce | Para qué parte de la rúbrica |
| --- | --- | --- |
| 0 | Hardware (CPU, GPU, RAM) | 4.4 Recursos computacionales |
| 1–3 | Descarga, conteo por clase, ejemplos, tabla de división | 4.3 Base de datos |
| 4 | Transformaciones y figura de *data augmentation* | 4.3 Preprocesamiento |
| 5 | Arquitectura, referencia y modificaciones | 4.2 Modelo |
| 6–7 | Curvas de pérdida, accuracy y F1 de las etapas A y B, tabla comparativa, tiempos | 4.4 Entrenamiento |
| 8 | Reporte por clase, matriz de confusión y errores en prueba | 4.5 Resultados |
| 9 | Métricas con fotos externas y comparación prueba vs. externas | 4.5 Generalización |
| 10 | Grad-CAM (dónde mira el modelo) | 4.5 Análisis de fallas |
| 11 | Predicción de una foto (demo en vivo) | Sustentación |
| 12 | Tabla resumen con hiperparámetros, tiempos y métricas | Informe |

## Cronograma (30 de octubre – 11 de noviembre)

| Fecha | Tarea | Resultado |
| --- | --- | --- |
| Vie 30 oct | Subir el link de este repositorio a Interactiva | Entrega preliminar |
| Sáb 31 oct – dom 1 nov | Correr el notebook completo en Colab con GPU y revisar exploración y curvas | `figuras/` |
| Sáb 31 oct – mar 3 nov | Tomar las fotos externas: 15–25 por clase, con fondos e iluminación variados | `externas.zip` |
| Mar 3 – mié 4 nov | Evaluación externa y Grad-CAM; uno o dos experimentos extra (sin pesos de clase, otra tasa de aprendizaje o ResNet-50) | Tabla de resultados |
| Jue 5 – sáb 7 nov | Análisis de resultados y redacción del informe (máximo 3 páginas, PDF) | Borrador del informe |
| Dom 8 – lun 9 nov | Diapositivas (15 minutos) | Presentación |
| Mar 10 nov | Ensayo cronometrado, repaso de preguntas, revisión final | Versión final |
| **Mié 11 nov** | **Entregar el informe en PDF y la presentación** (Interactiva y correo) | Entrega final |

Después del 11 de noviembre no se aceptan cambios, y la sustentación se califica solo con lo entregado en esa fecha.

## Guion de la presentación (15 min + 5 de preguntas)

Todos los integrantes deben exponer y cualquiera puede recibir preguntas sobre cualquier parte del proyecto.

1. **Problema y objetivos (2 min):** separación de residuos en la fuente, por qué automatizarla, objetivo general y específicos.
2. **Datos y preprocesamiento (2,5 min):** TrashNet, distribución de clases y desbalance, división 70/15/15, normalización y *augmentation* con su justificación.
3. **Modelo (2,5 min):** ResNet-18 con conexiones residuales, referencia (He et al., 2016), nueva cabeza `Dropout + Linear(512, 6)`, estrategia en dos etapas.
4. **Entrenamiento (3 min):** curvas de ambas etapas **interpretadas** (sobreajuste, subajuste, convergencia), hiperparámetros, pérdida con pesos por clase, AdamW y coseno, GPU y tiempos.
5. **Resultados (3,5 min):** métricas en prueba, matriz de confusión, errores típicos, **comparación con las fotos externas**, Grad-CAM y demo en vivo.
6. **Conclusiones (1,5 min):** cumplimiento de cada objetivo con evidencia, limitaciones y mejoras posibles.

## Estructura del informe (máximo 3 páginas)

1. **Objetivos** (~⅓ de página): problema, objetivo general y específicos.
2. **Base de datos** (~½ página): origen, tamaño, clases, desbalance, preprocesamiento y división.
3. **Modelo implementado** (~½ página): arquitectura, justificación, las dos etapas e hiperparámetros.
4. **Resultados** (~1 página): tabla comparativa A vs. B, curvas de la etapa B, matriz de confusión, tabla prueba vs. externas.
5. **Conclusiones** (~⅓ de página): hallazgos, cumplimiento de objetivos y limitaciones.

## Preguntas probables en la sustentación

- **¿Por qué transfer learning y no entrenar desde cero?** Con unas 1770 imágenes de entrenamiento, una CNN desde cero sobreajusta. Los filtros aprendidos en ImageNet (bordes, texturas) ya sirven para distinguir materiales.
- **¿Por qué ResNet-18?** Es liviana (11,7 M de parámetros) y entrena en minutos en una T4. Sus conexiones residuales evitan el desvanecimiento del gradiente, y para un dataset de este tamaño una red más grande no garantiza mejores resultados.
- **¿Por qué F1 macro y no solo accuracy?** La clase `trash` tiene 137 imágenes frente a 594 de `paper`. La accuracy puede ser alta aunque el modelo falle en la clase minoritaria, mientras que el F1 macro pesa todas las clases por igual.
- **¿Para qué sirven los pesos por clase en la pérdida?** Hacen que un error en una clase minoritaria cueste más, para que el modelo no la ignore.
- **¿Por qué dos tasas de aprendizaje en el fine-tuning?** El backbone ya tiene buenos pesos y una tasa alta los destruiría. La cabeza es nueva y necesita moverse más rápido.
- **¿Por qué las BatchNorm congeladas se dejan en modo `eval`?** Si estuvieran en modo `train`, actualizarían sus medias y varianzas con nuestros datos aunque sus pesos estén congelados, y el extractor dejaría de ser el de ImageNet.
- **¿Por qué se elige el modelo con validación y no con prueba?** Si se elige con prueba, esa métrica deja de ser una estimación honesta del desempeño con datos nuevos.
- **¿Por qué empeora con las fotos del celular?** TrashNet tiene fondo blanco e iluminación controlada, y las fotos reales tienen fondos, sombras y varios objetos. Es un cambio de distribución (*domain shift*), y Grad-CAM muestra si el modelo termina mirando el fondo.

## Referencias

- G. Thung y M. Yang, *Classification of Trash for Recyclability Status*, CS229 Project Report, Stanford University, 2016. Dataset: <https://github.com/garythung/trashnet>
- K. He, X. Zhang, S. Ren y J. Sun, *Deep Residual Learning for Image Recognition*, CVPR, 2016.
- R. R. Selvaraju et al., *Grad-CAM: Visual Explanations from Deep Networks via Gradient-based Localization*, ICCV, 2017.
- I. Loshchilov y F. Hutter, *Decoupled Weight Decay Regularization* (AdamW), ICLR, 2019.
