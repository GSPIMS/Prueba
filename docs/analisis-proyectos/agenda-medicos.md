---
sidebar_label: 'Agenda de Médicos'
---

# Agenda de médicos (web)

La agenda de médicos es la herramienta que estos usan para iniciar el proceso de atención de pacientes. En esta pueden ver todos los turnos asignados ordenados por día y hora. Se usa por todos los profesionales que hacen atención al publico, ya sea a través de consultas (evoluciones) o de estudios médicos (realización de informe médico).

## Parametrización:

La parametrización que afecta al módulo de agenda se maneja desde "Médicos o Equipos", ya sea desde la configuración de las agendas que estos registros tienen asignadas, o desde el registro del médico o equipo en si. Aparte influye en el funcionamiento de las agendas la configuración de textos, usuarios y config.json o archivo de configuración del proyecto en si.

### Configuración de médicos o equipos y agendas (.net)

- Dentro del módulo de Médicos o Equipos se encuentra la parametrización básica que afecta al funcionamiento de agendas web. En primera instancia un registro de médico debe estar asociado a un usuario para poder armar la relación entre ambos y así ofrecer al usuario las agendas que pertenecen al grupo con el cual se armo dicha relación. Esto se maneja desde el botón "Usuarios Asignados" (en la pantalla mencionada). Ademas de armar la relación descrita, se va a indicar si el usuario va a poder realizar evoluciones o no sobre las agendas del médico. Para lograr esto se tiene que tildar la opción de “Evoluciona” que se encuentra en la pantalla que se abre al ingresar a Usuarios Asignados.
- Otra configuración importante que hace a poder determinar si la agenda va a ser de consultas o de prácticas son las marcas que se encuentran en la segunda hoja de la configuración de agendas. En esta se van a encontrar marcas como:
    - Solo permite cargar Consultas: Agenda que solamente adminte una consulta como prestación del turno.
    - Solo permite cargar Prácticas: Agenda que solamente adminte una práctica como prestación del turno.
    - No carga automáticamente consulta médica: Por defecto, en todo turno que se asigne en una agenda se carga una prestación de consulta (código 42.01.01) para ahorrar tiempo debido a la gran cantidad de agendas de consulta que se manejan. Con esta opción se quita dicha funcionalidad y el turno queda vacío.
    - No evoluciona: Para indicar en aquellos casos donde no se tiene que realizar la evolución de una consulta.
    - No evoluciona prácticas: Para indicar en aquellos casos donde no se tiene que realizar la evolución de una práctica.

### Determinación de circuito a seguir (Historía clínica o informe)

Actualmente contamos con 3 circuitos de trabajo para la agenda de médicos web. El profesional puede ingresar a la historia clínica de un paciente y generar una o varias evoluciones, realizar un informe desde una prestación realizada o simplemente marcar el turno como atendido (sin una posterior acción). Para todos los circuitos el profesional siempre va a llamar al paciente desde la agenda, sin embargo va a cambiar lo que va a ver en pantalla una vez que llame:
- Si la agenda evoluciona, el profesional va a ver un modal (o ventana) donde va a poder ver datos del paciente, si tiene otros turnos en el día, seguimientos generados en atenciones previas y la evolución previa de existir. Aparte, va a poder llamar nuevamente al paciente, cerrar dicho modal o ingresar a la historia clínica del paciente con el botón "Evolucionar". Así es como se asigna la marca de "Atendido" al turno y se ingresa en la evolución del paciente en una nueva pestana.
- Si la agenda se encuentra configurada para informar, el modal va a mostrar al profesional información del paciente, si tiene otros turnos en el día y la orden del estudio médico a realizar. En cuanto a las posibles acciones va a poder cerrar el modal, llamar nuevamente o (a diferencia del caso anterior) ingresar a la pantala de realización de informes mediante el botón "Informar". Así es como se asigna la marca de "Atendido" al turno y se ingresa a la pantalla de realización de informes en una nueva pestana.
- La agenda puede estar configurada para que el profesional que atiende no tenga que evolucionar ni informar. En este caso, el modal que aparece cuenta con opciones para cerrarlo, volver a llamar o marcar como atendido. Esto solamente genera la marca de atendido en el turno.

Para poder determinar cual de los circuitos mencionados va a seguir la agenda se tiene que trabajar en los parámetros que hacen a la configuración de las mismas:

***La agenda va a permitir y solicitar evolucionar si:***
- Se indica que "Solo permite cargar consultas" y no se marca "No evoluciona"
- Se indica que "Solo permite cargar prácticas" y no se marca "No evoluciona prácticas"
- No se indica "Solo permite cargar consultas" ni se indica "Solo permite cargar prácticas" y no se marca "No evoluciona"

:***La agenda va a pemitir y solicitar informe si::***
- Se indica que "Solo permite cargar prácticas" y se indica "No evoluciona prácticas"

:***La agenda va a permitir y solicitar marcar el estado de turno como atendido si::***
- Se indica "Solo permite cargar consultas" y se marca "No evoluciona"

:::danger[Falta de marca para no informar ni evolucionar prácticas]
Faltaría una marca para indicar que no se informa. Así se podría indicar que la agenda solo permite cargar prácticas, que no se evolucionan ni se informan.
:::

### Liquidación de médicos (VB6)

El único parámetro a tener en cuenta en cuanto a lo que hace a la liquidación del médico es el mensaje programado que va a aparecer en la agenda indicando al médico lo que debe facturar a la clínica. Este mensaje se setea desde el sistema en VB6 y se usa el importe final de las liquidaciones de los médicos para completar la variable en el mensaje mencionado.

### Configuración de Textos (.net)

Todos los textos que ve el médico en la agenda web se configuran desde el sistema de turnos desde la opción de Textos Config.

### Agendas compartidas

Es posible permitir a distintos médicos acceder a evolucionar el turno de una misma agenda. Para esto se usa la opción de “Agendas Compartidas” a la cual se puede acceder desde la configuración de la agenda. Se deben identificar los médicos que pueden evolucionar en dicha agenda mediante su matrícula provincial.

:::info[Registros de agendas compartidas]
Estos registros se crean en en la tabla AGENDASRESIDENTES completando los campos IDMEDICO e IDESPECIALIDAD (de la agenda) y la matrícula indicada.
:::

Al querer evolucionar un turno desde la agenda web se revisa si para dicha agenda se cuenta con registros creados en la tabla mencionada. De ser así se compara la matrícula del médico que está queriendo evolucionar con los registros existentes. Si se encuentra en el listado se solicita la contraseña del usuario del médico en historia clínica (tener en cuenta que la contraseña es la del usuario que está queriendo ingresar a evolucionar. No la del usuario médico "dueño" de la agenda)

## Derechos:

Los derechos que afectan al sistema de agenda de médicos son:
- 1.2: Permite ver las agendas que se encuentran relacionadas al médico.
- 1.8: Cambiar el estado del campo atendido de los turnos a verdadero.
- 11.1: Ingresar a la Historia Clínica usando la clave del usuario creado en la base de HC.

## Circuito de uso por usuario:

Para que el médico pueda atender pacientes para evolucionar o realizar informes médicos, la agenda debe estar abierta desde el sistema de turnos [AGENDASACTIVAS]. Caso contrario no se debe dejar avanzar en dichos procesos (parametrizable).

### Consulta médica (Evolución):

El médico que evoluciona va a usar la agenda para llamar a los pacientes e ingresar a la historia clínica de cada uno de estos para realizar la evolución de la consulta médica. Para esto los pasos son:

1. Ingreso en agenda mediante usuario y clave del profesional.
2. Tocar botón para llamar al paciente desde la tabla o CARD de próximo paciente (siempre y cuando esté parametrizado que se usa el llamador)
3. Con el paciente en el consultorio, se ingresa a la Historia Clínica mediante botón de Evolucionar. Es posible usar el botón evolucionar directamente desde la tabla pero se recomienda usar el llamador ya que sino no se da aviso al paciente que se encuentra en sala de espera.
4. Ingreso de clave de usuario del médico en base de Historia Clínica (recordar que el usuario de historia clínica es distinto al usuario de agenda).

### Informe de estudio médico

1. Ingreso en agenda mediante usuario.
2. Tocar botón para llamar al paciente desde la tabla o CARD de próximo paciente (siempre y cuando esté parametrizado que se usa el llamdor)
3. Con el paciente en el consultorio, se ingresa a la pantalla de realización de informes mediante el botón Informar.

## Pantallas:

### Log In:

Ingreso al sistema de agendas con usuarios del sistema de turnos. Dependiendo de los derechos que tiene el usuario, se va a poder ingresar o no.

### Selección de agenda

Drop down con todas las agendas a la que puede acceder el médico. Al seleccionar una de estas, ingresa en dicha agenda. Si el médico cuenta únicamente con una agenda, se ingresa a la misma directamente. Para los usuarios que cuentan con derecho de Administrador, se van a mostrar todas las agendas activas. 

### Publicidad 

Antes de ingresar a la agenda, se va a mostrar en toda la pantalla la publicidad que se encuentre subida en la ruta según corresponda. Si no existe ningún archivo PNG en dicho directorio, se saltea este paso. Para poder avanzar y cerrar la publicidad, se tiene que hacer clic sobre el botón "Cerrar"

### Aviso de facturación

Si el profesional cuanta con importes liquidados y autorizados para que este emita una factura, se va a mostrar una tabla con los mensajes de dichas facturas a realizar. Puede indicar que ya fue notificado para que este mensaje no aparezca más. Si no tiene mensajes de facturas a emitir para leer, se saltea este paso.

### Agenda del día

Lugar donde el médico ve todos los turnos del día, pudiendo navegar hacia atrás o hacia adelante en el tiempo para ver turnos pasados o futuros. Desde aquí va a trabajar ya sea evolucionando al paciente o informando las prestaciones que se realicen.

En la tabla principal se van a ver todos los turnos asignados, mostrando el horario del mismo, el paciente al que pertenece el turno y el estado en que se encuentre según se encuentre [MAESTRO_TURNOS]

- Asignado: El turno tiene valor falso en PRESENTE [MAESTRO_TURNOS] y en ATENDIDO [MAESTRO_TURNOS]. Estos turnos no van a mostrar un estado en forma de texto.
- Presente: El turno tiene valor verdadero en PRESENTE pero falso en ATENDIDO. Estos turnos van a mostrar un estado Presente que se va a mostrar en color verde (#3D994A)
- Atendido: El valor tiene valor verdadero en ATENDIDO. Bajo las correctas circunstancias, el turno cuenta también con valor verdadero en PRESENTE. Estos turnos van a mostrar un estado Atendido que se va a mostrar en color azul (#006299)

Todos los turnos que sean un ***<font color="#FF8533" strong> sobreturno </font>*** van a tener la hora y el nombre del paciente en color naranja (#FF8533) para que el usuario pueda reconocerlo fácilmente. Estos no van a afectar el color del estado del turno.

Por encima de la tabla contamos con un filtro para manejar los estados antes mencionados y un calendario para poder navegar a una fecha de atención pasada o futura (o presente), mostrando en la tabla los turnos pertenecientes (por fecha de turno) al de la fecha seleccionada. Solo dejamos seleccionar fechas que coincidan con días generados de atención del profesional [ESPECIALIDADMEDICO_DIAS] (si no existe el registro en la tabla de "díás generados" en configuración de agenda, no se va a poder seleccionar el día en el calendario).

La información de la tabla se tiene que actualizar automáticamente cada 120 segundos (parametrizable en configuración del web service) y esto lo vamos a reflejar con un texto e icono animado que muestre cuanto tiempo queda hasta la próxima actualización automática. 

#### Acciones de tabla:

La tabla cuenta con varias acciones las cuales van a interactuar siempre con el turno individual sobre el cual se está iniciando la acción. Algunas de estas son informativas (mediante modal) y otras son operativas y hacen al trabajo del profesional. Estas son:
- Notas del turno: Muestra la nota que se haya agregado al turno [MAESTRO_TURNOS]{} (si existe) en un modal. Está va a aparecer como texto fijo, sin poder editarlo. Tiene que mostrar apellido y nombre del usuario que agrego la nota al turno
- Información del turno:
- Llamar: Botón para llamar al paciente que pertenece al turno sobre el cual se esta disparando esta acción. Se tiene que enviar al llamador información del turno para realizar el llamado en si. Por otro lado, se abre el modal que se utiliza para cuando se llama a un paciente, el cual va a permitir volver a llamar o entrar a evolucionar. Este se puede cerrar con la cruz sobre el margen superior derecho.Tenemos que ampliar esta funcionalidad para que el modal muestre más datos respecto al paciente. Estos datos adicionales son:
    - Turnos del día que tengan estado de presente y/o atendido.
    - Última evolución realizada por el mismo profesional. De no haber, no se muestra nada.
    - Seguimientos activos creados por el médico.
- Evolucionar: El botón evolucionar va a llevar al usuario a la historia clínica directamente a la evolución actual del turno que se está evolucionando. 
- Información Paciente: Este modal muestra información del paciente que se encuentra asociado al turno. Tenemos que mostrar apellido y nombre, DNI, fecha de nacimiento, Obra Social, Plan y Número de afiliado.
- Imágenes del paciente: Abre un modal que lista toda slas imágenes del paciente que se encuentren en el PACS listadas en registros (botones) donde se pueden abrir tocando en cualquier lugar del mismo. Esto tiene que abrir el visualizador que haya seleccionado el profesional para la apertura de imágenes. Dejar por defecto el Orthanc Stone.
- Ver Prestaciones/Orden: En este modal se van a listar todas las prestaciones que se encuentren asignadas al turno (códigos de nomenclador) [MAESTRO_PRACTICAS] en una tabla. Por debajo de esto se va a mostrar la orden médica del turno siempre que se encuentre un documento asociado al turno catalogado como “Orden médica”
- Dar turno: Esta acción va a llevar al  [Portal Lite](/docs/analisis-proyectos/portal-lite.md) donde el profesional puede buscar y asignar turnos en su agenda o en otras agendas de prácticas al paciente. 

A la derecha de la tabla tenemos un CARD grande con información de la especialidad de la agenda y del consultorio en el que se encuentra abierta. En este mismo CARD se va a mostrar el próximo paciente a llamar teniendo en cuenta los turnos que tienen marca de presente y no de atendido. Va a aparecer el paciente que tenga el estado presente y sea el próximo a ser atendido en orden cronologico de más antiguo a más reciente, respecto a la hora del turno (no la hora del presente).

#### Cartilla de médicos

Este botón se encuentra en el menú haburguesa en el margen superior derecho o también está accesible como botón flotante en el margen inferior derecho de la pantalla. Al acceder, se abre una nueva pestaña en el explorador para mostrar todos los médicos y prestaciones que se hacen en la institución. Identificando si está atendiendo o no en el momento en que se ingresa a la pestaña. En esta se pueden ver los horarios de atención de cada agenda que figura en la tabla.

#### Internos

Este botón se encuentra en el menú haburguesa en el margen superior derecho o también está accesible como botón flotante en el margen inferior derecho de la pantalla. Trae un modal con un listado de teléfonos o internos para las necesidades de comunicación interna del médico. 

:::info[Edición de internos]
Los registros que aparecen como números internos se pueden crear, modificar o eliminar desde "ARCHIVOS>TEXTOSCONFIGURACION" en .net.
:::
