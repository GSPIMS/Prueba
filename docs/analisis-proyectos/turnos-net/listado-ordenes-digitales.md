---
sidebar_label: 'Órdenes digitales (tabla)'
---

# Listado de órdenes digitales generadas desde Historia Clínica

:::info[Objetivo]
**Contar con una tabla donde se puedan ver todas las órdenes de prestaciones solicitadas por médicos desde Historia Clínica para poder hacer seguimiento de aquellos pacientes que tengan órdenes solicitadas sin asociación a un turno** 
:::

***Importante: Definir template de órdenes de prácticas. Podemos usar el mismo que para laboratorio.***

En esta tabla se ven todos los registros de órdenes generadas por médicos desde las evoluciones a sus pacientes. Se listan tanto órdenes de laboratorio como órdenes de turnos. La tabla se compone de las siguientes columnas:
- Fecha: Fecha de creación de orden.
- Prestación: Indicar si es una orden de laboratorio o de prácticas.
- Especialidad: Especialidad de la o las prestaciones del turno (código de nomenclador perteneciente a la definición de prestación)
- Médico: Médico solicitante o aquel que generó la orden
- Nombre paciente: Nombre del paciente sobre el cual se generó la orden (paciente al que corresponde la evolución donde se generó la orden).
- DNI Paciente: Dni del paciente
- Mail: Mail que figure en el campo de EMAIL del paciente.
- Número de teléfono: Teléfono que figura en el campo TELEFONOMOVIL del paciente.
- Turno: ID de turno de consulta, práctica o laboratorio (según corresponda). Este campo es la indicación de si la orden se encuentra asociada a un turno o no.
- Fecha de turno: Fecha del turno tomado en caso de que haya un turno relacionado.

Por defecto la tabla se va a ordenar por fecha de creación de registro y luego por DNI de paciente.

## Funcionalidad

Como otras tablas del sistema .net, se tienen que respetar todas las funcionalidades que estas tienen. Las acciones por encima de la tabla son:
- Salir: Se cierra la ventana
- Información del paciente: Se abre el modal de información del paciente (F6)
- Asignar turno/Información del turno: Dispara la búsqueda de turnos tomando en cuenta las prestaciones que se encuentran en la orden solicitada para el caso de prácticas (definición de prestaciones o ex grupos médicos) o para el protocolo generado en el caso de laboratorio.
- Ver orden: Se muestra el PDF de la orden digital generada.

Los registros se deben mostrar con un fondo rojo cuando se listen y la direfencia entre el campo fecha (fecha de creación) y la fecha actual superen los 7 días.

### Filtros

La tabla tiene que contar con los siguientes filtros adicionales a los nativos de la tabla (también por encima de esta):
- Fecha desde: Fecha a partir de la cual se van a listar registros en la tabla.
- Fecha hasta: Fecha a hasta la cual se van a listar registros en la tabla.
- Prestación: Para indicar si se listan prácticas o laboratorio en la tabla. 