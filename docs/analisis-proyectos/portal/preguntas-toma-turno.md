---
sidebar_label: 'Preguntas en circuitos'
---

# Preguntas en toma de turnos para circuitos

Solicitud: Se solicita agregar preguntas (como en circuitos actuales) a lo que sería un nuevo circuito para UNIDAD DE TIROIDES.

Ticket:ISMNP-90: Preguntas previas a la asignación de turnos - UNIDAD DE TIROIDES
EN ANÁLISIS
 

Texto de solicitud:

Buen día,
Necesitamos crear un nuevo circuito en cuanto a las preguntas previas que se realizan al otorgar un turno. Tengo entendido que las mismas se disparan al asignar un "circuito" dentro del ex grupo médicos (ahora "Def. Prestaciones"). En este correo me voy a centrar en el circuito referido a UNIDAD DE TIROIDES

Circuito Tiroides

Es un circuito nuevo, aún no creado. Se debe crear el circuito y asignar el mismo desde grupo médicos.

Esto hará que al otorgar un turno en la opción UNIDAD DE TIROIDES se levanten ciertas preguntas desde el sistema, la web y la app.

Estas preguntas fueron confeccionadas por las coordinadoras de clínica médica y actuarán como una especie de filtro para acceder al turno

Lógica: Con UNA SOLA cuya respuesta sea SI, ya puede acceder al turno. NO accederán al turno si TODAS las respuestas son NO.

Son 4 preguntas

1- Tiene usted antecedentes personales de alguna enfermedad tiroidea ya diagnosticada ?
2- Tiene ud algún estudio de laboratorio que evidencie hormona /s tiroideas fuera del valor normal? (Ej TSH, T4L, Anti TPO)
3- Tiene ud alguna ecografía tiroidea que evidencie una imagen anormal? (Ej Nódulo /s Quiste/s)
4- Estuvo anteriormente o está actualmente medicado con Levotiroxina?

Respuesta para propuesta de solución integradora:
Objetivo: 
Permitir a usuarios desde el sistema indicar cuando un circuito usa minutos y cuando un circuito requiere de preguntas a hacer al paciente. El usuario va a definir también cuales son las preguntas que se van a hacer y si permiten avanzar o no.

Análisis de la solución:
A nivel de circuito se tiene que poder identificar si el mismo usa calculo de minutos y si tiene preguntas a hacer al paciente. Para el caso de los minutos, se debe de poder indicar cual es la ecuación que se va a realizar para el calculo de los mismos cuando se agregan nuevas prestaciones al turno las cuales tienen cada una minutos asignados a nivel de nomenclador. Para las preguntas se tiene que poder definir:

Si el circuito lleva o no preguntas a hacer al paciente.

En caso de que se hagan preguntas, se deben definir cuantas y cuales son (indicando el texto de la pregunta).

Para cada una de las preguntas se va a indicar si existe una pregunta adicional hija que dependa de la misma, indicando cual es la pregunta nueva.

Para cada una de las preguntas se tiene que poder definir si se avanza con la respuesta positiva o negativa.

Poder definir cual es el mensaje de error por el cual no se avanza a la oferta de turnos para el circuito.

Identificar si la pregunta afecta la definición de agendas asociadas a la prestación e indicar cual/es son las agendas objetivo para la respuesta a una pregunta.

Para lo anterior hay que crear uan tabla nueva para el manejo de las preguntas de circuitos [CIRCUITOPREGUNTAS] donde se agregue:

ID del circuito al que pertenece.

Texto de la pregunta.

Si permite avanzar con el si o con el no.

Si deriva a otra pregunta (si tiene un padre no aparece por defecto).

Limita agenda dependiendo de respuesta positiva o negativa.

Agendas a ofrecer según marca anterior.

Aparte tenemos que agregar a la tabla [CIRCUITO] campos para:

Definir si se usa calculo de minutos según prestaciones agregadas al turno.

Se realizan preguntas a los pacientes a la hora de avanzar a la oferta de turnos.

Texto de error al no poder avanzar con la oferta de turnos.