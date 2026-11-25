# Contexto del proyecto

Tarea Académica del curso 1INF03 Análisis de datos (PUCP, 2026-2). Aplica el proceso KDD de punta a punta con datos reales de una institución y termina en un clustering. El trabajo se hace en Python (pandas, NumPy, scikit-learn, Matplotlib, openpyxl) con notebooks por etapa.

## Problema

El Puericultorio Pérez Araníbar es un centro de acogida residencial de Lima. Atiende a niñas, niños y adolescentes (NNA) de 0 a 17 años sin cuidado familiar. Hoy hay 134 residentes y no entran nuevos desde julio de 2025.

- La norma da 18 meses para que la familia de origen supere los riesgos. En la práctica los procesos duran años. La mitad de los residentes lleva más de 5 años en el centro y 93 de 134 siguen en desprotección provisional.
- El Plan de Trabajo Individual (PTI) no siempre se elabora ni se actualiza. 30 residentes lo tienen vencido y 21 no tienen uno.
- La información de cada residente está repartida entre la ficha nominal (que se reporta al MIMP), las planillas del área de nutrición y los expedientes físicos. Además tiene errores de registro: códigos repetidos o contradictorios, fechas en formatos distintos y campos que no se actualizan.
- Resultado: el equipo técnico (social, psicología, nutrición, medicina y legal) conoce cada caso por separado, pero no tiene una visión conjunta. No puede reconocer grupos con necesidades parecidas ni coordinar a las áreas.

## Qué se quiere lograr

**Objetivo de negocio:** aprovechar los registros para identificar perfiles de residentes con situaciones compartidas, y así apoyar la planificación y la coordinación entre áreas. Los perfiles deben servir para:

- agrupar residentes con condiciones similares (legal, administrativa, educativa, nutricional y psicoemocional);
- detectar grupos con situaciones prolongadas (provisional por más de 18 meses o sin PTI vigente);
- hacer revisiones interdisciplinarias por perfil y validarlas con el equipo técnico.

**Objetivo de análisis:** identificar y caracterizar esos perfiles con clustering, después de integrar, depurar y transformar la ficha nominal y las evaluaciones nutricionales. Hay que construir y comparar al menos 3 modelos de clustering. Se comparan por separación, cohesión, estabilidad e interpretabilidad. Luego se interpretan los perfiles para el equipo técnico.

Los resultados son informativos. No reemplazan las decisiones legales ni las evaluaciones profesionales.

## Data original (entregada por la institución)

- **Ficha nominal consolidada:** 1 archivo con 2 667 filas × 33 columnas. Cada fila es un NNA en un mes (enero 2025 a julio 2026, 19 meses), con 157 códigos distintos. Las variables son sobre todo categóricas y de fecha, y están en seis grupos:
  - datos personales (sexo, nacimiento, procedencia, nacionalidad);
  - ingreso (fecha, forma, documento, resolución, oficio);
  - situación legal (situación, UPE o juzgado, comunicado a la DGA);
  - PTI (fecha, vigencia, estado, instancia);
  - educación (grado, nivel, tipo de institución, si estudia);
  - salud y soporte psicoemocional (salud física, discapacidad, nivel nutricional, soporte, salud mental, seguro).
- **Evaluaciones nutricionales:** 6 libros de cálculo (ene, feb, mar, abr, jun y ago 2026) con 402 evaluaciones de peso, talla, IMC y diagnósticos (P/E, T/E, P/T, IMC/E). Cada hogar ocupa una hoja, con encabezados en varias filas y celdas combinadas. Las plantillas cambian entre meses y la cobertura es desigual: feb y mar solo infantes, ago solo adolescentes de un hogar, y no hay mayo ni julio. Los diagnósticos vienen en dos bloques (percentiles OMS y tabla rápida INS/CENAN) que no siempre coinciden.
- **Referencias externas:** tablas CENAN/INS y curvas OMS para recalcular los diagnósticos nutricionales.
- **Limitaciones clave:** no hay fechas ni motivos de egreso. El PTI solo registra su estado, no sus acciones. Solo hay 134 residentes en el corte final, lo que limita la estabilidad de los clusters.

## Qué se espera en cada paso (KDD)

1. **Entendimiento del negocio:** definir contexto, problema, objetivos de negocio y de análisis, población, recursos, limitaciones y acciones posibles.
2. **Selección y entendimiento de datos:** identificar fuentes y estructura, y documentar con evidencia los problemas de calidad (formatos, fechas, IDs inconsistentes, cronologías imposibles, plantillas distintas, campos sin variación), sin modificar los datos.
3. **Preprocesamiento:** aplicar tratamientos justificados a cada problema (excluir IDs contradictorios, estandarizar texto y fechas, imputar o completar el IMC, recalcular diagnósticos con OMS/INS) y dejar un dataset unificado con trazabilidad.
4. **Transformación:** pasar a un registro por residente, con variables derivadas (tiempo de permanencia, cambios históricos, estado del PTI, nutrición reciente), codificación y escalado, y reducción de dimensiones (EFA/PCA, con KMO y varianza explicada).
5. **Modelado (clustering):** construir al menos 3 modelos (por ejemplo K-means, jerárquico y DBSCAN o GMM), con búsqueda del número de grupos y de hiperparámetros.
6. **Evaluación e interpretación:** comparar los modelos con métricas internas (silueta, cohesión y separación) y estabilidad. Elegir el mejor, caracterizar cada perfil y traducirlo a conclusiones y acciones para el equipo técnico.

> Nota: la descripción de los pasos 4 a 6 es una inferencia a partir de los nombres de las clases (Step4 EFA, Step5 y 6, Step7 Clustering). No se pudo confirmar contra los PDF del curso (Guidelines y Rubric).
