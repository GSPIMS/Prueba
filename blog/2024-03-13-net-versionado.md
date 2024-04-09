---
slug: Versión NET 2024.03.13  
title: Versión NET 2024.03.13
authors: [gms]
tags: [netapp, versionado]
---

En la tabla Usuarios ahora cambia correctamente el caption del botón "Inactivo/Activo".
• En el formulario de huella, ahora si el médico está inactivo mostrará las huellas. Además se agregó un texto que indica si está activo o no.
• Se cambio el nombre del check "Excluye Impresión consulta Online (Contrato Dim)" a “No imprime bono consulta(Consulta Dim)”.

• Módulo del Controlador de Turnos: 

Se agregó un botón para enviar como pendiente de facturación. También se conserva el atajo (CTRL+F8).
Se agregó un filtro en memoria, el cual mostrará u ocultará las prácticas pendiente de facturación. El mismo se llama "Ver Pendientes" y por defecto las ocultará. 
Se modificó como se seleccionan los centros. Ahora existe una función que selecciona todos.

• Módulo de Pendientes de facturación:

Se pueden ver las prestaciones del turno a través del botón y del atajo (Barra espaciadora).
Se puede copiar el ID el turno desde la grilla
Cuando se lista las prestaciones se puede ver el código de la práctica en la grilla.
Se pueden levantar en bloque los turnos y las prestaciones.
Se agregó el filtro por centros. También se le agregó la posibilidad de "Seleccionar todos" para marcar todos los centros en conjunto.
Se quitó el listado de depósitos. Se migró a un formulario específico. (Auditoría de Depósitos)

• Módulo de Auditoría de depósitos: 

Es un nuevo módulo que listará todos los depósitos. Se puede filtrar por fecha. Una vez listado está la posibilidad de hacer visible los protocolos de laboratorio y los que están pendientes de devolución (por defecto traerá la grilla con estos datos ocultos).
Se podrá controlar y quitar el control de las prestaciones del turno.
Se podrá rechazar el turno.
Se podrá ver las prestaciones y la traza del turno.
ROMAN
• MEDICOS PREDETERMINADOS EN INFORME:
Funcionalidad para poder cargar médicos predeterminados en la firma de informes por agenda
Tanto carga a través de abm y tabla, cómo consiguiente lógica en el backend para que al momento de validar el informe, queden cómo prioritarios los médicos firmantes predeterminados por agenda.