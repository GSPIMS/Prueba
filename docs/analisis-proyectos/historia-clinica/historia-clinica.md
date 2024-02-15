---
sidebar_label: 'Historia Clínica'
---

# Historia Clínica

Figma: https://www.figma.com/file/fsR5v99toTA6WpKYNlGCne/Historia-Cl%C3%ADnica-React?node-id=0%3A1

Próximos desarrollos:
Funcionalidades a realizar:

Agregar concepto de diagnóstico presuntivo en base a SNOMED y la posibilidad de agregarlo desde la evolución (punto obligatorio). El diagnóstico tiene que tener estado para indicar si está o no confirmado. Este proceso va a depender de preguntarle al médico si confirma diagnósticos presuntivos. 

Para lo anterior, analizar SNOMED y aplicación.

Orden de laboratorio: Migrar lo ya funcional de Angular, aplicando mejoras de uso como la generación de la orden desde el guardado de la evolución, permitiendo hacer modificaciones hasta ese punto.

Orden de prácticas: Similar a lo realizado por laboratorio pero para prácticas a partir de los grupos médicos.

Medicamentos o indicaciones farmacológicas:

Investigar Vademécum Vidal para aplicación general y uso de contra indicaciones o advertencias.

Asignación de un medicamento como en HC actual con mejoras de uso. Se tiene que mostrar que el paciente está tomando el medicamento y alertar si alguno se asigno de manera permanente a los X meses de su asignación para preguntarle al médico si el paciente lo sigue tomando. 

Estudios previos del paciente: Traer listado de estudios previos para que puedan visualizar informes, imágenes y laboratorios del paciente (como en HC actual), Permitiendo copiar texto a evoluciones.

Enfermedades y antecedentes: En base a análisis de SNOMED. Se tienen que poder agregar y son informativas.

Examen físico: Definición de examen por especialidad con los datos básicos para cada una. Con posibilidad de agregar datos adicionales a nivel de texto. Tiene que ser un máximo de 10 datos.

Línea de tiempo con información de prestaciones que el paciente debe realizar según buenas prácticas. Definiendo como van a funcionar las relaciones y condicionales que hagan a la lógica de sugerencias.

Pantalla de armado de query para poder buscar pacientes con X diagnósticos, confirmados o sin confirmar, tomando X medicamento, X enfermedades y/o antecedentes.

Orden digital de laboratorio:
La orden digital de laboratorio es la orden que se hace desde el entorno de la historia clínica, especificamente desde la evolución de los pacientes. Cuando se genera una orden digital se va a crear un protocolo (entidad principal para la atención de laboratorio) [LAB_PROTOCOLO] el cual se encuentra asociado al paciente sobre el cual el médico está haciendo la evolución. Este protocolo se crea en base a:

Paciente que se está evolcuionando.

El médico solicitante es el médico que genera la orden digital.

La orden del protocolo es la que se compone de las determinaciones [LAB_NOMENCLADOR] que el médico selecciono al generar la orden digital.

Al ingresar al modal para generar la orden digital, el médico primero ve todas las ordenes que se encuentran predefinidas para el mismo. Estos grupos [LAB_GRUPOESTUDIO] se crean seleccionando las determinaciones que se van a encontrar en la orden generada y relacionandolos a un médico mediante su matrícula provincial. Así es como le aparecen a cada médico los grupos asociados al mismo. La relación es mediante el usuario de HC (completar)

En base a los grupos anteriores, el médico puede seleccionar uno y avanzar hasta la generación de la orden, o modificarlo solamente para esa solicitud. Una modificación generada desde la labor del médico no genera una modificación permamente al grupo en cuestión.

Los grupos de determinaciones se crean y se editan desde el sistema de turnos en el menú de Laboratorio. Los profesionales no cuentan con una herramienta para la creación directa de un grupo de determinaciones.

Pantallas (Modales)
Evolución actual
Sobre esta pantalla se agrega la opción para poder acceder al modal mediante el cual se van a generar las ordenes digitales

Combos predefinidos:
Al abrir el modal, el profesional va a ver todos los grupos antes mencionados ya creados desde el sistema de turnos para poder seleccionar uno de estos según necesite. También desde esta pantalla va a poder empezar con la creación de una orden nueva.

Cada grupo de orden se ve con su titulo y el listado de determinaciones que contiene. En caso de ser listados muy grandes, vamos a usar un botón “Ver más” para que se pueda ver el listado completo.

Si el profesional selecciona una orden y avanza, va a la pantalla de resumen de orden y definición de diagnóstico.

Si el profesional selecciona la opción para crear una orden nueva, avanza hacía la pantalla de selección de determinaciones.

Resumen de orden
Esta pantalla muestra un formato de orden digital con todas las determinaciones que contiene la misma. Es en esta pantalla donde se solicita el diagnóstico que debe indicar el profesional y que va a salir impreso en la orden (dato obligatorio para generar la orden).

Es posible modificar las determinaciones que se encuentran listadas en la orden mediante un botón “Modificar” que se encuentra por debajo del listado de determinaciones que aparecen en la orden. Estas modificaciones no generan un cambio permanente en el grupo seleccionado previamente, sino que es una modificación únicamente para la orden que se está generando en esta instancia. Si selecciona la opción de modificar, va a avanzar a la pantalla de “Edición de orden”. Cabe aclarar que para editar la orden no es obligatorio contar con el diagnóstico ingresado.

El profesional tiene también la opción de cambiar la fecha de generación de orden para que tomemos el día seleccionado como el día a partir del cual se genera la orden. Específicamente es esta fecha la que se va a usar como FECHA DE ESTUDIO del protocolo [LAB_PROTOCOLO].

Edición de orden
A esta pantalla el profesional puede llegar desde la creación de una orden nueva o desde la modificación de una orden pre definida. En la misma se va a mostrar un listado de varias determinaciones (las más usadas) con un checkbox a la izquierda, y un campo de búsqueda para buscar otras determinaciones. El profesional puede ir seleccionando interactuando con los checkbox o con los textos para ir agregando o quitando determinaciones, así como buscando desde el campo de búsqueda. Desde este último, se van a ir agregando por debajo del listado como elementos tabulados. Si el profesional busca y agrega una determinación que se encuentra en el listado superior, se marca directamente el checkbox de dicha determinación.

En el caso que el profesional se encuentre generando una orden nueva, este listado se va a ver sin ninguna determinación seleccionada ni elementos tabulados agregados.

Si el profesional ingresa desde la edición de una orden predefinida, tienen que aparecer ya seleccionadas (o agregadas como tabuladas) todas las determinaciones que se contienen en la orden que se está editando.

Cuando el profesional termina la edición y toca “Guardar”, tenemos que actualizar el listado de determinaciones que pertenece a la orden (protocolo) que se va a generar.

Generación de protocolo y orden
Terminado el circuito el profesional va a seleccionar la opción de “Generar orden” para volver a la evolución actual (cierra el modal). Esto no tiene que generar la orden en este momento, sino que tiene que guardar el registro para que se genere la orden al momento de guardar la evolución. De esta manera, permitimos editar la orden ya creada las veces que sea necesario (mediante el componente que representa la orden en la evolución) hasta guardar la evolución. Una vez guardada se generan todas las ordenes que se hayan generado en dicha evolución.

Parametrización de OS
Tenemos que agregar un campo en la tabla de parametros de obra social para poder indicar que obra social permite hacer una orden digital de laboratorio. Esto es hoy un hardcodeo para el panorama actual con laboratorio. Tenemos que limpiar el código y llevar esto a un parámetro que maneje contrataciones según coordinen con las obras sociales. 

Orden digital de prácticas:
Descripción: Generación de ordenes digitales para solicitudes de estudios médicos o prácticas con todas las opciones necesarias para replicar la solicitud en papel. 

Objetivo: Facilitar y agilizar la solicitud de estudios médicos para que el profesional pueda hacerlo todo desde donde realiza la evolución del paciente.

Encargado:

Integrantes: MM, GMS, FB, SC

Tiempos:

Análisis:

Armado de pantallas y diseño:

Desarrollo:

Pruebas:

Correcciones:

Diseño (Circuito): 

Alcance funcional:
Permitir crear una orden de prácticas desde la historia clínica generando un registro que se asocie al paciente, médico y prestaciones agregadas a dicha orden.

La entidad de búsqueda para la selección de lo que se va a solicitar es a través de los grupos médicos.

Los grupos médicos se pueden agregar mediante búsqueda directa, o selección de listado de más usado por el médico que se encuentra evolucionando o listado de más usado por la especialidad.

Para el listado de grupos médicos más usados, tenemos que llevar un contador que relacione al médico con el grupo médico y un contador de cantidad de usos según ordenes generadas.

Si se agregan distintos grupos médicos, tenemos que revisar a que circuito pertenecen. Si pertenencen al mismo enotonces es una sola orden, caso contrario son ordenes distintas.

La creación del registro de orden digital se va a llevar a cabo cuando el profesional guarda la evolución. Hasta este punto el mismo puede hacer las modificaciones que desee (quitar grupos médicos o agregar otros)

Envío de notificación y/o mail al paciente para que asocie las ordenes solicitadas a turnos.

Nueva pantalla para el portal de pacientes donde estos puedan ver las ordenes digitale solicitadas como un elemento, desde el cual pueden asociar a turnos. Cada orden va a ser un turno.

Pantalla en .net donde se pueda ver la cantida de ordenes digitales realizadas y cuantas se encuentran asociadas a un turno y cuales (y cuales) no. Volcando en la tabla nombre del paciente, OS y teléfono de contacto.

Descripción del proyecto
La orden digital de prácticas permite al profesional crear una orden para estudios solicitados (prácticas) desde la evolución del paciente. Esta orden tiene que generar un registro en una nueva tabla [ORDENDIGITAL] donde se van a ir guardando todos los registros de ordenes generadas. Esto es así porque el profesional no va a asignar el turno que se encuentre asociado a la orden generada. En el caso de laboratorio, la orden se genera en base a un protocolo que no se asocia a un turno, por la estructura que permite dicho manejo.

La selección de los estudios solicitados se va a hacer mediante los grupos médicos [GRUPOS_MEDICOS], los cuales se usan para la búsqueda de turnos desde el sistema o desde el portal de pacientes. El profesional va a poder seleccionar uno o varios de estos grupos y los mismos se van a ir agregando a un modelo de orden. Hay 2 puntos importante a tener en cuenta en cuanto al desarrollo de esta funcionalidad:

La selección de grupos médicos se va a hacer en base a 3 metodos:

Selección de grupo médico desde listado de sugeridos para la especialidad a la que pertenece la agenda sobre la que se está evolucionando. Para esto vamos a necesitar de una tabla o ampliación de otra donde podamos relacionar especialidades a grupos médicos, permitiendo asignar un orden de aparición. Tiene que llegar a una pantalla configurable desde sistema de turnos.

Selección de grupo médico desde listado de sugeridos para el médico que se encuentra evolucionando. Para esto vamos a tener que crear un contador que nos permita saber cuales son los grupos médicos que más se utilizan en las ordenes creadas por el médico que se encuentra haciendo una orden. Tenemos que relacionar ID de médico con los grupos médicos y llevar conteo de cantidad de usos. Esto se tiene que poder modificar desde el sistema para resetear el conteo o eliminar el conteo de un grupo especifico para la reacción con un médico determinado.

Búsqueda desde un campo donde se busca en base a todos los grupos médicos cargados en sistema.

La creación de la o las ordenes se va a hacer automáticamente dependiendo de lo que corresponda, según los grupos médicos que el profesional haya cargado. Hay ciertas prestaciones que pueden encontrarse en una misma orden y otras que deben ir en ordenes separadas. Para esto tenemos que identificar que es lo que se está solicitando (ANALIZAR QUE LO DEFINE). Tenemos que utilizar el seteo de CRCUITOS por grupo médico, para agregar en una misma orden las que comparten un circuito. De esta manera la búsqueda de un turno posterior va a contemplar todos los grupos médicos que tengan que ir juntos en un solo turno.

Pantallas (Modales)
Generación de orden
En el caso de las ordenes de prácticas, vamos a usar un único modal donde vamos a ver listados los grupos médicos a seleccionar en base a los 2 listados mencionados: Sugeridos para la especialidad y sugeridos para el médico. 

Aparte vamos a contar con un buscador para que el profesional pueda buscar otros grupos médicos que no se encuentran en los listados.

A medida que se van seleccionando de los listados o desde la búsqueda, estos grupos médicos los vamos a ir agregando a un modelo de orden que aparece en el margen derecho de la pantalla (“Agregados en esta consulta”). Desde esta listado se pueden quitar con un botón “Quitar” al margen izquierdo de cada grupo médico previamente agregado. 

Agregar detalle de diagnóstico. 

.NET
Tenemos que contar con una pantalla en donde se puedan visualizar todos los registros de ordenes digitales de prácticas generadas con datos del paciente para que se pueda filtrar que registro está asociado a un turno y cual no. 

Tenemos que contar con un servicio que envíe mail cuando exista una orden digital, para llevar al paciente a una nueva pantalla de ordenes digitales. Mediante un token se logue al paciente al portal y se lo lleva directamente a la búsqueda de turnos.

Parametrización de OS
Tenemos que agregar un campo en la tabla de parametros de obra social para poder indicar que obra social permite hacer una orden digital de estudios médicos. 

Portal Web (pacientes)
Para que el paciente pueda interactuar con sus ordenes digitales (solicitudes desde HC) y relacionarlas a turnos, tenemos que contar con una pantalla donde pueda ver todas las solicitudes y mediante un click, buscar turno para la orden seleccionada. Teniendo en cuenta que si los grupos médicos seleccionados comparten el circuito, deben de ir en una única orden que va a asociarse a un único turno.

Tablas Necesarias
 [ORDENDIGITAL]:

Fecha de realización. (modifica esta fecha el campo de fecha diferida)

Profesional que genera la orden.

Paciente que se encuentra evolucionando.

Grupo médico seleccionado.

Diagnóstico ingresado por el médico.

Tiene que contar con un campo que permita nulo con ID de turno

Texto adicional (texto libre limite en 100 caracteres) (Hablado con FORCADA)

Manejamos urgente???

 

Diagnósticos
El Diagnóstico es la conclusión a la que arriba el médico al cerrar la evolución del paciente. Este pasa por 2 estados:

Diagnóstico Presuntivo: Indicado por el médico al cerrar una evolución de un paciente en base al análisis realizado.

Diagnóstico Confirmado: Confirmación de un diagnóstico presuntivo en base a preguntas que el sistema le va a hacer al médico teniendo en cuenta los diagnósticos presuntivos usados para el paciente en cuestión.

El listado de diagnósticos lo tomamos desde SNOMED CT. Esta tipificación cuenta con una estructura jerárquica de elementos desde su definición más amplia hasta lo más especifico, permitiendo hacer asociaciones con regiones de hallazgos o condiciones generales.

Tutorial: Introducción a SNOMED CT (en Español) 

04/05/2023: Por el momento vamos a seguir usando la tabla de diagnósticos que usamos en la HC actual para acelerar el release del módulo.

Circuito de rechazo o confirmación de diagnósticos: Cada diagnóstico utilizado en base a evoluciones de un paciente tienen que tener un estado para que el profesional tenga la forma de poder confirmar o rechazar un diagnóstico. De ser confirmado, el mismo pasa como una “Enfermedad o diagnóstico” confirmada del paciente.

Necesitamos que estos diagnósticos manejen una categoría y un estado (mencionado en el párrafo anterior). Primero tenemos que poder distinguir si el diagnóstico es temporal o crónico. De esta forma, mantenemos bien actualizado el listado de condiciones del paciente. De ser temporal, necesitamos definir una lógica de mantenimiento para que se pueda dar de baja cuando el paciente superó la condición temporal. Para aquellos diagnósticos que son permanentes. En conjunto con el estado, esto permite al médico confirmar o rechazar un diagnóstico presuntivo, apareciendo dentro de sus enfermedades o antecedentes si se lo confirma. También tiene que tener el médico la posibilidad de indicar un diagnóstico presuntivo diferente en la próxima evolución, dando una traza de todo lo ocurrido con respecto a la atención de ese paciente.

El problema con esto es que hoy se usa el campo de diagnóstico presuntivo para indicar que la consulta es para un control (ej. “Control clínico”) y aparte dejamos agregar elementos tabulados para aprobación (por auditoría médica) con lo cual podría confirmarse un diagnóstico creado a mano por un médico, que no se encuentre aprobado. 

Pantallas:
Evolución Actual

Añadimos el componente para que el profesional pueda seleccionar un diagnóstico en la evolución que se encuentre realizando. Este dato es obligatorio para poder guardar la evolución. De no estar completo, se debe indicar que por favor lo llene.

 

Medicamentos o indicaciones farmacológicas
Los medicamentos se asignan a los pacientes mediante las evoluciones de los médicos. Esto se hace desde la pantalla de evolución actual, levantando un modal donde se busca un medicamento nuevo o edita uno ya asignado de manera previa. 

El listado de medicamentos lo tomamos desde VIDAL Vademecum (a definir si es mediante archivo subido o API). 

Adicionalmente, vamos a contar con una funcionalidad para evitar contraindicaciones desde la comparación de un medicamento que se esté asignando con otros que se encuentren activos. Al contar con medicación que no puede ser tomada en paralelo, tenemos que hacer esta consulta cada vez que se esté asignando un medicamento nuevo, para identificar si el paciente tiene otros medicamentos activos y frenar desde un mensaje que indique esto. Si el paciente no cuenta con otra medicación activa, debemos consultar con el profesional para que consulte al paciente si se encuentra tomando dicho medicamento.

Pantallas:
Evolución Actual

Agregamos un componente para poder ver medicamentos activos, asignados en la consulta actual o la opción de agregar nuevos. Al seleccionar la última opción, se abre un modal que se compone de un buscador y de los medicamentos que se encuentren activos en ese momento. Aparte se van a ver medicamentos que el paciente ya no está tomando.

Escribiendo en el buscador, va a ver las opciones de medicamentos disponibles según el Vademecum utilizado en sus diferentes presentaciones

 

Tablas usadas:
Parametrización:
Configuración de médicos o equipos y agendas (.net)
Configuración de Textos (.net)
Derechos:
 

Circuito de uso por usuario:
Se puede ingresar a la historia clínica desde 2 lugares distintos:

Desde la agenda web de médicos al ingresar a evolucionar a un paciente. Ingresando directamente a la pantalla de la evolución actual perteneciente al turno desde donde se ingresa.

Desde la URL directa, usando las credenciales correspondientes para ingresar [USUARIOS] (HC). Este ingreso debe llevar directamente a la búsqueda de pacientes.

El circuito de uso por el usuario es muy acotado para este proyecto. Los pasos son:

El profesional accede a la evolución del turno desde la agenda web y realiza la evolución completando los puntos de la misma.

Guarda la evolución y así vuelve automáticamente a la agenda para seguir con el próximo paciente.

Pantallas:
Evolución Actual
Esta pantalla es la que usa el profesional para realizar la evolución correspondiente al turno en el cual ingresa. Esta se va a guardar en [EVOLUCIONES] con el detalle del turno y del paciente (FK) al que corresponde.

Se puede ingresar desde 2 lugares:

Desde la agenda de médicos al ingresar a evolucionar un turno.

Desde la línea de tiempo al editar la evolución (siempre que dicha acción se encuentre habilitada)

En una primera etapa vamos contar con 2 campos para la evolución:

Motivo de consulta: Motivo por el cual el paciente se acerca a ver al médico. Por el momento es un campo de texto libre. A definir si queda como elemento tabulado o libre.

Evolución: Para todos los comentarios adicionales que hacen a la información que el médico vuelca en la evolución del paciente. Es un campo de texto libre con herramientas de formateo de texto.

Adicionales: (a realizar)

Diagnóstico presuntivo:

Diagnósticos confirmados:

Orden digital de Laboratorio:

Orden digital de prestaciones médicas: