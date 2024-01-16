---
sidebar_label: 'Toma de turnos'
---

# Toma de turnos desde Terminales

Ampliar la funcionalidad de las terminales para permitir que los pacientes tomen turnos desde la terminal de autogestión tomando en cuenta los parametros que actualmente se consideran para la toma de turnos desde el portal.

:::info[Objetivo]
**Permitir al paciente tomar turnos de consultas, prácticas y laboratorio desde la terminal de autogestión, sin necesitar interacción con representantes que se encuentran en la recepciones de DIM. Aumentar el porcentaje de gente que usa las terminales.**
:::

## Funcionalidad

Ampliar la funcionalidad de las terminales para permitir que los pacientes tomen turnos desde la terminal de autogestión tomando en cuenta los parametros que actualmente se consideran para la toma de turnos desde el portal.

Desde la terminal de autogestión vamos a cambiar la lógica al botón de “Necesito turno” donde el paciente va a seleccionar para que desea tomar un turno y luego avanzar en el circuito de toma de turno hasta su asignación. Este circuito va a ser el mismo que manejamos hoy en el portal web de pacientes, tomando en cuenta todos los filtros, bloqueos y topes que hoy se aplican en la búsqueda de turnos a ofrecer. 

## Circuito de toma de turno

El primer paso va a ser definir si se desea tomar un turno de:
- Consulta: Mismos resultados que en el portal.
- Estudio Médico Mismos resultados que en el portal.
- Laboratorio: Se tiene que seleccionar para que se desea tomar un turno de laboratorio dentro de los grupos de estudio que se encuentran cargados.

:::note[Importante]
Tenemos que tener en cuenta que el proyecto de pago y subida de orden obligatoria van a afectar a las terminales, con lo cual vamos a tener que agregar estos pasos para un preaviso antes de empezar a la búsqueda (aclarando que se usa MP) y agregar los pasos a la hora de 
:::

La principal diferencia con el portal es que tenemos que contar con una pantalla de búsqueda de registros (grupos médicos o agendas) para poder navegar los resultados de las prestaciones que coinciden con la búsqueda realizada. Para dicha búsqueda tenemos que crear un teclado que se pueda mostrar y ocultar, apareciendo desde el margen inferior de la pantalla. Este teclado va a aparecer siempre que se seleccione un campo donde se pueda ingresar texto. Para no tapar los elementos listados, tenemos que hacer que suba el banner superior de la pantalla cada vez que sube el telcado, transicionando todos los elementos a una posición más alta.

Al ingresar cada letra, se tienen que filtrar los elementos en pantalla según coincidan con el texto ingresado (mínimo de 3 caracteres) y se van a listar en arrays de registros para compaginación. 

La navegación de las prestaciones se hace mediante páginas en las que el paciente va avanzando a medida que necesita acceder a más registros. Los paneles de los monitores ELO que usamos para las terminales no cuentan con la capacidad para hacer un desplazamiento suave (soft scrolling) por lo que esta funcionalidad no va a servir para el paso del circuito donde el paciente debe buscar la prestación a seleccionar.  Con la búsqueda por frecuenta se apunta a que el resultado que busca el paciente se encuentre siempre en la primer página.

:::note[Prueba funcional]
Tenemos que probar usar gestures para desplazar verticalmente el listado de prestaciones y de turnos como se hace en el teléfono. Estimar horas de desarrollo para saber cuanto estaríamos agregando en una prueba funcional que va a dictar la navegación futura de las terminales. 
:::

Al seleccionar un registro, se buscan los turnos con la misma lógica con la que funciona el portal y se muestran los resultados con el uso de cards (igual que en portal). La pantalla va a contar con un botón para poder acceder a los filtros, los cuales se van a abrir desde el margen derecho de la pantalla. El paciente puede interactuar con los filtros (mismos que en el portal) y debe tocar un botón "Aplicar" para cerrar este apartado y aplicar los cambios. 
Para tomar el turno el paciente tiene que seleccionar su card. Este va a contar con un botón interno aunque tenemos que aceptar que toque en cualquier lado dentro de los limites del card. Como se explico más arriba, al igual que con la pantalla de búsqueda de prestaciones, tenemos que hacer que los turnos ofrecidos se naveguen mediante botones en los márgenes de la pantalla.

Se solicita que para las prácicas avancemos con los pasos de subida de orden, firma y carga del médico solicitante. Esto tiene que ser parametrizable para poder excluirlo del circuito. En caso de que se encuentra activado, es el mismo proceso de control de orden que se hace actualmente cuando un paciente se ingresa por las terminales de autogestión para poder recepcionar su o sus turnos con excepción de asignación de presente y sin realizar validaciones online ni verificar afiliados. El control positivo va a dar un mensaje de agradecimiento y de confirmación de asignación de turno. En caso de que exista un rechazo **????**

Para lo anterior, tenemos que modificar la pantalla de control de órdenes indicando si el control es para un turno del día o de un día futuro. Así desde la tabla se pueden identificar cuales son los controles para el presente y cuales son para un turno futuro.

:::warning[Texto a definir]
Indicar que texto se va a usar para indicarle al paciente que el turno se tomo con exito y subió todo lo requerido así para cuando se le rechace la orden.
:::

Terminado el circuito de toma de turno, preguntamos al paciente si necesita hacer algo más o no. Esto va a manejar un timer que se ve en la opción "No" donde si se cuple con el tiempo del timer, se cierra sesión. Si toca que "No", se cierra sesión y se vuelve al login. Si toca en el botón "Si" se vuelve al menú principal para que avance con lo que necesite.

## Pantallas

Las pantallas que tenemos que agregar a la terminar de autogestión son aquellas necesarias para poder cumplir con el circuito de toma de turnos. El proceso de base es identico al del portal o aplicación pero con diferencias que lo adecuan a interacción con un monitor touch como el de las terminales.

**Opción "Necesito turno"**

![Necesito turno](/img/terminales/Necesito_turno.png)

**Turnos más solicitados previo a búsqueda de consultas médicas**

![Turnos más solicitados](/img/terminales/Oferta_consutlas_más_solicitadas.png)

**Búsqueda de turnos con botones**

![Búsqueda sin texto](/img/terminales/Búsqueda_mediante_texto.png)

**Búsqueda con caracteres ingresados**

![Búsqueda con texto](/img/terminales/Búsqueda_al_ingresar_texto.png)

**Búsqueda sin teclado (para ver más elementos)**

![Búsqueda sin teclado](/img/terminales/Búsqueda_teclado_invisible.png)

**Oferta de turnos**

![Oferta de turnos](/img/terminales/Oferta_de_turnos.png)

**Oferta de turnos en página 2 o más**

![Oferta de turnos 2](/img/terminales/Oferta_turnos_opción_volver.png)



