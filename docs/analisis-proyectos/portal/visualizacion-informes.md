---
sidebar_label: 'Visualización de informes'
---

# Visualización de informes desde todos los medios

Visualización/Retiro de estudios:
El paciente puede visualizar sus estudios únicamente desde el portal de pacientes o desde la aplicación DIM. Tanto para los resultados de laboratorio como para los informes de estudios médicos el paciente ve una línea de tiempo en la cual se indica la fecha de realización del estudio, la fecha de entrega y el día de la fecha. Esto brinda mayor transparencia y previsibilidad para el paciente.  

Para informes médicos los parámetros a utilizar son:
- Fecha del turno [Fecha_Turno en MAESTRO_TURNOS]: Como fecha en la que se realiza el estudio
- Fecha de entrega (Fecha del turno + SLA) [INFORMES_ESTUDIOS]: Fecha en la que debe estar disponible el informe.
- Fecha del día: la fecha del día en que se ingresa al portal o TAG.

## Pantallas

***Desde la web (desktop)*** Se ven los informes como cards los cuales permiten ver o descargar el informe y acceder a las imágenes. Aparte se permite enviar el informe por mail. Cuando no está cerrado el informe se visualiza la línea de tiempo con las características ya mencionadas.

![Informes desde mobile](/img/informes-portal.png)

![Informes desde mobile](/img/informes-portal-pendiente.png)

***Desde aplicación*** Se muestra la misma información con otro formato debido a que el espacio es limitado y no el mismo que en la web.

![Informes desde mobile](/img/informes-mobile.png)

***Desde terminales de autogestión*** En este caso se pueden dar distintas situaciones:

- No está disponible el informe médico:
    - Esta a tiempo de ser cerrado (no hay demora): En este caso se le va a mostrar al paciente una línea de tiempo donde se observe que todavía estamos a tiempo de cerrar el informe. Permitimos avanzar a recepción?
    - Se encuentra demorado: Mostramos en la línea de tiempo que estamos demorados y que no cumplimos con la fecha de entrega del informe médico. 
- El informe médico está disponible
    - El paciente posee cuenta en portal de pacientes: Mostramos la opción de enviar un mail con el informe y link a imágenes. 
    - El paciente NO posee cuenta en portal de pacientes: Permitimos avanzar a recepción para explicación (speech) de como crear la cuenta y para que sirve o habilitamos una opción para hacerlo ahí (a tener en cuenta que se habilita mediante un envío de token al mail. Con lo cual puede no ser inmediato). No podemos permitir ingresar un mail porque es una falla de seguridad.
    - El paciente NO tiene mail cargado en el F6: Mandamos el paciente a la recepción para verificar su identidad y agregar el correo?

![Informes desde Terminales](/img/informes-terminal.png)

![Informes desde Terminales (Opciones)](/img/informes-terminal-opciones.png)

***Para laboratorio:***

- Fecha del protocolo [FechaEstudio en LAB_PROTOCOLO]: La fecha en la que se atiende al paciente y se hace el ingreso completo.
- Fecha de entrega [FechaEntrega en LAB_PROTOCOLO]: Esta fecha se calcula automáticamente con el parámetro de días de demora de los análisis.
- Fecha del día: la fecha del día en que se ingresa al portal o TAG.

Los casos y circuitos son exactamente los mismos que para los informes médicos. Con la diferencia de que cuando el informe de un estudio médico se encuentra validado, tenemos una acción adicional para ver las imágenes del estudio.