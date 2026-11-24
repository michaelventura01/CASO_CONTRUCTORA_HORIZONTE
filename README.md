# Análisis técnico preliminar --- CASO_CONSTRUCTORA_HORIZONTE

**Repositorio:**
https://github.com/michaelventura01/CASO_CONTRUCTORA_HORIZONTE\
**Tipo de proyecto:** Análisis de datos y Machine Learning\
**Problema:** Predicción de días restantes por frente de trabajo en una
constructora.

## 1. Descripción general

El proyecto aborda un problema de predicción relacionado con la
estimación de los días restantes para completar frentes de trabajo en
una empresa constructora. Su enfoque no se limita al entrenamiento de
modelos: también considera la calidad de las métricas, los riesgos de
fuga de información (*data leakage*) y la utilidad operativa de las
predicciones.

La estructura visible del repositorio organiza el trabajo en cinco
etapas:

1.  Revisión de variables y partición de los datos.
2.  Identificación de posibles fugas de información en el
    preprocesamiento y la selección de modelos.
3.  Implementación de tres procedimientos.
4.  Evaluación de métricas, comparación de resultados y análisis de
    capacidad operativa.
5.  Identificación de métricas principales y potencialmente engañosas,
    revisión de coherencia y elaboración de conclusiones.

Entre los archivos identificados en la vista del repositorio se
encuentran:

-   `Asignación.docx`: documento con instrucciones del trabajo.
-   `Equipo 4 - Semana 2 - Podemos creer nuestras metricas.pptx`:
    presentación del equipo.

## 2. Evaluación metodológica preliminar

### 2.1 Preparación de los datos

Antes de entrenar los modelos, conviene verificar:

-   La definición exacta de la variable objetivo: días restantes.
-   La granularidad de cada registro: frente de trabajo, proyecto o
    fecha de observación.
-   La existencia de registros duplicados.
-   El tratamiento de valores nulos.
-   La presencia de valores atípicos.
-   La consistencia de las variables temporales.
-   La disponibilidad real de cada variable en el momento en que se
    emitiría una predicción.

La granularidad es especialmente importante. Si existen varias
observaciones del mismo frente de trabajo a lo largo del tiempo, una
partición aleatoria convencional podría colocar observaciones del mismo
frente tanto en entrenamiento como en prueba y producir una evaluación
demasiado optimista.

### 2.2 Prevención de Data Leakage

La fuga de información ocurre cuando el modelo utiliza, directa o
indirectamente, datos que no estarían disponibles en el momento real de
la predicción.

Ejemplos de variables que requieren revisión:

  -----------------------------------------------------------------------
  Variable                            Riesgo potencial
  ----------------------------------- -----------------------------------
  Fecha real de finalización          Fuga directa si se usa para
                                      predecir antes del cierre

  Días reales restantes calculados    Fuga directa
  posteriormente                      

  Porcentaje final de avance          Fuga si se conoce después de la
                                      fecha de predicción

  Fecha de cierre del proyecto        Fuga directa o variable no
                                      disponible

  Historial de avance disponible      Generalmente válido
  hasta la fecha de predicción        
  -----------------------------------------------------------------------

También debe evitarse ajustar imputadores, escaladores, codificadores y
métodos de selección de características utilizando el conjunto de
prueba. Estas transformaciones deben ajustarse exclusivamente con los
datos de entrenamiento, preferiblemente dentro de un `Pipeline`.

Si hay observaciones temporales, debe considerarse una separación
temporal o una validación agrupada por proyecto/frente, según el
escenario de uso. La estrategia debe representar cómo se utilizará el
modelo en producción.

### 2.3 Selección de modelos

Como punto de partida para un problema de regresión, se pueden comparar:

1.  **Linear Regression:** modelo de referencia sencillo e
    interpretable.
2.  **Random Forest Regressor:** permite capturar relaciones no lineales
    e interacciones.
3.  **Gradient Boosting / XGBoost:** alternativa basada en árboles
    potenciados que puede ofrecer buen rendimiento en datos tabulares.

La comparación debe realizarse utilizando las mismas particiones,
variables disponibles y criterios de evaluación. También conviene
incluir un modelo base (*baseline*), por ejemplo, predecir la mediana de
días restantes del conjunto de entrenamiento.

## 3. Métricas de evaluación

La predicción de días restantes es un problema de regresión. Las
métricas recomendadas son:

  -----------------------------------------------------------------------
  Métrica                 Qué mide                Consideración
  ----------------------- ----------------------- -----------------------
  MAE                     Error absoluto medio,   Fácil de interpretar y
                          expresado en días       recomendable como
                                                  métrica principal

  RMSE                    Raíz del error          Penaliza más los
                          cuadrático medio        errores grandes

  R²                      Proporción de           No expresa directamente
                          variabilidad explicada  el error en días
                          por el modelo           

  MAPE                    Error porcentual        Puede ser inestable
                          absoluto medio          cuando los valores
                                                  reales son cero o
                                                  cercanos a cero
  -----------------------------------------------------------------------

### Métrica principal recomendada: MAE

Un MAE de 5.2 significa que, en promedio, las predicciones se desvían
5.2 días de los valores reales. No significa que todas las predicciones
tengan un error máximo de 5.2 días.

La evaluación debería complementarse con:

-   Distribución de errores y análisis de residuos.
-   Porcentaje de predicciones dentro de tolerancias operativas, por
    ejemplo ±3, ±7 o ±14 días, si esos umbrales tienen sentido para el
    negocio.
-   Desempeño por proyecto, tipo de trabajo y etapa de ejecución.
-   Comparación frente al modelo base.
-   Análisis de sesgo: comprobar si el modelo tiende a subestimar o
    sobreestimar los días restantes.

## 4. Aplicación empresarial

El valor del proyecto consiste en transformar una estimación estadística
en información útil para la gestión de proyectos de construcción.

Una evolución posible sería desarrollar un sistema que permita:

-   Estimar fechas probables de finalización por frente de trabajo.
-   Identificar frentes con riesgo de retraso.
-   Comparar avance planificado y avance real.
-   Detectar proyectos con desviaciones importantes.
-   Simular escenarios de retraso.
-   Generar alertas para responsables de obra y gerentes de proyectos.
-   Priorizar intervenciones y asignación de recursos.

Las predicciones deberían presentarse como estimaciones sujetas a
incertidumbre, no como fechas garantizadas. Para decisiones operativas,
sería útil mostrar intervalos o rangos de predicción cuando la
metodología lo permita.

## 5. Fortalezas identificadas en la estructura visible

-   El trabajo está dividido en etapas metodológicas.
-   Se contempla explícitamente el riesgo de *data leakage*.
-   Se plantea implementar y comparar varios procedimientos.
-   Se considera la evaluación de métricas y su posible interpretación
    engañosa.
-   Se incluye una dimensión de capacidad operativa y conclusiones.

Estas observaciones corresponden a la estructura y los objetivos
visibles del repositorio; no constituyen una validación del código ni de
los resultados experimentales.

## 6. Aspectos pendientes de verificación

Para emitir una evaluación técnica definitiva es necesario revisar los
notebooks, scripts, datos y documentos completos. En particular:

-   Calidad, consistencia y representatividad del dataset.
-   Definición del objetivo y granularidad de los registros.
-   Implementación efectiva de los tres procedimientos.
-   Correctitud de las particiones de entrenamiento, validación y
    prueba.
-   Posibles fugas de información en variables y transformaciones.
-   Hiperparámetros y proceso de selección del modelo.
-   Resultados cuantitativos y reproducibilidad.
-   Capacidad de generalización a proyectos nuevos.
-   Viabilidad de uso operativo y monitoreo del modelo.

Por tanto, no corresponde asignar una puntuación definitiva de calidad
al modelo sin inspeccionar esos elementos.

## 7. Plan de auditoría técnica recomendado

  -----------------------------------------------------------------------
  Área                                Actividades
  ----------------------------------- -----------------------------------
  Data Engineering                    Revisar estructura, tipos de datos,
                                      granularidad, duplicados y calidad

  Data Science                        Revisar análisis exploratorio,
                                      distribuciones, correlaciones y
                                      selección de variables

  Machine Learning                    Auditar algoritmos,
                                      hiperparámetros, pipelines y
                                      validación

  Model Evaluation                    Verificar métricas, sobreajuste,
                                      generalización y análisis de
                                      errores

  Business Intelligence               Definir indicadores y
                                      visualizaciones para la gestión de
                                      proyectos

  MLOps                               Evaluar reproducibilidad,
                                      versionamiento, trazabilidad,
                                      despliegue y monitoreo
  -----------------------------------------------------------------------

## 8. Conclusión

CASO_CONSTRUCTORA_HORIZONTE plantea un problema relevante para la
gestión de obras: estimar los días restantes de los frentes de trabajo.
Su estructura visible muestra atención a cuestiones importantes de
Machine Learning, especialmente la prevención de *data leakage* y la
interpretación crítica de las métricas.

El siguiente paso es auditar el contenido real de los archivos y
reproducir los experimentos. La pregunta central no debe ser únicamente
qué modelo obtiene el menor error, sino si la evaluación representa el
escenario real de uso y si sus predicciones son suficientemente
confiables para apoyar decisiones en una constructora.

------------------------------------------------------------------------

**Alcance de este documento:** análisis preliminar basado en la
estructura y los elementos visibles del repositorio. No se han validado
internamente los notebooks, el dataset, el código ni los resultados. Las
recomendaciones técnicas son criterios propuestos para la auditoría, no
hallazgos confirmados sobre la implementación.
