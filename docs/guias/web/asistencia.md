---
sidebar_label: 'Asistencia (subida de orden)'
---

# Asistencia a ordenes subidas desde portal

El módulo de asistencia para las ordenes subidas por pacientes desde el portal se usa para ayudar en los casos donde el paciente no se anima a tomar un turno por miedo a equivocarse en la selección de una prestación.

Este módulo consiste de:
- Modificaciones en el portal para que el paciente pueda ingresar a la opción de solicitud de asistencia desde el circuito de toma de turnos para estudios médicos. Al seleccionar dicha opción, se le solicita revisar los datos de contacto y luego subir la orden.
- Pantalla de control de orden y asociación de turno. Este es el punto a desarrollar en esta guía. Es el paso en el cual se requiere de interacción de un usuario de DIM para que el paciente obtenga el turno que busca.

## Pantalla de control

![Pantalla de control](/img/pantalla_control_.png)

La pantalla de control es la que utiliza el usuario de DIM para poder acceder a las solicitudes de asistencia y contactarse con el paciente para así coordinar un turno. Los pasos a seguir son:
1. Ingresar al nuevo portal BETA de turnos turnos.dim.com.ar
2. Seleccionar la opción “Asistencia”
3. La pantalla que se abre es la que se encuentra más arriba (a.1) donde se listan en una tabla todos los registros que hacen a diferentes solicitudes de asistencia. (cada línea es una solicitud aparte. Es importante tener en cuenta que pueden existir distintas solicitudes de un mismo paciente y se tiene que controlar si es la misma orden o no). 
Para empezar a trabajar se va a seleccionar el primer registro de la misma haciendo click con el mouse por encima de la fila (se puede seleccionar tocando en cualquier lugar de la fila)
4. Usar el botón “Ver orden” (D) para analizar que prestación es la que se solicita en la misma. En el caso de ser muchas órdenes que requieran distintos turnos, hoy no tenemos forma de relacionarlas a cada turno individual.
5. En el botón “Ver Disponibilidad” (F) se puede observar los días y horas en que el paciente definió su preferencia para la asignación del turno. En este paso se puede buscar y asignar el turno según su preferencia de día y hora o contactar al paciente (con los datos en la tabla) y coordinarlo con este.
6. Una vez asignado el turno se lo debe relacionar con el registro de asistencia. Para esto usar el botón “Relacionar con turno” (C) y seleccionar el turno asignado en el paso anterior para que la orden se adjunte al mismo como Orden Médica. Como se menciona en el punto 4, de exisitir varias ordenes que hagan a distintos turnos, manualmente hay que llevar estas ordenes adicionales a los turnos asignados bajo las distintas prestaciones de las diferentes órdenes.

Adicionalmente a lo anterior, la pantalla cuenta con otros botones y filtros para un manejo más dinámico. Hay 2 botones que son para trabajar con la continuidad de una solicitud de asistencia en cuanto a no poder contactar al paciente en los casos donde es necesario hacerlo. Estos son:
- Marcar visto: Seleccionando un registro, al tocar en este botón, nos aparecerá una ventana en la cual podemos indicar el motivo. Esto guarda el mensaje que se escribe y el usuario y fecha de cuando se creo. Este mensaje es accesible desde un botón que aparece solamente cuando un registro tiene un comentario adicional.
- Anular Documento: Esta acción permite quitar de la tabla un registro para cuando se necesita sacarlo debido a que se necesita hablar con el paciente y no hay forma de comunicarse con el mismo. 