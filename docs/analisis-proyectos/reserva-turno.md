---
sidebar_label: 'Reserva de turno'
---

# Reserva de turno (pago y subida de orden obligatorios)

:::info[Objetivo]
El objetivo del desarrollo solicitado es bajar el ausentismo de los pacientes aumentando el compromiso que estos tienen con sus turnos tomados así como frenar la toma de turnos masiva que hacen los pacientes para contar con turnos “de respaldo”.
:::

## Funcionalidad

El desarrollo requiere hacer un circuito adicional en la toma de turno donde el paciente tenga que cumplir con ciertas condiciones para llegar a la misma. Para esto vamos a usar el concepto de “Reserva de turno”, el cual va a durar desde que el paciente selecciona un turno hasta que se realice el pago o termine un plazo de tiempo determinado (luego de X [PARAMETRIZABLE] minutos sin contar con el 100% de las condiciones cumplidas, se da de baja la reserva del turno). Durante este plazo el turno no puede ser ofrecido a otra persona. Si el paciente cumple con los requisitos, avanza en el circuito según corresponda. Si el paciente no cumple, se deshabilita la reserva y se libera el turno. El pago lo va a hacer mediante mercadopago, el medio actual para cualquier importe que deba abonar el paciente.

El importe de reserva de turno se va a devolver al paciente cuando este se recepcione por el medio que sea (cuando el turno tiene la marca de presente). Hay que tener en cuenta que si el paciente debe abonar un importe en base a la prestación del turno, o un coseguro de la OS, tenemos que compensar con lo abonado por reserva. 

:::warning[Texto a definir]
Se tiene que crear un template HTML especifico para envios de mails informando de la devolución de un importe. Vamos a tener que agregar la variable de monto de devolución y el manejo del % que representa del total (con explicación de porqué se devolvió dicho monto)
:::

### Pago de reserva de turno

El paciente va a abonar la reserva de un turno al tomarlo desde el portal, aplicación y terminal de autogestión. Se recomienda que desde comunicación trabajen en dejar muy claro para el paciente que va a tener que cumplir con pasos obligatorios para poder terminar de tomar un turno. 

Al seleccionar un turno dentro de la oferta post búsqueda se le va a solicitar lo requerido (pago y subida de orden o únicamente el pago). Para estudios médicos se solicitará primero la orden y luego el pago. Si el paciente no cumple con algunos de los requisitos, no se le asignará el turno y volverá a la pantalla de búsqueda. Si cumple con los requisitos, avanza a la pantalla de próximos turnos donde avanzamos con el circuito.

Según tipo de turno:
- Consulta: Para las consultas se va a requerir el pago de la reserva de turno para todo paciente que tome un turno desde cualquiera de los medios mencionados.
- Estudios: Para prácticas va a ser obligatorio el pago del importe en concepto de reserva de turno y la subida de la orden médica en todos los casos. 
- Laboratorio: Para turnos de laboratorio va a ser obligatorio el pago del importe en concepto de reserva de turno y la subida de la orden médica en todos los casos.

Si el paciente toma varios turnos para distintos días, va a abonar la reserva de cada uno de estos. Sin importar la composición de los turnos tomados (consultas, estudios, laboratorio)

Si la asignación del turno se hace desde el sistema de turnos (ya sea desde la recepción como desde la central) se crea la reserva. El paciente va a contar con un plazo (parametrizable en horas) en el cual va a tener que cumplir con los pasos obligatorios para la confirmación de toma de turno. Una vez que el paciente cumpla con esto, se levanta la reserva sobre el turno y este queda asignado. Si no se cumplen con todos los requisitos en el tiempo designado, se da de baja la reserva y queda el turno sin paciente asignado para ser ofrecido nuevamente.

:::warning[Texto a definir]
Refuerzo la idea de que la comunicación es CLAVE. Esto va a requerir cambios en la asignación de turnos desde el sistema de turnos y NO VAMOS a contar con excepciones que permitan asignar un turno sin antes pasar por el esquema de reseva y sin cumplir con los requisitos establecidos.
:::

El problema con lo anterior es que vamos a tener que manejar 2 esquemas de tiempos distintos dependiendo de la fuente de asignación de turno (portal o sistema de turnos). Esto es así porque desde el portal tenemos que contar con un tiempo coherente para verificar si el paciente cumple o no con los requisitos obligatorios y cancelar la reserva de turno cuando no lo haga en un plazo de tiempo breve (no podemos dejar una reserva de 4hs para todo el mundo sino quedan los turnos congelados por tiempos largos donde quizás el paciente ya salió y se fue del portal. No ofreciendo dichos turnos a otros pacientes).

Para poder manejar 2 circuitos de tiempo, necesitamos poder detectar el dato de quien es el usuario (operador) que está en la asignación de turno, sin llegar a asignar el turno, en la tabla de reservas (donde vamos a manejar de manera temporal una pseudo asignación del turno sin que se asigne realmente).

El sistema va a contar con una opción para poder avanzar en la asignación de un turno sin tener que hacer la reserva ni subir la orden de manera obligatoria mediante el uso de un perfil adicional. Para esto tenemos que generar un derecho adicional para ser contemplado en la toma de turnos.

### Fuentes de asignación de turno:

- Portal de pacientes: Todas las reglas de requisitos para toma de turno se aplican.
- App: Todas las reglas de requisitos para toma de turno se aplican.
- Netapp: Al asignar un turno desde el sistema de turnos se mantiene la reserva por un periodo de horas parametrizable desde el sistema. Si el paciente no abona, se da de baja la reserva y el paciente pierde el turno.
- Netapp desde Central: Al asignar un turno desde el sistema de turnos se mantiene la reserva por un periodo de horas parametrizable desde el sistema. Si el paciente no abona, se da de baja la reserva y el paciente pierde el turno.
- Portal Lite: No se aplican reglas de requisito para la asignación de un turno del lado del profesional. No podemos pedirle esto al médico porque no puede cobrar al paciente o hacer el pago online desde su consultorio.

### Definición de valor de reserva

El importe se va a definir en un cmapo nuevo dentro de un apartado “Reserva de turno” debajo de “Archivo”. Solo usuarios con derecho lo podrán definir o cambiar cuando sea necesario. Tenemos que agregar 2 campos:
- Obras sociales con marca de “Abonan”
- Resto de obras sociales.

### Devolución

El importe de reserva se va a devolver al paciente de manera automática si el mismo realiza la cancelación de un turno con anticipación. El importe a devolver va a depender de la cantidad de horas de anticipación en la que el paciente cancela el turno. Tenemos que crear parametrización para que se pueda definir el monto a devolver en forma de porcentaje. Los porcentajes indicados más adelante son según lo solicitado pero se podrán modificar.
- Si el paciente cancela un turno 72hs antes de su hora de atención se le devuelve un 100% de lo abonado en concepto de reserva de turno.
- Si el paciente cancela un turno 48hs antes de su hora de atención se le devuelve un 50% de lo abonado en concepto de reserva de turno (tenemos que revisar como aplicar devoluciones parciales sin cancelar la factura generada por la reserva)
- Si el paciente cancela un turno 24hs antes de su hora de atención o se ausenta sin previo aviso, no se le devuelve nada de lo abonado en concepto de reserva de turno.

Aparte, el importe se devuelve al paciente desde el momento en que se presenta para atenderse. Esto tiene que ser mediante un proceso automático a partir del cual se genera la devolución de la reserva del turno al obtener el turno el estado de presente. También tiene que ser posible generar una devolución desde un botón en la cuenta corriente del paciente (distinto a la nota de crédito).

Vamos a tocar la funcionalidad de nota de crédito para que no se pueda hacer sobre un movimiento de cobro de reserva de turno. Tenemos que crear un nuevo derecho para que esta acción la puedan hacer solamente los usuarios a los que se le designe dicho derecho.

:::tip[Importante]
Tiene que existir si o si un plan B por potenciales graves problemas con pacientes que lleven a la decisión de devolver importes de reserva.
:::

Si el paciente avanza en la recepción de un turno y luego se retira (sin atenderse), el dinero se le va a haber devuelto más allá de si se atendió o no. Con obtener el estado de presente en el turno, se genera la nota de crédito sobre el movimiento de la reserva de turno.

### Impacto en sistema

Para hacer impactar el movimiento de una reserva de turno, tenemos que asociar al turno e ingresar el movimiento en la tabla donde se encuentran todos los registros de cobros de la clínica a nivel histórico (como cuando cobramos desde el portal mediante mercadopago). El movimiento se va a ver desde la cuenta corriente del paciente y como un movimiento histórico asociado a la caja desde la cual se cobro. La caja a utilizar será la que ya usamos para cada centro en cuanto a los cobros que se hacen desde el portal (sectores de caja WEB).

### Reporte

A nivel de reportería vamos a contar con una estadística que muestre los registros de reserva de turno pudiendo identificar el turno y paciente que participan en cada reserva.

Vamos a mostrar cuales fueron las potenciales reservas que no llegaron a la asignación de un turno ya sea porque no cumplieron con alguno o ambos requisitos. En base al circuito planteado, con lo que deberíamos de encontrarnos es con que el paciente sube la orden pero no realiza el pago. Básicamente este reporte va a permitir ver la cantidad de casos de pacientes que intentan tomar un turno desde el portal (o app) pero no lo hacen debido a que se encuentran con un paso que no pueden cumplir.

Desde el reporte mencionado se podrían comunicar con los pacientes para ayudarlos o entender que es necesario modificar para solucionarles problemas.

### Gestión

Nos queda por revisar como va a impactar esta solicitud en el sistema de gestión. Específicamente con temas como devoluciones parciales.