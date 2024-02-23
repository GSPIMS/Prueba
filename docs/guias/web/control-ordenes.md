---
sidebar_label: 'Control de órdenes'
---

# Control de órdenes web

En este módulo se controlan todas las ordenes que adjunte el paciente a un turno mediante cualquiera de los medios disponibles para hacerlo:
- Portal de pacientes: El paciente sube la orden al momento de tomar un turno para que sea controlada por un representante de DIM y así ahorrar tiempo en la recepción de dicho turno.
- Terminal de autogestión: En casos donde el paciente no pudo subir la orden de antemano, o se acerca a atención sin turno previo, este puede subir la orden desde la terminal. Este control tiene que ser inmediato ya que el paciente se encuentra esperando en una terminal de autogestión en el centro.
- Aplicación en auto check in: Al igual que con la terminal, es posible subir la orden el día del turno desde la aplicación.

## Circuito de control

Al ingresar al control de órdenes se tiene que identificar si se va a trabajar con el listado de pacientes que suben la orden desde el portal o con aquellos que requieren más inmediatez ya que provienen de terminales o desde la aplicación.

IMAGEN DE TABLA

:::info[Estados para el control]
Los estados que se usan para el control de órdenes son:
- A controlar: Todos los turnos que están para ser controlados. 
- Rechazados: Turnos que fueron rechazados desde su control al responder las preguntas básicas al iniciar el circuito.
- Validado: Turnos que se controlaron y se validaron.
- Modificado: REVISAR EL NOMBRE Son los turnos que se rechazaron por un problema con la órden y el paciente volvió a subir otro documento.

Por defecto, la tabla va a mostrar los turnos en estado "A controlar" y "Modificado"
:::

Identificado el origen de los turnos, se van a listar en la tabla todos los que se encuentren para ser controlados. Es importante tener en cuenta que lo que se va a ver en la tabla depende de los filtros que se encuentran en cada una de las columnas de la tabla o la fecha por encima de la misma. Para habilitar el filtrado por columnas, se tiene que tocar el botón de filtros (embudo) que se encuentra en el margen superior derecho de la tabla. Los principales filtros a usar son:
- Fecha: Es la fecha del turno. A medida que se van controlando los registros, se puede ir seleccionando el siguiente día, para traer estos registros y seguir controlando. Se usa la fecha del turno para poder armar un circuito de trabajo donde se controlen a tiempo los turnos de los próximos días.
- Autoterminal: Un checkbox que se encuentra arriba de la tabla. Se va a usar para trabajar con el control de las órdenes que se suben desde la terminal o desde la aplicación.
- Estado: Al usar el filtro en la columna "Estado" podemos definir si queremos visualizar registros que se encuentren rechazados o validados. Por defecto, se van a mostrar los turnos que esten para controlar. Ya sea porque el paciente subió la orden por primera vez al tomar un turno o porque volvió a subir orden sobre un turno previamente rechazado. 
Al ingresar, los turnos que se muestran son todos los que requieren de control (obviando lo que ya está validado o rechazado) para la fecha del día. De necesitar trabajar con otros registros, se usan los filtros que se mencionan más arriba.

**Para iniciar el control de un turno**, se hace clic sobre el botón que se encuentra en la columna "Control" (el que corresponda a la línea del turno que se quiere controlar). Esto va a abrir otra pestaña con informacióny documentación especifica del turno. 

IMAGEN DE CONTROL DE ORDEN

Esta pantalla es dinamica y va a cambiar a medida que uno avanza con el control. Esto es así para acompañar a quien hace el control en cuanto a los pasos que tiene cumplir para avanzar con el circuito establecido. Los pasos son:

1. Responder las preguntas básicas para poder determinar si el turno debe ser rechazado o se puede continuar con la carga de datos o el control de los mismos. En este paso el usuario puede únicamente interactuar con las preguntas, teniendo acceso visual al resto de la información (para poder responderlas). VER SI LINKEAMOS EL CIRCUITO DE PREGUNTAS. Es también posible manejar los comandos del visualizador del PDF o acceder a otros documentos del turno. El resto de los elementos van a aparecer sin color (en gris) y sin posibilidad de interacción.

2. La respuesta de las preguntas van a llevar a 2 posibles escenarios: avanzar con el control del turno o indicar el rechazo del mismo. Lo que va a cambiar en el caso de ser rechazado el turno es la notificación que se va a enviar al paciente dependiendo de en que pregunta se efectua el rechazo (con la respuesta negativa). No todas las respuestas negativas van a llevar a un rechazo ya que depende del circuito mencionado.

3. Si luego de responder las preguntas se puede avanzar con el control, estas van a desaparecer y va a ingresar en la pantalla el estado de los puntos que falten para poder hacer la validación final del turno. Estos son:
    - Documentos del turno: Al menos uno de los documentos del turno tiene que estar catalogado como "Orden médica" para poder avanzar. Ya que es la clasificación con la que el sistema entiende que se encuentra una orden adjuntada al turno. 
    - Prestaciones: Por defecto el turno tomado desde el portal va a contener la prestación (debido a que se toma desde una definición de una prestación) con lo cual no va a ser un freno para la validación del turno. Sin embargo, depende del operador modificar la prestación de ser necesario. Sí va a requerir interacción en el caso que la obra social requiera autorización. 
    - Médico solicitante: El paciente puede ingresar el médico solicitante al momento de tomar el turno. De no hacerlo, es un requisito buscar y seleccionar el mismo para el usuario de DIM. Sin médico solicitante no se puede validar el turno.
    - Centro derivador: El centro derivador es otro campo obligatorio el cual siempre va a definirse por usuario de DIM.

4. Una vez que se interactue con todas las variables mencionadas del turno, se va a poder hacer la validación final. Al validarlo se envía una notificación al paciente indicando que la orden se controló con exito y avisando que su recepción puede realizarse mediante las terminales de autogestión. Aparte va a cambiar el estado del turno en la tabla 

:::info[Estado del turno]
Tener en cuenta que el turno validado no se va a ver en la tabla ya que por defecto solo se muestran registros con los que se debe trabajar. Se tiene que modificar el filtro de la columna estado para poder acceder a estos registros
:::

ES POSIBLE REINCIIAR EL CIRCUITO DE PREGUNTAS PARA LO VALIDADO O RECHAZADO???


