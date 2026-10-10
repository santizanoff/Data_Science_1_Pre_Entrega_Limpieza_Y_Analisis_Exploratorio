# # Documentación

```markdown
# Proyecto de Análisis de Riesgo Crediticio

Este proyecto contiene el diagnóstico, la limpieza profunda y el análisis agregado de un dataset de riesgo crediticio enfocado en la toma de decisiones financieras.

## 1. Origen de los Datos
Los datos provienen de un archivo CSV remoto alojado en GitHub:
* **URL del dataset**: [credit_risk_dataset.csv](https://raw.githubusercontent.com/santizanoff/Dataset_Prueba/refs/heads/main/credit_risk_dataset.csv)
* **Descripción general**: Contiene información sociodemográfica y financiera de solicitantes de préstamos, incluyendo variables como edad, ingresos, tipo de vivienda, experiencia laboral, monto de préstamo solicitado, tasas de interés, historial de morosidad previa y el estado actual del préstamo (mora o al corriente).

## 2. Diagnóstico del Dataset
Tras una inspección inicial mediante los métodos `.info()`, `.describe()` y `.isnull().sum()`, se identificaron los siguientes puntos críticos:
* **Registros iniciales**: 32,581 registros.
* **Duplicados**: Presencia de registros idénticos en el sistema.
* **Valores atípicos (Outliers)**:
  * Edades máximas imposibles (como 144 años).
  * Inconsistencia lógica entre la edad (`person_age`) y los años de experiencia laboral (`person_emp_length`), donde la experiencia superaba los años de vida.
* **Valores faltantes**:
  * `person_emp_length`: 895 valores nulos.
  * `loan_int_rate`: 3,116 valores nulos.

## 3. Decisiones de Limpieza y Transformación Tomadas
Para garantizar la consistencia y la veracidad del análisis, se implementaron las siguientes acciones correctivas:

1. **Eliminación de Duplicados**: Se eliminaron 165 registros duplicados idénticos en todas sus columnas.
2. **Tratamiento de Outliers de Edad**: Se filtraron los registros descartando a personas mayores de 100 años.
3. **Tratamiento de Inconsistencias de Empleo**: Se mantuvieron únicamente las filas donde la experiencia laboral (`person_emp_length`) fuera estrictamente menor que la edad del aplicante (`person_age`).
4. **Imputación de Valores Faltantes**: Se utilizó la **mediana** de las columnas numéricas respectivas para rellenar los datos nulos de manera robusta, evitando que los outliers residuales sesgaran los resultados:
   - `person_emp_length` imputada con su mediana.
   - `loan_int_rate` imputada con su mediana.
5. **Reseteo de Índices (Buenas Prácticas)**: Se aplicó un reindexado físico (`reset_index(drop=True)`) tras haber removido los duplicados y las filas inconsistentes, previniendo errores de alineación en cálculos matemáticos posteriores.

**Resultado final**: Un conjunto de datos completamente consistente con **31,522 registros** y **0 valores nulos**.

## 4. Principales Hallazgos de Negocio
A través de agregaciones grupales (`groupby`) determinamos patrones de riesgo clave:
* **Vivienda**: Los inquilinos (`RENT`) representan la tasa de morosidad más alta (~31.1%), mientras que las personas que son propietarias plenas de sus hogares (`OWN`) registran un índice de impago mínimo de apenas el 6.9%.
* **Propósito**: Consolidar deudas (`DEBTCONSOLIDATION`), salud (`MEDICAL`) y remodelaciones del hogar (`HOMEIMPROVEMENT`) representan las mayores tasas de morosidad relativa.
* **Calificación crediticia (Grade)**: El sistema de calificación actual es altamente predictivo; los grados excelentes (A y B) exhiben tasas de mora menores al 16%, mientras que los grados de alto riesgo (D a G) superan holgadamente el 50% de probabilidad de impago.
```
