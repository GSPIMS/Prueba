---
sidebar_label: 'Asistencia'
---

# Asistencia en orden subida desde portal

Puntos tomados de análisis de modificaciones al portal en confluence:
- Permitir al paciente solicitar asistencia sobre su orden médica con un nuevo circuito para que pueda subir la misma. Si está desde Mobile tenemos que permitir adjuntar o tomar foto. Si se encuentra en desktop solamente ofrecemos adjuntar la orden.
- Agregar una tabla a la base de datos que guarde información de la solicitud de asistencia, usando la fecha de subida y el archivo de imagen adjunto, fechacontacto, usuariocontacto, mensajecontacto y un estado resuelto booleano, fechabaja e IDdeturnodestino.
- Crear una nueva grilla donde listemos las solicitudes de asistencia, mostrando información del paciente y una acción para levantar la imagen subida. Las columnas de la tabla son: Fecha de subida, nombre, dni, teléfono verificado, teléfono móvil, email, obra social, plan, Fecha contacto, Usuario contacto y resuelto. Agregar filtro de fecha y otro para ver lo resuelto o sin resolver. Por defecto se abre en la fecha del día con los registros sin resolver.
    Tenemos que tener 3 acciones con diferentes circuitos:
    - Resuelto: esto tiene que marcar el registro como resuelto.
    - Asignar turno: Lleva al portal LITE con el paciente cargado para la búsqueda del turno.
    - Indicar contacto: Se guarda la fecha y usuario de quien lo marca como que se contacto y solicitar un comentario.
- Verificar dato de teléfono y mail con el paciente cuando sube la orden. Se muestra un modal con estos datos para que el paciente indique si son correctos o no. 

:::info[Objetivo]
**Saltear un problema actual por el cual los pacientes NO toman turnos desde el portal para estudios, debido al miedo a seleccionar una prestación incorrecta.**
:::

### Funcionalidad

Adicionalmente a lo ya desarrollado (la interacción del paciente con el portal para subir la orden, saí como la tabla para controlar lo subido) se solicita automatizar el proceso de toma de turno sin que tenga que interactuar un operador/representante de DIM. Un operador va a controlar la orden subida por el paciente e indicar la definición de prestación que corresponde para la toma de turno. Confirma la selección mediante un botón “Confirmar envío“. Esto dispara un mensaje al paciente para que esté, desde el portal, tome un turno dentro de los ofrecidos.

Para poder cumplir con lo solicitado necesitamos:
- Permitir buscar y asociar una definición de prestación a un registro de asistencia para que al momento de crear el link para el paciente, se mande dicha prestación dentro de lo que hace a la búsqueda de turnos (link de ejemplo: https://wsportal2023.dim.com.ar/api/v1/estudios/verificarOS/127/false/false// El 127 es el ID de la prestación). Para esto tenemos que modificar la pantalla con la que se trabaja cada una de las ordenes que suben los pacientes.
El operador tiene que poder seleccionar varias prestaciones ya que en algunos casos de determinados circuitos la búsqueda permite multi selección.
- Creación del Link y envío del mismo por whatsapp al paciente. Para esto vamos a tener que modificar el desarrollo realizado pasra no permitir avanzar si el paciente no cuenta con un teléfono móvil cargado (campo boligatorio). Este paso de revisión lo hacemos antes de que pueda subir la orden. Si no tiene dicho dato y no lo carga, no se permite avanzar.
El link tenemos que armarlo con la definición de la prestación seleccionada y con un token de seguridad para los datos personales que van a permitir el ingreso.
Para el envío del whatsap tenemos que revisar la funcionalidad de integración con Botmaker como que se usa actualmente en mensajería de confirmaciónes. 
- En Botmaker se va a tener que armar el template de salida del mensaje principal así como el manejo de la conversación que pueda derivar por respuestas del paciente.
- Asociar la orden subida al turno tomado por el paciente. Actualmente guardamos el dato del ID de turno cuando el operador lo relaciona a un registro. Para esto, necesitamos usar la información del ID del registro de asistencia para que al momento de tomar un turno, se cargue la orden subida previamente al turno asignado. 

### Circuito de subida de orden

Desde el portal el paciente accede a la opción para subir la o las órdenes que corresponden a lo que le fue solicitado. Para esto toca el botón “Subir Orden” que se encuentra en el card de búsqueda de definiciones de prestaciones en Estudios.

El circuito de subida es mediante los mismos modales que usamos para cuando un paciente toma un turno y se le pide la orden. Una vez subido el o los archivos, llega a una pantalla de confirmación donde se le indica que el proceso fue exitoso y un representante de DIM lo va a controlar.

:::warning[Texto a definir]
Definir textos de template de whatsapp así como los mensajes posteriores en caso de que el paciente inicie una conversación.
:::

### Circuito de revisión de órdenes

Circuito de revisión de órdenes
Un representante de DIM va a trabajar con una nueva pantalla a desarrollar donde tenga el listado de ordenes a controlar en una tabla en el lado izquierdo y datos del paciente, la orden y el buscador de prestaciones del lado derecho. 

Al seleccionar un registro de la tabla, se van a cargar los datos para esa solicitud de asistencia en el resto de la pantalla. El usuario debe revisar la orden y cargar las prestaciones que sean necesarias para la futura asignación del turno buscando cada una de estas y seleccionandolas con el mouse o tecla “Enter” del teclado. Así se agrega la o las prestaciones y se confirma el envío de la notifiación al paciente. Esto va a encolar el envío del mensaje por whatsapp al paciente con un delay de 15 minutos (para posibles correcciones) y avanzar al siguiente registro en la tabla para resolver la próxima solicitud de asistencia. Este proceso de repite hasta no contar con nuevas solicitudes.

### Circuito de asignación de turno

Una vez que se resuelve una solicitud de asistencia de un paciente, se le va a enviar una notificación al número que se encuentra cargado en su campo de teléfono móvil por el canal de whatsapp. Esto se hace mediante integración con Botmaker (herramienta de Bot Externa) utilizando un template que se tiene que cargar en dicha plataforma (requiere aprobación de Facebook). Este mensaje va a contener un link que se debe armar cuando se indica resuelta la solicitud y va a contener información del paciente para el ingreso al portal (DNI y clave) y el registro de solicitud de asistencia. 