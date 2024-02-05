---
sidebar_label: 'Toma de turnos'
---

# Toma de turnos desde Terminales

:::info[Objetivo]
**Permitir al paciente tomar turnos de consultas, prácticas y laboratorio desde la terminal de autogestión, sin necesitar interacción con representantes que se encuentran en la recepciones de DIM. Aumentar el porcentaje de gente que usa las terminales.**
:::

## Funcionalidad

Se solicita ampliar la funcionalidad de las terminales para permitir que los pacientes tomen turnos desde las mismas, tomando en cuenta los parametros que actualmente se consideran para la toma de turnos desde el portal.

Desde la terminal de autogestión vamos a cambiar la lógica del botón “Necesito turno”. Mediante el cual el paciente va a seleccionar para que desea tomar turno y luego avanzar en el circuito hasta su asignación. Este  va a ser el mismo que manejamos hoy en el portal web de pacientes, tomando en cuenta todos los filtros, bloqueos y topes que hoy se aplican en la búsqueda de turnos a ofrecer. Se modifica la navegación para adaptar la usabilidad al hardware que que se usa en las terminales de autogestión.

Para poder ingresar a esta opción, tenemos que solicitar la clave del portal del paciente. Si no tiene clave, se va a indicar que se acerque a recepción para poder regularizar la situación. Este punto es importante ya que representaría una falla de seguridad la asignación de turnos sin autorización a otros pacientes y la terminal (para simplificar el uso para la recepción) no solicita clave de acceso.

**Podemos usar un circuito para que genere la clave en ese momento pero va a llevar tiempo del paciente ahí y podemos tener demoras con el envió de mail**

## Circuito de toma de turno

El primer paso va a ser definir si se desea tomar un turno para:
- Consulta: Mismos resultados que en el portal. En vez de mostrar un drop down, se pasa a un esquema de botones
- Estudio Médico: Mismos resultados que en el portal. En vez de mostrar un drop down, se pasa a un esquema de botones
- Laboratorio: Se tiene que seleccionar para que se desea tomar un turno de laboratorio dentro de los grupos de estudio que se encuentran cargados. En vez de mostrar un drop down, se pasa a un esquema de botones

:::note[Importante]
Tenemos que tener en cuenta que el proyecto de pago y subida de orden obligatoria van a afectar a las terminales, con lo cual vamos a tener que agregar estos pasos para un preaviso antes de empezar a la búsqueda (aclarando que se usa MP) y agregar los pasos a la hora de seleccionar un turno para asignarlo. 
:::

La principal diferencia con el portal de turnos es que para la terminal vamos a mostrar como botones individuales los resultados a una búsqueda de prestaciones. Se agrega un teclado siempre visible para poder escribir la prestación buscada. Para seleccionar una prestación y avanzar con la búsqueda de turnos el paciente selecciona el botón que corresponda. Si los resultados a una búsqueda realizada por el paciente superan los 10 elementos, se van a ir listando a la derecha y se van a habilitar botones de navegación para avanzar o retroceder de página. Apuntamos a que en los 10 elementos que se muestran se encuentre la prestación que el paciente busca. En este listado vamos a aplicar un filtro por frecuencia para ordenar los resultados según la frecuencia con la que se toman las definiciones de prestaciones (como en el portal).

Los elementos se van a ir filtrando a medida que el paciente escribe cada carácter en el teclado (con un mínimo de 3 carácteres para mostrar resultados)

:::note[Prueba Funcional]
Se trabajo en un carousel deslizable para probar como sería la navegación desde los monitores que se usan en las terminales. La experiencia según diversas personas no fue buena y se descarto la idea. Por esto es que quedaron botones para avanzar o retroceder en todas las pantallas donde mostremos una cantidad alta de elementos.
:::

Al seleccionar una prestación del listado, se buscan los turnos con la misma lógica con la que funciona el portal y se muestran los resultados con el uso de cards (igual que en portal). Esta pantalla va a contar con un botón en el margen superior derecho, para poder acceder a los filtros, los cuales se van a abrir desde el margen derecho de la pantalla. El paciente puede interactuar con los filtros (mismos que en el portal) y al seleccionar cada uno de estos, se abrirá un modal con la información que hace a dicho filtro. Al cerrar el modal, tenemos que aplicar los cambios realizados al filtro que se abrió y aplicar a los turnos que se muestran como resultado de búsqueda. No es necesario tocar el botón "Volver" para aplicar el nuevo filtro (como en el portal).
Para tomar el turno el paciente tiene que seleccionar el botón "Confirmar". No se va a avanzar si toca en otro lugar del Card. Como se explico más arriba, al igual que con la pantalla de búsqueda de prestaciones, los turnos ofrecidos se navegan mediante botones en los márgenes de la pantalla (apareciendo el botón para regresar en naranja solamente cuando se avanza en la navegación de elementos. Sino, se va a ver en gris).

Si la prestación seleccionada pertenece a un circuito, mostramos la selección de dicha prestación por encima de los resultados de la búsqueda, permitiendo agregar más prestaciones, las cuales se van listando en doble fila, ampliando hacia la derecha (ver imagen de referencia). Si se superan los 4 elementos, se va a poder navegar como un carousel adicional al que se usa para navegar las prestaciones que son resultados de búsqueda.

Si el paciente está tomando un turno de prácticas o de laboratorio, vamos a agregar los pasos de carga de médico solicitante y subida de orden y firma (como en el portal). Al cargar esta información, se ingresa en el circuito de control de órdenes de terminales pero como un registro del día, mostrandolo en el listado que usamos actualmente para los pacientes que se están recepcionando.
Como en el portal, no permitimos abonar hasta que la orden este controlada.

**Que hacemos si se rechaza la orden desde el control?**
**Puede el paciente saltear pasos que hoy no son obligatorios para la toma de turnos? Por ejemplo no subir la orden o el médico solicitante**

:::note[Importante]
La solicitud de datos adicionales al tomar un turno de prácticas o de laboratorio va a ser parametrizable desde la tabla TERMINAL. Con un parámetro para prácticas y otro para laboratorio.
Tenemos que tener en cuenta que tenemos que modificar el listado de control de órdenes para identificar que turno viene de un proceso de recepción para el presente y cual es un turno tomado para atención futura (no sin turno previo o ADE) 
:::

:::warning[Texto a definir]
Indicar que texto se va a usar para indicarle al paciente que el turno se tomo con exito y subió todo lo requerido así para cuando se le rechace la orden.
:::

Terminado el circuito de toma de turno, preguntamos al paciente si necesita hacer algo más o no. Esto va a manejar un timer que se ve en la opción "No" donde si se cuple con el tiempo del timer, se cierra sesión. Si toca que "No", se cierra sesión y se vuelve al login. Si toca en el botón "Si" se vuelve al menú principal para que avance con lo que necesite.

## Reportería

Los turnos que se den por este medio se van a poder agregar a todos los listados actuales del sistema de turnos como de power BI a través del usuario como se hace actualmente. Los usuarios de las terminales son todos "TAGYYYx" siendo 'YYY' la denominación abreviada del centro y 'x' el número que identifica la terminal en el centro.

## Pantallas

Las pantallas que tenemos que agregar a la terminar de autogestión son aquellas necesarias para poder cumplir con el circuito de toma de turnos. El proceso de base es identico al del portal o aplicación pero con diferencias que lo adecuan a interacción con un monitor touch como el de las terminales.

Todas las pantallas cuentan con un botón para volver a la pantalla anterior. Tenemos que guardar lo que se va haciendo en los pasos posteriores al momento de volver para atrás. Si se vuelve al menú principal, se pierde toda esta

**Opción "Necesito turno"**

Selección de para que quiere turno el paciente. Consulta, práctica o laboratorio.

![Necesito turno](/img/terminales/inicio-toma-turno.png)

**Selección de para quien es el turno**

En caso de contar con familiares, se van a mostrar en este paso. El paciente tiene que seleccionar si es para uno de estos o para el/ella.

![Turnos más solicitados](/img/terminales/seleccion-familiar.png)

**Turnos más solicitados previo a búsqueda de consultas médicas**

Los 6 turnos de consultas más solicitados por el paciente. Con la opción de avanzar a buscar otras prestaciones.

![Turnos más solicitados](/img/terminales/consultas-mas-solicitadas.png)

**Selección de centro para toma de turno de laboratorio**

En esta pantalla se seleccionan los centros para los cuales se va a buscar turno de laboratorio.

![Turnos más solicitados](/img/terminales/seleccion-centro-laboratorio.png)

**Búsqueda de prestaciones**

Pantalla en la que el paciente selecciona la prestación que está buscando.

![Búsqueda sin texto](/img/terminales/busqueda-prestaciones.png)

**Búsqueda de prestaciones (en circuito)**

Ante el caso de que la prestación seleccionada pertenezca a un circuito, se va a agregar (junto con otras prestaciones del mismo circuito) por debajo del campo de búsqueda.

![Búsqueda con texto](/img/terminales/busqueda-circuito.png)

**Oferta de turnos**

Listado de turnos ofrecidos al paciente según la búsqueda realizada.

![Oferta de turnos](/img/terminales/oferta-turnos.png)

**Oferta de turnos (abriendo filtros)**

Ventana de filtros para ajustar la oferta de turnos.

![Oferta de turnos](/img/terminales/oferta-turnos-filtro.png)

**Oferta de turnos (ejemplo de filtro)**

Ejemplo de un modal de filtro con el cual el paciente interactua para especificar lo que que necesita.

![Oferta de turnos](/img/terminales/nuevo-turno-filtros.png)

**Solicitud de médico solicitante**

Solicitud del médico solicitante al paciente, según se indica en la orden.

![Oferta de turnos 2](/img/terminales/nuevo-turno-medico-solicitante.png)

**Pregunta si el paciente tiene más de una orden**

Pregunta al paciente de si cuenta con una o más órdenes para el turno asignado.

![Oferta de turnos 2](/img/terminales/nuevo-turno-cantidad-ordenes1.png)

**Pregunta si el paciente tiene más de una orden**

Selección de cantidad de órdenes que tiene el paciente para el turno asignado. Solo se muestra este paso si se responde afirmativo en la pregunta anterior.

![Oferta de turnos 2](/img/terminales/nuevo-turno-cantidad-ordenes.png)

**Solicitud de orden**

Toma de foto de orden para adjuntarla al turno.

![Oferta de turnos 2](/img/terminales/nuevo-turno-foto-orden.png)

**Confirmación de foto de orden**

Pregunta para que el paciente indique si la foto de la orden se encuentra en condiciones para ser enviada a control. Si se indica que no, se vuelve a solicitar la foto.

![Oferta de turnos 2](/img/terminales/nuevo-turno-confirmacion-orden.png)

**Solicitud de firma (de ser requerida)**

Solicitud de firma del paciente siempre y cuando la obra social lo requiera.

![Oferta de turnos 2](/img/terminales/nuevo-turno-firma.png)

**Espera en control de orden**

Pantalla que se muestra mientras se controla la orden del paciente. La idea es contar con publicidad o una comunicación hacia el paciente.

![Oferta de turnos 2](/img/terminales/nuevo-turno-control-orden.png)

**Confirmación de turno**

Confirmación de que el turno se asignó con exito y pregunta de si desea hacer algo más o no. De querer hacer algo más, se lleva al paciente al menú principal.

![Oferta de turnos 2](/img/terminales/nuevo-turno-confirmacion.png)



