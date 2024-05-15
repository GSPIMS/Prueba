---
sidebar_label: 'Bono/Firma Obligatorio'
---

# Solicitud de bono y firma como datos obligatorios

:::info[Objetivo]
**Solucionar los problemas ocasionados por no contar con el bono de consulta y la firma del paciente en los turnos donde se requiere.** 
:::

Resumen de solución: 
- Crear una nueva marca a nivel de Obra Social para poder identificar cuales requieren un bono de consulta
- Modificar lógica de circuito de presente para controlar si la Obra Social del turno requiere el bono de consulta y no permitir asignar el presente si no tiene un documento catalogado como tal (ID: 128 - Descripción: "Bono de consulta y Kinesiología"). Además, mostrar un mensaje que recuerde al usuario que debe estar la firma del paciente en dicho bono. Este mensaje se va a cargar en el sistema como un texto parametrizable para ser definido como corresponda.
- Al querer cerrar una agenda se va a controlar si los turnos bajo control requieren bono de consulta y de ser así se verificará que tengan adjunto un archivo catalogado como se menciona más arriba.. En el caso de contar con turnos que requieran de bono de consulta pero que no lo tengan asociado, no se va a permitir avanzar en el cierre de la agenda.


## Funcionalidad

Al momento de dar el presente desde cualquier pantalla que contenga dicha acción ("Atención paciente", "Agenda del día" o "Prestaciones") vamos a controlar si la Obra Social del turno sobre el que se dispara requiere de bono de consulta. Si no requiere bono, se avanza normalmente corriendo el resto de las comprobaciones. Ante el caso de que se requiera bono, se va a verificar que exista un documento en el turno con categoría "Bono de consulta y Kinesiología". De encontrar un archivo bajo esa categoría se avanzará a la asignación del presente, dependiendo del resto de las comprobaciones. Si no se encuentra dicho archivo se agregará un texto como motivo a mostrar al momento de recibir el error por el cual no se puede asignar el presente, indicando que falta dicho bono de consulta. Con lo cual se va a bloquear la posibilidad de asignar el presente. 
Se va a permitir ingresar clave de supervisor para el caso donde solamente falte este documento para poder asignar el estado presente al turno. Siempre cuando no exista otro potencial problema que no permita ingresar clave de supervisor para poder avanzar.
Adicionalmente, si la Obra Social del turno cuenta con dicha marca, se va a mostrar un mensaje para recordar al usuario que el bono debe estar firmado. El texto del mensaje será el que se ingrese en la parametrización de mensajes desde "Archivo > Textos Configuración". Esto es meramente informativo ya que no cuenta con lógica adicional. Al cerrar el mensaje el operador sigue trabajando con el sistema normalmente.

El cierre de agenda mostrará un error adicional ante la existencia de turnos cuya Obra Social requiere bono de consulta obligatorio y no tengan un documento catalogado con el ID ya mencionado (ID: 128). Esta situación no debería de ocurrir debido a que no se podrá asignar un presente con un turno en estas condiciones pero servirá como un segundo nivel de control de los turnos que se encuentran en la agenda.

## Modificaciones de parametrización

Se van a agregar al ABM de Obras Sociales o su pantalla de edición el parámetro para poder indicar:
- Requiere Bono de consulta: mediante esta marca van a identificar todas las Obras Sociales que requieren de un bono de consulta para todos sus planes y todas sus prestaciones. Esta es la marca a consultar en los circuitos mencionados para permitir avanzar o frenar la asignación de estado de presente al turno.
Este checkbox se va a encontrar en la pestaña de "Parámetros" junto con las que se usan actualmente para indicar que las órdenes médicas no requieren firma del paciente.


Ambos checkbox se van a encontrar en la pestaña de "Parámetros" junto con las que se usan actualmente para indicar que las órdenes médicas no requieren firma del paciente.

## Fuera del alcance

- No se harán modificaciones del portal de turnos ni de aplicación o terminal de autogestión debido a que solamente se indicará que requieren bono obras sociales que no pueden pasar por terminal de autogestión. Este punto tenemos que reverlo en el proyecto de redirección de pacientes, ya que se debe contemplar si la marca de que no son aptas de pasar por terminales es suficiente para el reenvío del paciente o si va a existir un comportamiento específico para pacientes cuya obra social solicita bono de consulta médica obligatorio. 
- Tampoco se solicitará el bono de consulta desde el portal como un documento adicional a controlar.
- La categoría de documento va a seguir siendo la que existe hoy ya que no se deberían de chocar lógicas de Kinesiología con lógicas de bono de consulta por su naturaleza de consulta médica.


## Para Desarrollo

A nivel de base de datos agregar a tabla OBRASOCIAL_PARAMETROS: (agregar detalles)
- REQUIEREBONO: Para indicar cuando la Obra Social requiere bono de consulta médica.
- REQUIEREBONOFIRMA: Para indicar cuando la Obra Social requiere firma en el bono de consulta.

Lógica a agregar:
En el metodo 'objTurno.AsignarPresente' tenemos que agregar un condicional al circuito de consultas médicas para comprobar si la Obra Social requiere bono de conosulta médica 




