---
sidebar_label: 'Control de orden'
---
Borrador

# Control de Orden web (prácticas y laboratorio)

:::info[Objetivo]
**Contar con un módulo de control de órdenes que suben los pacientes mediante el portal o la aplicación, adaptandose al circuito operativo y ofreciendo herramientas para un mejor entendimiento de lo que el usuario tiene que hacer.** 
:::

Sitio: www.turnosdim.com.ar

## Funcionalidad

Con esta herramienta se van a controlar todas las órdenes que los pacientes suben como adjunto a los turnos que toman desde el portal de pacientes o la aplicación. Esto principalmente sirve para que el paciente pueda hacer el proceso de recepción desde la terminal de autogestión o desde la aplicación de DIM sin necesidad de tener que un representante de DIM tenga que interactuar en dicho proceso. 

Las condiciones para que el turno cumpla con lo necesario para un proceso de recepción más rápido son:
- Identificación y aprobación de la orden del turno.
- Identificación y aprobación de la autorización del turno (cuando se requiera)
- Identificación del médico solicitante según lo indique la orden (debe de ser ingresado si el paciente no lo hizo)
- Validación de las prestaciones del turno. Viendo que se correspondan con lo que se solicita en la orden médica.
- Identificación del centro solicitante según corresponda.

Una vez que se asigna un turno y se adjunta una orden este se lista en la tabla en la que se encuentran todos los turnos a controlar. Cuando el usuario de DIM ingresa al control de órdenes, va a ver la tabla mencionada con todos los turnos que requieren control. En esta va a visualizar información del turno, del paciente y el estado en que se encuentra el registro. Este estado puede ser:
- A controlar: La orden del turno todavía no fue controlada.
- Validado: El registro se va a mostrar como validado cuando un operador haga el control y en este encuentre que todo se encuentra en condiciones para que el paciente haga la recepción personal desde la terminal o desde su aplicación.
- Rechazado: Ante el caso de que haya un problema con el turno, se va a generar un rechazo. 

El estado del control de la orden va a depender de las respuestas que se den a un circuito de preguntas que se hacen cuando se inicia cada control. Estas respuestas van a determinar si es posible avanzar a la validación de la orden o si se va a rechazar el turno y el motivo por el cual se rechaza.

## Circuito de control

El circuito de control de órdenes se inicia cuando el usuario ingresa a controlar un registro de la tabla.

### Respuesta a preguntas predeterminadas

Lo primero que se hace es responder todas las preguntas que aparecen en el margen superior izquierdo. Dependiendo de estas respuestas se va a rechazar orden (no existe un botón de rechazo) o se va a permitir validar el registro. Las preguntas son las que se listan a continuación. No siguen un orden ya que algunas preguntas van a depender de la respuesta de una pregunta previa (ver diagrama debajo):
- **¿Es legible la orden?**: Esta es la primer pregunta y su sentido es descartar rápidamente aquellos registros de órdenes que no se pueda siquiera leer por foto mal tomada o incorrecta. 
Respuesta afirmativa hace avanzar el circuito
Respuesta negativa genera mail al paciente indicando que hay un problema con la orden.

- **¿Los datos de la orden médica coincideon con el del sistema?**: Esta pregunta se contesta contrastando los datos de la orden con los datos que tenemos del paciente en sistema. El objetivo es descartar aquellas ordenes que no pertenezcan al paciente o que tengan datos erroneos.
Respuesta afirmativa hace avanzar el circuito
Respuesta negativa va a llevar a la pregunta "¿Puede ir a demanda?"

- **¿Puede ir a demanda?**: Esta pregunta se hace para indicar si el problema con los datos de la orden se pueden solucionar mediante la atención en demanda espontanea o no.
Respuesta afirmativa rechaza el turno y se envía un mail al paciente indicando que se acerque al centro para poder atenderse.  
Respuesta negativa rechaza el turno y se envía un mail al paciente indicando que los datos de la orden no concuerdan y que debe rever la orden.

- **¿El pedido del médico coincide con los datos de la orden?**: Para esta pregunta se tiene que controlar que la prestación cargada en el turno (la que el paciente selecciona) coincida con lo que solicita el médico en la orden.
Respuesta afirmativa hace avanzar el circuito
Respuesta negativa lleva a la pregunta "¿Le sirve el turno"?

- **¿Le sirve el turno?**: Si es posible modificar las prestaciones para arreglar aquellas situaciones donde el paciente solicitó el turno de manera erronea se va a indicar que el turno le sirve al paciente para luego poder hacer la modificación.
Respuesta afirmativa hace avanzar el circuito
Respuesta negativa rechaza el turno y se envía mail al paciente indicando que hay un problema en la selección de la prestación que hizo el paciente al tomar el turno. 

- **¿Está la autorización?**: Para los casos donde el turno requiera de autorización se va a preguntar si se encuentra la autorización adjunta al turno. Debido a que la categoría de los documentos es "Orden o autorización" se tiene que identificar si dentro de los documentos subidos está la autorización.
Respuesta afirmativa hace avanzar el circuito
Respuesta negativa rechaza el turno y se envía un mail al paciete indicando que falta la autorización.

- **¿Es correcta la autorización?**: Si el paciente subió la autoización esto no quiere decir que se encuentre en condiciones de avanzar con la recepción del turno. Es necesario controlar que la autorización sea correcta y sus datos se correspondan con la prestación del turno
Respuesta afirmativa hace avanzar el circuito y se inicia el proceso de correcciones del turno
Respuesta negativa rechaza el turno y se envía un mail al paciente indicando que hay un problema con la autorización subida. 

Circuito de preguntas:
![Necesito turno](/img/control-ordenes/circuito-preguntas.png)

### Modificación de atributos del turno

Una vez que se responden todas las preguntas se habilita la modificación de los datos del turno para cumplir con lo que falte y sea obligatorio para llegar a la validación del turno. Esto se representa con un cambio visual en la pantalla donde se le da prioridad al visualizador de documentos del turno y a sus datos. Dejando del lado derecho un apartado con datos del paciente y los pasos a realizar para avanzar en el circuito.
En esta etapa se van a hacer todas las modificaciones necesarias de los atributos del turno. Estas son:
- Recatalogar los documentos del turno: Es necesario que el turno cuente con un documento catalogado como orden para poder hacer la validación del mismo. Se tienen que ver todos los documentos del turno y catalogar dependiendo de la categoría a la que pertenezcan (sea una orden/autorización o documentación adicional complementaria)
- Prestacion o prestaciones del turno: En caso de que se pueda salvar un error en la toma del turno donde no coincida la prestación seleccionada con la orden del turno, se tiene que modificar en este paso. Si la prestación coincide con lo solicitado, no se tiene que hacer ninguna modificación. Es probable que la prestación requiera autorización de la obra social. En estos casos es necesario escribir el número de autorización al lado de la prestación.
- Médico solicitante: Este dato lo puede cargar el paciente al momento de tomar el turno. En este caso, se tiene que controlar que corresponda al que figura en la orden. Si no coincide el médico ingresado o el paciente no lo cargo, se va a tener que agregar buscandolo por alguna de sus matróiculas. De no existir el médico solicitante, se tiene que agregar.
- Centro derivador: Este dato se va a tener que agregar siempre ya que no se solicita al paciente de manera previa. Se identifica el centro al cual pertenece la orden y se selecciona.

:::info[Centro Derivador]
Los centros derivadores se agregan desde el sistema de turnos. No es posible agregar un nuevo centro derivador desde este sistema. Se recomienda agregar todos aquellos centros que se necesitan identificar y usar un registro genérico para órdenes que provengan de médicos particulares
:::

