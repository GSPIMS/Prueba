---
sidebar_label: 'Fila de espera'
---

# Fila de espera de pacientes

:::info[Objetivo]
**Ofrecer al paciente entrar en una fila de espera para notificarlo de cuando hay disponibilidad de turnos en base a búsquedas que no obtienen resultados o que el resultado es dentro de gran cantidad de días.**
:::

## Funcionalidad

La fila de espera es un encolado de pacientes a los cuales se van ofreciendo turnos a medida que estos se van liberando según las variables para las cuales se encuentra encolado (médico o especialidad) el paciente. Se ofrece al paciente ingresar a la fila de espera cuando el turno que encuentran y se asigna es a más de X días respecto al día de toma o asignación de turno. 

:::note[Parametrización]
Los días a partir de los cuales se empieza a ofrecer la fila de espera es un campo parametrizable en tabla de configuración de fila de espera.
:::

Se ofrecera la opción de ingresar solamente a los pacientes que hayan completado los pasos requeridos para poder avanzar. En el caso de las consultas no hay requisito previo (excepto a futuro el pago de la reserva del turno) y para las prácticas, la subida de la orden (a futuro el pago de la reserva del turno). Con lo cual la oferta en si se va a encontrar en el último paso de la toma de turno y de no cumplirse con un punto requerido, se indicará que para poder aprovechar del servicio, se tiene que cumplir con dicho paso. La única opción aparte de volver y realizar los pasos obligatorios va a ser NO aceptar la fila de espera, con lo cual se pasa al final de confirmación de toma de turno. 

Si el paciente cumple con lo requerido y acepta ingresar a la fila de espera, se genera un registro en la base de datos donde se almacena la fecha y hora de ingreso a la fila y el turno tomado, para poder contar con la información necesaria para luego ofrecer turnos.

La fila de espera se va a ofrecer según las agendas las cuales se indiquen como que manejan esta funcionalidad. Esto se va a parametrizar por sistema, junto con otros puntos que se van a ver más adelante. El paciente puede ingresar a la fila de espera para recibir turnos solamente del profesional con quien se encuentra tomando el turno, o para la especialidad a la que el profesional pertenece. (si hay turnos disponibles de otros profesionales antes los ofrecemos en este momento o suponemos que el paciente está buscando turno para el profesional especifico?)

## Parametrización

La fila de espera se va a ofrecer a los pacientes siempre y cuando se cumplan con las condiciones que hacen a los parámetros que se van a usar para su funcionamiento. Toda la parametrización que se va a manejar para poder definir el comportamiento de los aspectos que hacen a la fila de espera son:

- Días a partir de los cuales se ofrece: Es la indicación de la cantidad de días a futuro a partir de los cuales se empieza a ofrecer la fila de espera. Se va a comparar dicha cantidad con la diferencia entre el día del turno que se está tomando y el día actual. 
Ej.: Parámetro: 14 días
Paciente turno para el día 28/01/2023 siendo el día que lo toma el 10/01/2023. Como la diferencia entre ambas fechas es de 18 días, entonces se va a ofrecer la fila de espera
- Agendas que ofrecen fila de espera: Listado de agendas que potencialmente van a ofrecer al paciente ingresar a la fila de espera siempre que el resto de las condiciones se cumplan. 
- Tiempo de vencimiento de oferta: Es el tiempo en minutos a partir del cual se vence la oferta del turno al paciente al cual se le ofreció. Una vez enviada la oferta de un turno a un paciente que se encuentra en la fila de espera, si pasa el tiempo determinado en esta variable, se ofrece a otro paciente, dejando sin poder acceder al turno a quien se le oferto en primera instancia. 
- Obras Sociales prioritarias : Se solicitó un parametro para poder definir cuales son las Obras Sociales que tienen prioridad en el ofrecimiento de un turno a paciente sque se encuentran en la fila de espera. Esto implica que si un paciente se encuentra en el peusto 15 como candidato para un turno ofrecido pero es el primero que tiene una OS prioritaria, se va a ofrecer el turno. Para esto recomendamos no manejar un parámetro nuevo sino usar la identificación actual por Obra Social donde indica personal de facturación si la misma es “Normal” o de prioridad “Nivel 1”, “Nivel 2”, o “Nivel 3”.

## Circuito para ingreso en fila

La fila de espera se va a ofrecer siempre que un paciente tome un turno desde el portal/app/terminal y la diferencia de días entre la fecha del mismo y la fecha actual sea superior a la cantidad de días a futuro que se maneje por el parametro antes mencionado. Otro parámetro a revisar desde la lógica de funcionamiento de fila de espera es si la agenda sobre la cual el paciente está tomando el turno ofrece fila de espera o no (la fila no es global con lo cual no se va a ofrecer siempre).

El ofrecimiento de la fila de espera se va a realizar como el último paso antes del modal de confirmación de turno tomado. Si el paciente no sube la orden en el caso de una práctica, no se le va a permitir ingresar a menos que haga dicha subida. Suponiendo que todos los requisitos se encuentren completados, se va a ofrecer la fila de espera al paciente con las opciones para “Ingresar” o “Rechazar”. 

:::warning[Texto a definir]
Definir textos para el modal de ingreso en la fila de espera. Tener en cuenta los casos de ofrecer para el médico o para la especialidad
:::

Si el paciente rechaza el ingreso, este modal se cierra y queda el paciente en próximos turnos. Si el paciente selecciona la opción para ingresar a la fila de espera, se le va a solicitar que indique día y rango horario para indicar disponibilidad semanal. El paciente va a poder seleccionar un día o todos y un rango horario o todos (los rangos horarios van a ser Mañana, Tarde y Noche). No es posible no seleccionar nada. Solo se va a permitir avanzar cuando se encuentre al menos seleccionado un elemento para el día y otro para el rango horario. Por defecto, se va a encontrar TODO seleccionado. Por último, al avanzar luego de seleccionar lo mencionado, se le comunica que ingresó correctamente (sin mostrar una posición) y se avanza con al modal de confirmación del turno. En este momento se genera el registro de fila de espera en la base de datos con información del paciente, fecha de toma de turno y el turno tomado. 

Para el caso de los ingresos a fila donde el paciente quiera esperar turnos para el mismo médico para el cual tomo el turno, solo se verá el médico a nivel de reportería de pacientes que se encuentran encolados. También es posible que el paciente quiera esperar en la fila de espera para recibir ofertas para cualquier médico de la especialidad del médico para el cual tomo turno. En este caso tenemos que revisar si hay otras agendas de la misma especialidad (para habilitar la pregunta) y si tienen turnos disponibles antes o no. Ya que de existir un turno antes del que el paciente está tomando, se debería de ofrecer primero el turno. De no existir se avanza con el cierre del ingreso a la fila de espera.

:::warning[Importante]
Es necesario definir horarios preciso para los conceptos Mañana, Tarde y Noche.
:::

Para definiciones de prestaciones que manejen un código de consulta, se va a ofrecer la posibilidad de ingresar a la fila de espera para la especialidad. Lo cual implica que cualquier cancelación de un turno de cualquier médico de la especialidad sobre la cual se está tomando el turno va a ofrecerse a este paciente. En estos casos los registros insertados en la base de datos solo van a indicar la especialidad y no el ID del médico de la agenda sobre la cual se toma el turno.

## Circuito para Ofrecimiento de turno

Los turnos que se ofrecen a pacientes que se encuentran en la fila de espera son aquellos que se originan de:
- Cancelación de turnos por usuario WEB: Todos los turnos que se cancelen por accionar del paciente desde cualqueir plataforma van a ser turnos con potencialidad de ser ofrecido a otro paciente en la fila de espera (dependiendo de condiciones como elegibilidad de la agenda).
- Creación de días adicionales para pacientes en fila de espera: Va a ser posible crear un día adicional únicamente para ofrecer los turnos generados a pacientes de la fila de espera. Para esto vamos a tener que modificar la creación de los días adicionales y su tabla, ya que tenemos que permitir una opción “Crear día para pacientes en fila de espera” lo cual va a reservar todos los turnos del día generado y ofrecerlos a pacientes en fila de espera. Para esto se va a ofrecer el primer turno al primer paciente, el segundo turno al segundo paciente, etc. Si se generan más turnos que pacientes en fila de espera, se va a tener que crear una nota para bloquear dichos turnos (REVISAR SI HACEMOS ESTO O NO). 
- Cancelación de turno desde el sistema de turnos: Los turnos generados mediante una cancelación no se ofrecen a pacientes de la fila de espera. Para poder indicar que el turno se va a ofrecer a pacientes de fila de espera, tenemos que crear una función la cual va a estar presente en la agenda del día y va a permitir reservar el turno para pacientes de la fila de espera.
- Creación de un sobreturno desde el sistema de turnos: Los sobreturnos creados manualmente no se ofrecen a pacientes de la fila de espera. Al igual que con los turnos cancelados, para ofrecerlos a pacientes de la fila de espera, se deberá seleccionar la opción para indicarlo.

Cuando se genera un turno por alguno de los medios mencionados arriba, se va a bloquear (*2) (reservar) y ofrecer al primer paciente que se encuentre en fila de espera para las variables de médico y/o especialidad y disponibilidad horaria que coincidan con el turno generado. 
Para determinar el paciente al cual se va a ofrecer el turno, se tiene que hacer una revisión larga que hace a distintas variables que se tienen que contemplar. El circuito lineal va a ser:
1. Controlar si la agenda en la cual se cancela el turno se encuentra parametrizada para ofrecer fila de espera.
2. Buscar los pacientes que se encuentran en la fila de espera para el médico o especialidad que pertenecen a la agenda del turno cancelado.
3. Revisar la fecha de los turnos que ya tienen asignados los pacientes de la fila de espera para no ofrecer un turno más lejano a un paciente que tiene un turno más cercano.
4. Revisar la Obra Social de dichos pacientes y comparar contra el nivel de prioridad que tengan. Se va a buscar al primer paciente con OS de nivel 3. De no existir, de nivel 2 y así bajando hasta llegar a las OS “normales”. 
5. Se ofrece el turno al primer paciente que se encuentre haciendo el proceso del punto 4. Ofreciendolo mediante un link (que lleva un token con vencimiento en el tiempo parametrizado) mediante whatsapp.
6. Si el paciente no acepta en el tiempo designado el turno o lo rechaza, se ofrece al segundo paciente en la fila de espera. Este paso se repite hasta que el turno es aceptado por un paciente.
7. Al aceptar un paciente el turno, se va a cancelar su turno previo y se va a generar un nuevo turno, ofreciendolo a los pacientes en fila de espera nuevamente. 

Para determinar el paciente se va a controlar la fecha de ingreso a la fila para saber cual es el primer paciente que ingreso para dicha agenda/especialidad. En segundo lugar, tenem que revisar la fecha del turno tomado **[TURNOORIGEN]** para determinar si el turno que se ofrece es previo al turno que tiene asignado el paciente. 

Encontrado el registro se va a ofrecer mediante whatsapp (Tenemos que revisar la funcionalidad de whatsapp que usamos hoy con Botmaker para integrar esta comunicación). El mensaje tiene que contar con información del turno ofrecido (día, hora, centro, agenda) y un llamado a acción para tomar dicho turno o rechazar la oferta. De rechazse la oferta hay un paso adicional donde se le consulta al paciente si quiere seguir en fila de espera o no. Tenemos que sumar 1 al contador que hace al campo de ofertas realizadas **[CANTIDADOFERTAS]**  y marcar como verdadero el **[RECHAZOFILA]** si quiere salir de la fila de espera. Si acepta la oferta se tiene que llevar al paciente al portal e ingresarlo hasta la pantalla “Próximos Turnos” mostrando el nuevo turno asignado.

:::warning[Texto a definir]
Solicitar texto para el template de envío por whatsapp al paciente en el cual vamos a ofrecer un turno y solicitar una acción para aceptarlo o rechazarlo. Teniendo en cuenta un texto para la aceptación y la pregunta para cuando lo rechace (si desea continuar en fila o no). Los textos a definir son:
- Envío de oferta
- Aceptación de la oferta
- Pregunta si desea continuar en fila
- Negación a seguir en fila
- Aceptación de seguir en fila
:::

:::note[Alternativa]
Para no ofrecer el mismo turno al mismo paciente podemos crear un campo **[ULTIMOTURNOOFRECIDO]** en donde guardemos el ID del turno que se ofrece. Si el paciente rechaza el turno, guardamos este dato. Cada vez que se ofrece un turno se tiene que comparar dicho dato. También se puede recorrer la fila y cortar con el último registro de la tabla.
:::

Adicionalemnte, se tiene que “Mover” el turno con toda la información que contiene y el pago al nuevo espacio y dejar el espacio del turno anterior (esto lo hace hoy dicha funcionalidad, intercambiando los turnos). Se va a volver a iniciar el circuito con el turno liberado, ofreciendolo al próximo paciente en la fila de espera. El proceso se repite hasta que ya no hay pacientes a quien ofrecer los turnos. 

Importante: El turno que se genera por una cancelación se va a mantener libre (sin reservarse para pacientes de fila de espera) por X minutos.

:::note[Parametrización]
El tiempo que se mantenga el turno libre, sin ser ofrecido a pacientes de la fila de espera va a ser parametrizable.
:::

Si el paciente cancela el turno que se encuentra en un registro de la fila de espera bajo el campo **[TURNOORIGEN]** se tiene que dar de baja dicho registro.