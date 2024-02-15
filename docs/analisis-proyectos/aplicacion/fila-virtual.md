---
sidebar_label: 'Fila virtual'
---

# Fila virtual

:::info[Objetivo]
**Permitir a los pacientes ingresar en una fila virtual para saber aproximadamente cuando va a ser atendido desde la recepción por la aplicación y con avisos a su teléfono.**
:::


## Funcionalidad

Descripción del proyecto
La fila virtual se va a ofrecer:

- En el caso en que el paciente se recepciona desde afuera de la institución y hay más de dos pacientes presentes antes en la agenda. Alertando en la situación donde está próximo a atenderse.
- Cuando el paciente se recepcione desde un centro de la institución y exista una demora de más de 40 minutos

La fila virtual va a estar disponible solamente para quienes tengan la aplicación de la institución. Si se recepcionan desde las terminales ofrecemos la funcionalidad permitiendo descargar la aplicación e ingresando manualmente. Mostramos un QR en pantalla par que los lleve al Store de su sistema operativo. El tramite lo termina desde la terminal y este sería un último paso con un timer de 30 segundos hasta que se cierra la pantalla y la sesión. Si cuenta con más de un turno, se ofrece al final del circuito de recepción de todos los turnos.

Si desea ingresar en la fila virtual después de recepcionarse por la terminal, el paciente tiene que abrir la aplicación, ingresar a la misma e ir a ver sus turnos recepcionados. En esta instancia va a aparecer la posibilidad de ingresar a la fila virtual si se cumplen las condiciones.

Si el proceso de recepción de un turno termina en la recepción física, se le va a ofrecer al paciente ingresar a la fila virtual de la misma manera que como lo haría desde la terminal. Ingresando personalmente a la aplicación y seleccionando dicha opción (como se explica en el párrafo anterior).

### Calculo de Demora

La demora de la fila virtual se va a calcular en base a la cantidad de turnos con estado presente (no atendidos) que existan en la agenda multiplicado por el tiempo que tiene designado un turno en dicha agenda (Configuración de agenda “Minutos por turno” [ESPECIALIDADMEDICO]). Esta demora se va a mostrar en todas las agendas de atención sin turno previo y al recepcionar turnos asignados previamente si supera los 40 minutos. 

### Uso/Circuito

Se tiene que crear una cola en la que van ingresando los pacientes a medida que se vayan recepcionando, ya sea que ingresen en la fila virtual (quienes seleccionen la opción de ingresar van a tener verdadero el campo RECIBEALERTA) o no. Se da aviso a un paciente en fila cuando queda un turno con presente antes que el turno del paciente al que se avisa. Esto se tiene que hacer mediante una notificación desde la app donde se informa al paciente que se acerque al centro y consultorio del turno. Todos los pacientes van a estar encolados, con la diferencia de que aquellos que se encuentren en la fila virtual van a poder ver la demora en vivo y van a recibir la notificación mencionada indicandoles que se acerquen.

El médico debe respetar la cola a la hora de llamar a los pacientes, haciendo dicho llamado desde “Llamar próximo paciente” (en margen derecho de la agenda de médicos). Se va a dejar la opción de llamar individualmente pero se tiene que comunicar que si no se respeta el orden de llamados, no se va a poder cumplir con los pacientes que se encuentren en fila virtual. Cuando el médico llama al paciente también lo vamos a notificar de que lo están llamando.

### Ubicación e interacción con códigos QR

Tenemos que poder distinguir si el paciente se encuentra o no en DIM para poder determinar caminos de navegación desde la aplicación. La distinción de si el paciente se encuentra en DIM o no va a ser mediante la lectura de un QR o la conexión a la red de DIM. El paciente va a poder asignarse a turnos de demanda espontanea (sin turno previo) o recepcionarse en turnos previamente asignados desde ubicaciones que NO sean los centros de la institución pero no va a poder acceder a opciones de atención en recepción para otros tramites como un retiro de resultados.

También tenemos que contar con QR que se encuentren en la sala de espera para que el paciente pueda anunciarse y así saber que ya se encuentra esperando afuera del consultorio. En la agenda tenemos que agregar un icono para indicar que el paciente se encuentra en la fila virtual pero todavía no se anunció en la sala de espera. Estos pacientes tienen que estar dentro de la lógica de pacientes próximos a llamar ya que es posible que el paciente se olvide o no sepa como anunciarse, y para esto se los tiene que llamar igual. 

### Notificaciones

La primer notificación que se va a mandar al paciente que se encuentre en la fila virtual con la marca de “Recibe Alerta” va a ser en el momento en que el médico marque como atendido (ingresando a evolucionar) el turno que se encuentra 2 puestos por delante del turno sobre el cual estamos generando la notificación. Dicho de otra manera, si contamos con 3 turnos y el tercero se encuentra en fila virtual, se le va a notificar que debe acercarse a la sala de espera cuando se marque como antedido el primero de estos turnos. Dejando un margen de tiempo de un turno para posibles demoras que pueda tener el paciente si se fue a hacer otra actividad.

También se va a notificar al paciente cuando el médico lo llame desde el consultorio, permitiendo responder en base a opciones como “Estoy llegando” o “No voy a concurrir al turno”

### Particularidades

La fila virtual es para el manejo de un único turno. No vamos a tener varias filas virtuales para un paciente si este cuenta con varios turnos con demora. Si el turno con menos demora no tiene más de 45 minutos, no se ofrece la fila virtual. Si el turno con menos demora cuenta con más de 45 minutos, entonces si se va a ofrecer la fila virtual al terminar de recepcionar todos los turnos. No importa la separación de los turnos y las demoras que el resto maneje.



Determinar demora de atención para turnos de pacientes según turnos existentes en agenda tomando en cuenta los turnos con estado presente (Presente = 1) y no atendido (Atendido = 0) y multiplicando por la cantidad de minutos asignada a cada turno desde la configuración de la agenda.

Armar una tabla para fila virtual donde se van a ir agregando todos los registros de pacientes que se recepcionan para todas las agendas con el fin de poder manejar el orden de llamado de los pacientes el cual se tiene que seguir por los médicos. Pacientes que no estén en la fila virtual van a estar encolados en esta tabla al igual que quienes si ingresen.

Ofrecer la posibilidad de ingresar en la fila virtual a los pacientes que cuentan con una demora de más de 40 minutos. De aceptar, se genera el registro en la tabla antes mencionada y se indica que está en fila virtual. Solamente se ofrece a quienes accedan desde la aplicación.

La fila virtual se va a ofrecer sin importar la cantidad de turnos que tenga con estado de presente, siempre que el que menor demora tenga hasta el horario de atención sea de más de 40 minutos. Si el paciente cuenta con un turno (dentro de todos los turnos del día) que tiene menos de 40 minutos de demora, no se ofrece la fila virtual. DEFINIR SI APARTE DE LO MENCIONADO, SE TIENE QUE EVALUAR LA DIFERENCIA ENTRE LA HORA DEL TURNO Y LA HORA ACTUAL. EJ. SI EL TURNO CON MENOS DEMORA TIENE 20 MINUTOS, PERO ESTÁ A 1 HORA Y MEDIA DE LA HORA ACTUAL, SE OFRECE O NO SE OFRECE? SE RESPETA EL HORARIO DE LLEGADA O SE RESPETA EL HORARIO DEL TURNO? NO SE PUEDEN MEZCLAR AMBOS CONCEPTOS

Al paciente que se recepciona desde la terminal o termina el proceso en recepción se le ofrece la fila virtual pero depende de este bajarse la aplicación e ingresar. Para esto, cuando el paciente tiene turnos con presente, vamos a mostrar un botón más en el menú el cual indique “Turnos recepcionados” donde el mismo puede ver información de estos turnos, entrar a fila virtual y anunciarse.

Acciones realizables desde cualquier lado:

Toma de turno de demanda espontanea o atención sin turno previo con ingreso en fila virtual.

Recepción de turno previamente tomado cuando la diferencia no es de más de 3 horas contra el turno. Sin posibilidad de pagar en efectivo o hacer validación online. Tampoco se va a poder para las obras sociales que indican monto a abonar en la orden/autorización (validación interna)

Acciones realizables unicamente desde la institución:

Solicitar atención para recibir informe de estudios (ofreciendo previamente verlo online mostrando directamente los informes y sus estados)

Solicitar atención para recibir informe de resultados de laboratorio (ofreciendo previamente verlo online mostrando directamente los informes y sus estados).

Solicitar atención para turnos de chequeos médicos.

Para saber si el paciente se encuentra en la institución:

Identifiacar por GPS (siempre que el dispositivo lo tenga activado) la ubicación.

Mediante generación de QR que representen el ID de centro para que el paciente lo lea. Definir donde los van a ubicar.

Ofrecer al paciente la opción de anunciarse, para el turno en el que se encuentra haciendo la fila virtual (relacionando registro en tabla por ID de turno). Se anunca mediante la lectura de un código QR.

Enviar notificación al paciente cuando es llamado por el médico si este no está anunciado. Preguntar si se encuentra para ser atendido y en caso de que no sea así, ofrecer opciones tabuladas como respuesta armada hacía el médico.

### Cambios en agenda:

Identificar visualmente quien se encuentra en fila virtual y quien está anunciado.

Visualizar mensajes que el paciente envíe como respuesta a un llamado por parte del médico en un modal en margen inferior izquierdo. Estos los tiene que cerrar el médico.

Enviar a los pacientes a colas individuales por puesto de atención según lo que el paciente tenga que hacer presencialmente en recepción.

Para lo anterior: Ampliación de talba [PUESTOS] para poder identificar mediante 2 nuevas columnas (TIPOATENCION y PRIORIDAD) que tipo de atención se va a realizar en ese puesto. Ejemplo: “PCDIM-0725 Puesto 1 -LABORATORIO - 1” para identificar que en el puesto 1 de recepción se va a atender laboratorio de pacientes que tengan mayor prioridad. DEFINIR PRIORIDADES SEGUN ESTADOS DE TURNO Y LOGICAS ADICIONALES QUE SE QUIERAN AGREGAR PARA MEJORAR ESTE PROCESO DE ATENCION.

### Cambios en agenda de médicos

Tenemos que modificar la agenda de médicos para poder informar al médico si un paciente se recepcionó pero es posible que no se encuentre en la sala de espera aguardando ser atendido. La comunicación para con el paciente y el médico una vez que el paciente se encuentre en la fila va a ser de la siguiente manera:

### Tablas necesarias

FILAVIRTUAL (Temporal): Guarda información del turno, paciente para indicar que está en fila virtual.

| FILAVIRTUAL    | Descripción |
| ----------- | ----------- |
| ID      | Identificación numérico del registro |
| IDTURNO   | ID del turno del paciente que se encuentra en la fila |
| IDPACIENTE   | ID del paciente del turno que se encuentra en la fila |
| FECHA   | Fecha en la que se ingreso en la fila |
| HORA   | Hora en que ingreso a la fila de espera |
| ANUNCIADO   | Boolean para poder identificar si el paciente se encuentra en sala de espera o no |
| ULTIMOLLAMADO   | Ultima vez que se llamo al paciente desde la agenda |
| DEMORAINICIAL   | Demora que se le indica al paciente al momento de entrar a la fila |
| RESPUESTA   | Mensaje que indica el paciente como respuesta al llamado de un médico |


 