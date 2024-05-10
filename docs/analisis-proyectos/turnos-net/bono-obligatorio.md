---
sidebar_label: 'Bono/Firma Obligatorio'
---

# Solicitud de bono y firma como datos obligatorios

:::info[Objetivo]
**Solucionar los problemas ocasionados por no contar con el bono de consulta y la firma del paciente en los turnos donde se requiere.** 
:::

Resumen de solución: 
- Crear 2 nuevas marcas a nivel de Obra Social para poder identificar cuales requieren un bono de consulta y cuales la firma del paciente en dicho bono. 
- Modificar lógica de circuito de presente para controlar si la Obra Social del turno requiere el bono de consulta y no permitir asignar el presente si no tiene un documento catalogado como tal (ID: 128 - Descripción: "Bono de consulta y Kinesiología")
- Al catalogar o recatalogar un documento a "Bono de consulta y Kinesiología" revisar si la Obra Social del turno requiere de firma del paciente. En caso de ser así, abrir un modal con un mensaje para recordar al operador que debe estar la firma del paciente. Este texto se va a cargar en el sistema como un nuevo texto parametrizable para ser definido como correpsonda.
- Al querer cerrar una agenda se va a controlar si los turnos bajo control requieren bono de consulta y se controlará que cuentan con estos documentos. En el caso de contar con turnos que requieran de bono de consulta pero que no lo tengan asociado, no se va a permitir avanzar en el cierre de la agenda.

## Funcionalidad

Al momento de dar el presente desde cualquier pantalla que contenga dicha acción ("Atención paciente", "Agenda del día" o "Prestaciones") vamos a controlar si la Obra Social del turno sobre el que se dispara requiere de un bono de consulta. Si no requiere bono, se avanza normalmente corriendo el resto de las comprobaciones. Ante el caso de que se requiera bono, se va a verificar que exita un documento en el turno con categoría "Bono de consulta y Kinesiología". De encontrar un archivo bajo esa categoría se avanzará a la asignación del presente, dependiendo del resto de las comprobaciones. Si no se encuentra dicho archivo se agregará como motivo a mostrar al momento de recibir el error por el cual no se puede agregar el presente, indicando que falta dicho bono de consulta. REVISAR SI HAY LÓGICA PRA CLAVE DE SUPERVISOR

Al recatalogar un documento asociado al turno usando la categoría "Bono de consulta y Kinesiología" se va a controlar si la Obra Social del turno requiere la firma del bono médico y de ser así se va a mostrar una ventana con un mensaje para reccordar al operador que debe estar la firma del paciente. El texto del mensaje será el que se ingrese en la parametrización de mensajes desde "Archivo > Textos Configuración". Esto es meramente informativo ya que no cuenta con lógica adicional. Al cerrar el mensaje el operador sigue operando el sistema normalmente

El cierre de agenda mostrará un error adicional ante la existencia de turnos cuya Obra Social requiere bono de consulta obligatorio y no tengan un documento catalogado con el ID ya mencionado. Esta situación no debería de ocurrir debido a que no se podrá asignar un presente con un turno en estas condiciones pero servirá como un segundo nivel de control de los turnos que se encuentran en la agenda.

## Modificaciones de parametrización

Se van a agregar al ABM de Obras Sociales o su pantalla de edición los parámetros para poder indicar:
- Requiere Bono de consulta: mediante esta marca van a identificar todas las Obras Sociales que requieren de un bono de consulta para todas sus planes y prestaciones. Esta es la marca a consultar en los circuitos mencionados para permitir avanzar o frenar la asignación de estado de presente al turno.
- Requiere firma en Bono: Solo se va a habilitar esta opción si la anterior se encuentra seleccionada (tilde en checkbox). De esta forma se tiene que indicar cuales son las Obras Sociales que aparte del bono solicitan que este presente la firma del paciente. Es esta marca la que va a hacer al comportamiento de sistema para los casos mencionados de recatalogación de documentos al cambiar un documento a "Bono de consulta y Kinesiología"

Ambos checkbox se van a encontrar en la pestaña de "Parámetros" junto con las que se usan actualmente para indicar que las órdenes médicas no requieren firma del paciente.

## Fuera del alcance

No se harán modificaciones del portal del turnos ni de aplicación o terminal de autogestión debido a que solamente se indicará que requieren bono obras sociales que no pueden pasar por terminal de autogestión. Este punto tenemos que reveerlo en el proyecto de redirección de pacientes, ya que se debe contemplar si la marca de que no son aptas de pasar por terminales es suficiente para el reenvío del paciente o si va a existir un comportamiento especifico para pancientes cuya obra social solicita bono de consulta médica obligatorio. 
Tampoco será este documento parte del control de órdenes que se hace para prácticas.
La categoría de documento va a seguir siendo la que existe hoy ya que no se deberían de chocar logícas de Kinesiología con lógicas de bono de consulta por su naturaleza de consulta médica.



