---
sidebar_label: 'Snomed'
---

# Snomed

Url Browser: http://snomed.dim.com.ar:80

Url Swagger: http://snomed.dim.com.ar:8080/swagger
http://snomed.dim.com.ar:8080/swagger-ui/index.html#/MultiSearch/findDescriptions_1

Url Portainer: https://snomed.dim.com.ar:9443/

Url Elastic: https://snomed.dim.com.ar:9200/

IP: 10.21.0.34

Temas a resolver:
- Entender concepto y estructura de Snomed
- Hostear BD de Snomed en Elastic usando Snowsotrm para acceso a data mediante API
- Formular y calibrar la búsqueda de terminos para llegar a lo que se tiene que ofrecer a los médicos desde HC
- Convivencia entre diagnósticos usados desde Bd de historia clínica y la nueva base de datos.
- Mejorar el circuito de mantenimiento del diagnóstico presuntivo y de diagnósticos confirmados

Tablas de HC actuales:
DIAGNOSTICOS: Se guardan los registros de todos los diagnósticos que se usan en entorno HC

DIAGNOSTICOEVOLUCIONES: Diagnóstico confirmado que se puede observar en el front de HC como un diagnóstico del paciente.

DIAGPRESUNTIVOEVOLUCIONES: Relación entre evolución y diagnóstico usado



El problema con el esquema actual es que no podemos hacer un mantenimiento eficiente de los diagnósticos presuntivos o confirmados del paciente. Tenemos que agregar un campo para indicar si el diagnóstico se encuentra resuelto y así poder trabajar en una funcionalidad para que un profesional pueda indicar si hay un diagnóstico confirmado que ya no representa una condición activa en el paciente, quedando como un antecedente.

Otro problema es que el diagnóstico presuntivo no hace al circuito para que termine siendo un diagnóstico confirmado cuando corresponda. 



Se tiene que consultar al profesional si el diagnóstico presuntivo indicado en una evolución previa del paciente es un diagnóstico confirmado o no. Esto se puede hacer sin preguntar de manera directa sino dando la posibilidad de hacerlo mediante un recuadro con diagnósticos presuntivos. Es importante permitir al médico indicar diagnósticos del paciente de manera directa, solicitando completar campos para hacerlo.

Para poder llevar a cabo las funcioanlidades mencionadas necesitamos agregar datos a las tablas que asocian los diagnósticos a las evoluciones. Se agrega una fecha de alta y el ID de usuario ya que pueden no agregarse solamente desde una evolución o se puede cofirmar un diangóstico presuntivo de manera posterior a la evolución.
