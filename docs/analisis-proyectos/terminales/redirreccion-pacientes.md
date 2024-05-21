---
sidebar_label: 'Redirección de pacientes'
---

# Redirección de pacientes

:::info[Objetivo]
**Tomar el control de a donde se va a redirigir al paciente desde las terminales para que todos pasen por las mismas. Ya sea un puesto de recepción, una fila o un punto de atención.**
:::

Metodologías posibles para el reenvío de pacientes desde las terminales a recepción o punto de espera para atención:

- Envío a una única cola de atención en recepción (ejemplo actual: CME)
Es el método más fácil porque mediante texto se le indica al paciente que debe hacer la fila como una única fila para la recepción indicada.
- Envío a distintas colas de atención en recepción (ejemplo actual: Rivadavia)
En este caso se tiene que poder definir un camino a seguir para una opción especifica, pudiendo crear los lugares de atención o las filas que se encuentran implementadas para que el paciente sepa a donde debe de ir.
- Envío a un puesto especifico en la recepción predefinida o para otra recepción del mismo centro (Ejemplo actual: Punto de control)
Para este caso, hacemos algo similar a lo que se plantea en el punto anterior. La diferencia es que se manda a un puesto de recepción y no a una cola. A nivel de configuración va a ser lo mismo (sin identificar el puesto).

Para resolver las 3 metodologías mencionadas dentro de un único circuito a contemplar es que vamos a trabajar en parametrización en base a la selección de:
- un TIPO de redirección para poder identificar cual es el motivo de la redirección. Estos motivos son los únicos elementos que van a estar precargados y se van a tener que modificar a medida que se mejore el sistema y se generen nuevas situaciones de atención. Estos motivos se listan más abajo. 
- un Centro donde se va a usar la redirección. Esto sirve para poder tener distintos comportamientos en cada centro según se necesite. 
- un Mensaje preconfigurado que exista en la configuración de mensajes que tenemos en .net para tomar como mensaje a mostrar al paciente una vez que se produce la redirección.  
- un Puesto/Fila hacia donde se va a redirigir al paciente. Estos puestos o filas se tienen que crear en la tabla puestos y se van a referenciar acá para saber a donde se redirige al paciente usando el texto que es la descripción del puesto.

El objetivo de manejar la parametrización de esta manera es poder hacer modificaciones en cualquier momento desde el uso del sistema, ya sea mediante el cambio de un texto, por cambios estructurales de una recepción, etc. Como se menciona arriba, lo único que depende del sector de Desarrollo es el listado de los potenciales motivos de redirección y cualquier modificación se va a tener que solicitar a dicho sector.

Hoy en día contamos con una tabla para el envío del paciente a la recepción para cuando debe ir a punto de control (PUNTOCONTROL). Mediante la lógica se identifica cual es el problema que deriva a punto de control y se completa la información que hace a esa redirección. Esto no permite parametrización para poder identificar a donde se va a dirigir el paciente. Es por esto que deberíamos de modificar esta tabla para contemplar el ID del puesto que se va a usar como destino para la redirección. A su vez permitir ingresar una descripción al puesto para poder ingresar lo que va a aparecer en la terminal completando el mensaje de la redirección.

Todas las redirecciones de los pacientes desde las terminales son:
- A esperar ser atendido, identificando un consultorio médico (prácticas o consultas)
- A esperar ser atendido, identificando una sala de espera o piso (laboratorio)
- A pagar en la recepción cuando el paciente selecciona opción de abonar en efectivo.
- A puesto de recepción o fila para cuando el paciente quiere retirar un estudio sea de laboratorio o de un estudio médico.
- A puesto de recepción o fila para cuando el paciente tiene una obra social que no puede recepcionarse por las terminales y está avanzando para esto.
- A puesto de recepción o fila para cuando el paciente quiere solicitar un turno de consulta/práctica.
- A puesto de recepción o fila para cuando el paciente quiere solicitar un turno de Laboratorio.
- Error al querer abonar mediante mercadopago
- La obra social del turno tiene método de validación interna (definición de valores en la orden o autorización que trae).
- El paciente no cuenta con el token de su obra social para validar online
- Paciente de Kinesiología tiene turno que no se encuentra controlado.
- El paciente no tiene la autorización del estudio.
- Protocolo de laboratorio que no se encuentra verificado
- Opción de prácticas deshabilitada para la terminal
- Punto de control debido a:
    - No están bien los datos de la obra social del paciente
    - La validación online de las prestaciones del turno vuelven rechazadas
    - La agenda en la que se encuentra el turno está cerrada
    - Cuando el operador interno identifique que algo está mal en la orden que sube el paciente.
    - Errores varios en el proceso de recepción
    - Tiene orden médica pero no cuenta con el médico solicitante ingresado. Si el paciente no lo ingresa

Todos los motivos mencionados tienen que ser eventos controlados los cuales tenemos que tabular para que se puedan configurar como un disparador de un mensaje (texto + imagen) que se va a mostrar como resultado al paciente.

Desde el usuario, va a tener que poder crear registros que contengan:
- Disparador: Drop down con todos los posibles eventos mencionados más arriba. Esto es lo que tenemos que poder controlar para determinar cuando estamos ante cada una de estas situaciones. Al agregar nuevas funcionalidades tenemos que asegurarnos de analizar la ampliación de eventos para que se pueda mantener el control a nivel de usuario
- Texto visible: Texto que vamos a mostrar en pantalla para comunicar al paciente lo que tiene que hacer o a donde tiene que ir. Este campo maneja texto libre con tags HTML para el formato
- Imagen adjunta: El usuario tiene que poder subir un PNG que se muestre por encima del texto para mejorar la interpretación de lo comunicado. Esta imagen tiene que ser un PNG con fondo transparente para mantener el fondo de la TAG.
- Marca de punto de control: Es un campo booleano donde se indica si la redirección envía a punto de control o no. Esto es para poder identificar fácilmente si el reenvío es a punto de control.

Para contener esta información podemos usar PUNTOCONTROLTIPO y PUNTOCONTROLTEXTO ampliando a los potenciales eventos en la primera y los textos en la segunda. Tenemos que agregar la data de la imagen y si es o no punto de control. La segunda tabla ya tiene campo para definir centro y recepción, para poder identificar que se va a mostrar en ese centro específicamente. 


## Fuera del alcance

- Opción de habilitar llamador en la recepción.
