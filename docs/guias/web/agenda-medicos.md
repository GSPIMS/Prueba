---
sidebar_label: 'Agenda de médicos'
---

# Agenda de Médicos

La agenda de médicos es la plataforma basada en el trabajo de los profesionales, listado todos los turnos que se encuentran asignados, para que esté pueda ir llamando a los pacientes que se encuentran esperando a ser atendidos.

La nueva agenda se desarrollo en base a un plan de cambio y mejora de varias plataformas de la clínica. Al ser una de las herramientas más chicas, es la primera en ser lanzada. En los próximos meses vamos a estar actualizando otras herramientas como la Historia Clínica o el sistema de informes.

En la migración se mantienen la mayoría de las funcionalidades que existen en la agenda actual, mejorando la visualización y arreglando varios problemas chicos que generan problemas constantemente. En el diseño algunos elementos o botones cambiaron de lugar a fin de tener un layout más armónico.

Las funcionalidades que se mantienen son:
- Información del paciente: Muestra datos básicos del paciente.
- Notas: Para acceder a las notas del turno que se ingresan desde recepción o sector administrativo
-Llamar paciente: Mediante este botón (bocina) se llama al paciente. Se lo puede llamar desde la tabla o mediante el botón “Llamar” que se encuentra en la tarjeta que indica el próximo paciente a llamar en el lado derecho.
- Evolucionar: Una vez que el paciente se encuentra en el consultorio se usa este botón, ya sea desde la tabla como desde la ventana que se abre al llamar, para ingresar a la Historia Clínica.
- Ver HC del paciente (sin evolucionar): Se ingresa a la Historia Clínica para ver evoluciones del paciente, sin ir a la evolución actual.
- Ver imagenes: Muestra todos los estudios del paciente con descripción de la especialidad del estudio y fecha de realización
- Ver Orden o prestaciones: Muestra orden del turno (para prácticas) o prestaciones cargadas en el mismo.

La nueva agenda se ve de la siguiente manera:

![Agenda de médicos](/img/agenda-medicos.png)

Las funcionalidades básicas para el llamado de los pacientes y el acceso a la evolución en Historia Clínica son idénticas a la agenda actual. 

Los principales cambios son:
- Para cambiar la agenda o salir de la misma, se tiene que hacer mediante el botón “Hamburguesa” (3 líneas horizontales) en el margen superior derecho de la agenda. 
- Se agrega una estadística de la agenda para saber el total de pacientes que tienen un turno en la agenda del día, pudiendo observar cuantos se encuentran esperando, cuantos todavía no llegaron y cuantos se atendieron.
- El botón del cierre de atención (“Finalizar la atención”) pasa a estar en el margen inferior derecho para no seguir teniendo problemas donde quedaba por debajo del listado de turnos y no se veía.
- Se cambio el lugar de los botones para acceder a la cartilla o a los números de internos. Estos se encuentran en el margen inferior derecho. También se los puede encontrar en el menú “Hamburguesa” del margen superior derecho, a la derecha del nombre de uno.
- La función de asignación de turnos se modifico. Al tocar el botón de asignación de turnos, el sistema va a llevar a otro sitio donde se buscan turnos para la agenda de uno y se permite asignar un sobreturno según se necesite. Esta plataforma es la denominada Portal LITE:

![Agenda de médicos](/img/portal-lite.png)

Al ingresar desde la agenda, se carga automáticamente en el campo de búsqueda la misma. Lo único que tenemos que hacer es tocar el botón “Siguiente” y así accedemos a la oferta de turnos actual. Como se ve en la siguiente pantalla:

![Agenda de médicos](/img/portal-lite-oferta.png)

En esta pantalla se ven todos los turnos ofrecidos según la disponibilidad en agenda. Se pueden filtrar usando los filtros superiores, o buscar más con el botón “Buscar más turnos”. Si se desea agregar un turno a un paciente un día que se encuentra completamente ocupado, se puede usar la función “Sobreturnos”. Esta nos va a abrir la siguiente ventana:

<div style={{textAlign: 'center'}}>

![Agenda de médicos](/img/portal-lite-asignacion.png)

</div>

En esta ventana, seleccionamos un día de los habilitados en agenda (aparecen en naranja en el calendario) y luego un horario. Al ingresar todo lo anterior, tocamos “Continuar” y así asignamos el turno como un sobreturno.
