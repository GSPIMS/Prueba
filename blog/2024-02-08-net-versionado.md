---
slug: versionado net    
title: Versión NET 2024.02.08
authors: [gms]
tags: [netapp, versionado]
---

# Subido en esta versión:

- Al dar de baja una obra social ofrece eliminar los bloqueos relacionados a la OS.
- Agrego la información "Que habilita" en la grilla de perfiles.
- Cuando una determinación de laboratorio está inactiva el sistema avisará y no la cargará en el protocolo automáticamente. Se podrá visualizar el mensaje nuevamente desde el botón indicado. (Está limitado en la pantalla de laboratorio y en la pantalla del paso a paso).
- Al crear un día adicional verificará el día generado. Si está generado, deberá limpiarse los turnos libres. En el caso que se quiera crear un día adicional cuando no está creado el día no habrá inconvenientes.
- Corrección al cargar una OS que tiene un tipo de iva predeterminado. Ahora trae correctamente el tipo de iva asignado.
- El sistema verifica si el paciente es de primera vez a los dres que tengan configurado esta parametrización. En la búsqueda de turnos por especialidad y/o práctica no se ofrece turnos con el profesional.
- En la búsqueda de turnos de fkt, ahora aparece el centro donde se van a asignar los turnos.
- Se agrega a la cta cte del paciente la columna "Depósito" donde mostrará si el comprobante es uno.
- En el conteo de turnos de una agenda (ctrl f2), si hay un turno o más libres mostrará el texto como "Turnos Libres" en vez de estar vacío.
- En la pantalla de protocolo de laboratorio y en el presupuesto se mejoró la velocidad de la carga al querer abrir el buscador de obras sociales.
- Se podrá cambiar la nota del turno desde la agenda, independientemente si está cancelado.
- Cambio el nombre de los botones de Bloqueos de OS, cuando es Med-Esp se refiere a que toma el médico y la especialidad seleccionada en la grilla. Si es "Totales" muestra la del médico y la de todas sus especialidades, inclusive las inactivas.
- En la grilla de Obras Sociales ahora muestra correctamente el "Inactivar Obra Social" y el "Activar Obra Social, según corresponda.
- Corrijo la función de "Buscar como Particular" en el módulo de Kinesiología, cuando una OS no tiene pactada la prestación. (Antes salía un mensaje informando que no lo cubría, tenías que cambiar la OS desde el ctrl z y volver a abrir el alta de turnos kinesio).
- Habilito la opción "Buscar Primeros Disp.(Rosas)" para que puedan seleccionar dependiendo el uso. Al no tener tildada la opción traerá todas las opciones de turnos excepto la pintada en rosa(Más días salteados). Al hacer la búsqueda con la opción tildada traerá también los turnos "en rosa".
- Limpio los lbls que levantaba el abm de pacientes al cargar uno nuevo.
- Cuando se ingresa a un informe desde "Informes", "Prestaciones" ó el "Atención Paciente" si se trata de un informe preocupacional, no se mostrará si no tiene el derecho Informes > Ver Preocupacional. (Se debe agregar el derecho a los perfiles que lo requieran. (Recomendación: Asignárselo al perfil que informa estudios, para que ellos puedan ingresar.)