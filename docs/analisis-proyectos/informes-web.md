---
sidebar_label: 'Informes'
---

# Sistema de informes

Este sistema es mediante el cual los profesionales realizan los informes de las prestaciones médicas que se hacen a pacientes. Estos informes contienen el diagnóstico y la conclusión a la que arriban los profesionales visualizando la imagen o trazado del estudio.

En la actualidad hay 3 circuitos mediante los cuales los profesionales emiten informes:
- Búsqueda de turno: El profesional busca el ID del turno y se abre el informe o los informes del que se encuentran asociados al turno.
- Desde agenda de médicos: El profesional llama al paciente y desde el modal que se abre, ingresa a la pantalla de realización de informe.
- Listado de turnos por distribución: Los informes a realizar se listan en una tabla a la que el profesional ingresa. Desde este listado se van informando las prestaciones de los turnos.

## Parametrización:

Para indicar que se debe realizar un informe para un código de nomenclador, se debe indicar un tipo y un subtipo de informe para el mismo. 
- Tipo de informe [INFORMESTIPOS]: Se deben definir todos los tipos de informe a utilizar en base a la especialidades o modalidades de los estudios que se hacen. De este listado se van a asociar a los códigos de nomenclador.
- Subtipo de informe [INFORMES_ESTUDIOS]: Cada tipo de informe va a contar con uno o varios subtipos de informe. Este nivel se usa para poder diferenciar y categorizar las prestaciones según se quiera aplicar. de este campo va a depender la cantidad de informes que requiere un turno ya que si el turno tiene 2 prestaciones que coinciden en subtipo de informe, esto a a indicar al sistema que se tiene que hacer un informe. Si las prestaciones cuentan con distinto subtipo, van a generarse 2 informes que pertenecen al mismo turno.

Gran parte de la parametrización para el manejo de informes se maneja a este nivel . Al crear el subtipo se puede definir:

- SLA (Service level Agreement): Tiempo (en horas) en que el informe tiene que estar cerrado en base al objetivo de servicio definido.
- Tiene resultados: Algunos informes usan cuadro de resultados. Para usar esta funcionalidad, tiene que estar creado el cuadro de resultados por fuera del sistema y subido a la base de datos. El uso de estos cuadros de resultado se definen desde el código. No hay forma de parametrizarlo mediante el uso del sistema. 
- Tiene proforma: Si el médico que informa la prestación va a poder usar proformas (textos precargados) se tiene que seleccionar esta opción. Tienen que existir proformas cargadas para el subtipo de informe para que el médico pueda acceder a estas. Estas proformas se crean desde el menú “Proforma” en apartado “Informes”
- Tiene imagen: Aquí se indica si el estudio lleva imagen DICOM para que el sistema sepa si va a mostrar o no visualiazdores y si tiene que buscar o no imágenes en el PACS.
- Imagen obligatoria para cerrar informe: En el caso de que no se quiera permitir cerrar un informe sin encontrarse la imagen del turno, se tiene que usar esta marca. Esto permite una nueva etapa de control para que el médico confirme que la imagen del turno pertenece al paciente. Es importante ya que desde el portal de pacientes se pone a disposición el o los informes e imágenes.
- El informe se genera importando un PDF externo: Mediante esta opción, se indica que se puede subir un PDF externo que reemplace el informe a realizar. 

### Tipos y subtipos de informe

Para el correcto funcionamiento del sistema de informes se deben crear los tipos y subtipos de informe según las prestaciones que se hacen en la institución y el grado de apertura que se quiera manejar en cuanto a las prestaciones a informar. El tipo de informe es el tipo de prestación al que pertenece un código de nomenclador el cual representa su modalidad. Los subtipos de informes son todas las divisiones que se manejen para cada tipo. Esta apertura depende de que manejo se quiera hacer de los informes. La cantidad de informes que se deben hacer para un turno va a depender de la cantidad de códigos de nomenclador con distinto subtipo de informe se encuentren en el mismo. 
Ejemplo: 

<details>
      <summary>
        Ejemplo de turno con 2 informes
      </summary>
      <div>Un turno de ejemplo contiene 2 prestaciones: (códigos de nomenclador con sus tipos y subtipos a continuación) </div>
      <div>RESONANCIA DE HOMBRO - Tipo: RESONANCIA MAGNETICA - Subtipo: Musculo Esqueletico </div>
      <div>RESONANCIA DE CEREBRO - Tipo: RESONANCIA MAGNETICA - Subtipo: Neuro </div>
      <div> <br></br> </div>
      <div>Al contar con 2 prestaciones que difieren en el subtipo de informe, el turno va a contar con 2 informes a realizar. </div>
</details>

Esto afecta directamente el sistema de distribución automática ya que uno va a trabajar en las definiciones de profesionales que informan indicando que tipos y subtipos de informes va a informar cada uno de estos. Así se puede diferenciar entre quien informa Resonancias de músuculo esqueletico o quienes informan Resonancias 
de cardiacas. Es por esto que la definición de tipos y subtipos de informe no es estática y depende del funcionamiento esperando del sistema en general.

Los tipos y subtipos de informe se asocian al nomenclador que representa la prestación. En estos se puede seleccionar un tipo y subtipo de informe. Si el código de nomenclador no tiene información en estos campos, el sistema interpreta que no es una prestación que requiera de informe. 

### Médicos Predeterminados.

Esta configuración permite marcar como predeterminado a uno o 2 médicos para un tipo de informe, lo cual va a agregar la firma del médico cuando se valide o cierre un informe. En el cierre de un informe se agrega por defecto la firma y sello del médico que hace dicho cierre y la del médico que se encuentre agregado como un médico predeterminado si coincide el tipo de informe.

Nos faltan pantallas para poder manejar médicos predeterminados por tipo de informe y estudio [INFORMES_MEDICOSPRED]. Desde la base usamos esto para poder definir cuando sale la firma de un médico que no es el que cierra el informe

### Nomina de médicos

Este es el listado de médicos que aparecen en el margen izquierdo de los informes. Se pueden agregar médicos por tipo de informe para que aparezcan los médicos que pertenecen al servicio (especialidad o modalidad).

Nos faltan pantallas para poder manejar los médicos que aparecen listados en el servicio [INFORMES_NOMINAMEDICOS]

Aparte, hay otros parámetros (desde .net) que influyen en el circuito de realización de informes. Estos se encuentran en la configuración del sistema en el archivo [Configuracion.json] lo cual se puede editar desde el front end de .net. Está parametrización depende de cada estación de trabajo.
- Estación de informe: Si se tilda esta opción el sistema va a abrir la imagen del asociada al turno de manera automática. Buscando hacerlo en un segundo monitor si el puesto de trabajo tiene.
- Visualizador a usar: Cual es el visualizador por defecto que se va a usar desde la estación de trabajo que se está usando.
- Paths Visualizadores: Donde se encuentran los visualizadores que dependen de un instalable. 
- Imprime informe con Logo de centro: Si desde esta estación de trabajo se usa el logo del centro en la impresión de informes o no.

### Proformas

Las proformas [INFPROFORMA] se usan como un texto pre creado, que puede ser utilizado desde la realización del informe para que el médico ahorre tiempo de tipeo. Cuando el médico ingresa a la pantalla para hacer el informe, el sistema va a ofrecer las proformas que están asociadas al estudio al que pertenece la prestación que se está informando (sean proformas asociadas al usuario o globales). En el caso donde la prestación a informar cuente con una única proforma, se va a cargar automáticamente en el campo de texto, sin abrir un modal para seleccionarla. Si no hay una proforma para el tipo/subtipo de estudio que se está informando, se abre el informe sin una carga previa.

Estas se generan en base a:
- Nombre: El nombre con el cual se va a identificar la proforma.
- Tipo de informe: La proforma se va a ofrecer en base al tipo de informe seleccionado.
- Estudio Tipo: Sumado al punto anterior, se va a ofrecer para el estudio seleccionado. El listado de lo que se puede seleccionar en este apartado va a depender de lo seleccionado en el campo “Tipo de informe”
- Tipo Proforma [INFTIPOPROFORMA]: Se pueden definir distintos tipos de proforma según se requiera. En general manejamos Normal, Patológica u Otros. Así el médico selecciona si va a usar una u otra dependiendo del estudio que está analizando. En estaciones de trabajo donde el informe se abre automáticamente, el médico visualiza la imagen antes de indicar que proforma usar. Así sabe que tipo le conviene seleccionar.
- Texto: El texto que se va a cargar en el campo de texto del informe.
- Rota: NO SE USA. La idea es permitir tener proformas que conceptualmente sean lo mismo pero donde cambia el texto y así poder usar distintas proformas para el mismo diagnostico. Esto permite que no saga siempre el mismo texto para las mismas situaciones.
- Tiene opciones: NO SE USA
- Usuario [USUARIOS]: El médico al cual está asociada la proforma. Si se selecciona un usuario entonces esa proforma se va a mostrar únicamente al que se haya agregado. Sino, se muestra a todos los usuarios médicos.

### Distribución automática

La distribución automática asigna los estudios a informar a médicos en base a definiciones [INFLISTADEFS] y filtros [INFLISTADEFSFILTROS] creados para que cada médico cuente con un listado de informes a realizar y no tenga que buscarlos desde las agendas o esperar a que le pasen un listado generado a mano.

Estas definiciones relacionan un usuario con los tipos y subtipos de informe que este usuario puede informar, teniendo en cuenta otras variables que hacen a filtros que se van a tener en cuenta en la distribución. Las definiciones cuentan con:
- Fecha desde: Desde que fecha se toma activo el registro
- Fecha Hasta: Hasta que fecha se toma activo el registro
- Usuario: Usuario médico al que pertenece la definición
- Tipo de informe: El tipo de informe que se asocia al médico. 
- Tipo de estudio (subtipo). El o los tipos de estudio que se asocian al médico.
- Máximo diario: La cantidad máxima de informes que se van a asignar al usuario de dicha definición en un día.
- % Máximo diario: NO SE USA ACTUALMENTE. La idea de este campo es poder reemplazar cantidades absolutas con un porcentaje de lo que se informa. El problema con esto es que tenemos que definir la lógica mediante la cual podemos determinar la cantidad de informes que potencialmente se hacen en el periodo de tiempo en el cual se quiera asignar la limitación.
- Activo: Si se encuentra activo o no (revisar si tenemos fechas)
- Atiende adultos: Define si se van a encolar informes a realizar de turnos donde el paciente sea tenga edad mayor o igual a 18 años.
- Atiende pediátricos: Define si se van a encolar informes a realizar de turnos donde el paciente sea tenga edad menor a 18 años.
- Es Preinformante: No sería necesario si tenemos el derecho de preinformante. Revisar que usamos. La idea es que marquemos el informe como pre informado en caso donde informe un usuario de estos. 
- Listado para informes sin repartir: NO SE USA
- Atiende Urgente: Define si se van a encolar informes a realizar de turnos que se encuentren con la marca de URGENTE
- Atiende Prioritarios: Define si se van a encolar informes a realizar de turnos que se encuentren con la marca de PRIORITARIO.

Cuando un turno obtiene su estado de atendido [MAESTRO_TURNOS] se encuentra habilitado para el análisis de un servicio que va a revisar el tipo y subtipo de informe de las prestaciones del turno y controlar que exista una definición activa que contenga estos tipos de informe. De ser así, el servicio se encarga de verificar a quien se entrega dicho informe y si no se puede encolar a nadie, va a quedar en un registro de informes sin encolar para control de usuario supervisor.

### Filtros adicionales para la distribución automática

Cada definición cuenta con un filtro en donde se van a indicar otras condiciones que hacen a la elegibilidad de dicha definición a la hora de distribuir un informe. Este filtro se compone de:
- Desde: Fecha desde la cual se va a tener en cuenta este filtro adicional.
- Hasta: Fecha hasta la cual se va a tener en cuenta este filtro adicional.
- Centro: Si el profesional va a informar prestaciones de un centro especifico, se puede indicar en este campo.
- Agenda: Si el profesional va a informar prestaciones de una agenda especifica, se puede indicar en este campo. 
- Día y hora: Se tiene que indicar por día si el profesional está trabajando y en que horario. Esto es importante para el cumplimiento de los SLA de los informes a hacer. Si se encola un informe a un profesional que no está trabajando o que ya terminó de trabajar en el día, puede causar un problema en la demora del informe.Tener en cuenta que se puede usar únicamente 2 de las 6 columnas que aparecen en la definición de horas. Existen 6 columnas por si un profesional divide su horario laboral del día en hasta 3 franjas distintas con pausas en el medio.

### Proceso de distribución de informes

El sistema cuenta con un servicio que cada pocos segundos procesa las prestaciones de turnos que hayan obtenido recientemente el estado "Atendido" y recorre una lógica para determinar al listado de que médico o médicos se van a distribuir. Este proceso revisa cuales son los tipos o subtipos de informe de las prestaciones (códigos de nomenclador) que se encuentran en el turno y luego cuales son las definiciones de médicos creadas que podrían informarlas. 
Obtenido el listado, se empiezan a comprar los filtros básicos y avanzados de cada definición contra la información del turno y/o paciente y así se logra definir cual o cuales son los aquellos que pueden hacer el informe. Si existe más de un médico que puede informar la prestación, se controla cuantos informes sin hacer tienen y se asigna a quien menos tenga (respetando siempre los limites establecidos).

Un filtro importante a tener en cuenta son los días y horas de trabajo. Esto es fundamental porque el sisntema tiene que saber si un profesional va a llegar a cumplir con el SLA (service level agreement) del estudio o no. No sería posible asignar un informe a hacer de un estudio con SLA de 48h horas a un profesional que empieza a informar dentro de 48 horas.

Si no hay una definición como candidato para distribución de un informe antes del vencimiento del SLA de su estudio, el informe va a quedar en un listado sin distribuir. Esto es para que un usuario supervisor controle estos informes sin distribuit y lo haga manualmente. 

Por defecto, todos los informes de un mismo turno van al mismo profesional. Esto es un parámetro dentro de la tabla "Centros" en el apartado "Archivo". Al destildar esta opción, puede que informes del mismo turno se distribuyan entre distintos usuarios.

### Parametrización de agenda para indicar que el profesional informa

Como vimos más arriba, hay profesionales que informan directamente de la agenda. Al llamar al paciente se abre un modal (ventana) donde se permite al profesional ingresar al sistema de informes. Para que la agenda redirija al ssitema de informe se tiene que configurar la agenda utilizando determinados parámetros. Para conocer más de este tema se puede ingresar a la definición de proyecto de agendas. 

## Derechos:

Los derechos que afectan al sistema de informes son:
- 5.1: Realizar informes y grabarlos.
- 5.2: Configurar las definiciones y filtros de la distribución automática de informes
- 5.3: Transferir un informe desde un médico asignado a otro que puede hacer dicho informe.
- 5.4: Permite al médico transferir un informe asignado a si mismo a otro usuario médico.
- 5.5: Habilita hacer informes como pre informantes (no es una marca que se use hoy)
- 5.6: Creación , edición y baja de proformas.
- 5.7: Validación y cierre final del informe. Aplicando la firma del médico que lo cierra
- 5.8: Permite quitar la validación de un informe, abriéndolo para modificaciones y quitando la visualización desde portal de pacientes y HC.
- 5.9: NO SE USA
- 5.10: Permite a un usuario acceder a informes que se encuentran encolados a otros usuarios médicos.
- 5.11: Permite a un usuario guardar informes que se encuentran encolados a otros usuarios médicos.

## Urgentes/Prioritarios:

Desde la agenda del día, así como de las pantallas administrativas de manejo de informes, se puede determinar si un informe es urgente o prioritario [INFLISTACOLAS]. Esto va a pintar el registro en los listados de informes de rojo o amarillo respectivamente. Por el momento es solamente funcional para los informes que se asignan por distribución automática. Cuando un profesional selecciona ingresar a informar un turno que no es prioritario ni urgente y en el listado se encuentra uno con estas caracteristicas, se le recuerda al profesional que tiene un informe con más importancia para hacerlo antes.

## Tablas usadas:

| FILAVIRTUAL    | Descripción |
| ----------- | ----------- |
| MAESTRO_PRACTICAS      | Esta tabla cuenta con los códigos de nomenclador de los turnos y tienen definición de tipo y subtipo de informe |
| INFORMES   | Esta tabla contiene todos los informes creados |
| INFORMESTIPOS   | Definiciones de tipos de informes que se van se asociar a códigos de nomenclador |
| INFORMES_ESTUDIOS   | Definiciones de subtipos de informes que se asociar a códigos de nomenclador |
| INFORMESAENCOLAR   |  |
| INFESTADOS   |  |
| INFLISTACOLAS   |  |
| INFLISTADEFFILTROS   |  |
| INFLISTADEFS   |  |
| INFORMEGENERARPDF   |  |
| INFORMES_ABUSCAR   |  |
| INFORMES_MEDICOSPRED   |  |
| INFORMES_NOMINAMEDICOS   |  |
| INFPROFORMA   |  |
| INFSINENCOLAR   |  |
| INFTIPOPROFORMA   |  |
| INFORMES_MF   |  |
| INF_CODDIAGS   |  |
| USUARIOS   |  |
| WEB_APIS   |  |

## Circuito de uso por usuario médico (Realización de informe):

Este circuito va a depender del perfil que tenga el usuario médico que se encuentra haciendo el informe. Solo puede validar un informe quien tenga el derecho [5.7] asignado. Si el informe se encuentra encolado a un médico (registro en [INFLISTACOLAS]) no va a permitir abrir dicho informe a otro usuario que no sea el asignado, a menos que el usuario que lo esté abriendo cuente con el derecho para poder acceder a informes que se encuentran asociados a otro usuario [5.10].

El médico ingresa en la pantalla de realización de informe desde cualquiera de los orígenes antes descritos e inicia el circuito:

1. Detectamos si el turno cargado cuenta con uno o más informes a realizar. Esto depende de la cantidad de prestaciones cargadas en el turno y si estas comparten o no el tipo y subtipo de informe. Si todos los códigos comparten esta información el informe es uno solo. Si difieren, la cantidad de informes a realizar va a depender de la cantidad de subtipos de informe. Es probable que tengamos varias prestaciones que varíen en tipo de informe lo cual también va a requerir de múltiples informes.
Ej: Turno contiene 2 prestaciones con tipo RMN y subtipo RMN de Rodilla > 1 informeTurno contiene 2 prestaciones donde la primera tiene tipo RMN y subtipo RMN de Rodilla y segunda con tipo RMN y subtipo RMN de Hombro > 2 informes
:::info[Informes urgentes/prioritarios]
Si el médico informa desde un listado de informes a hacer y selecciona un turno sin urgencia ni prioridad, el sistema va a buscar siempre si existe un registro urgente o prioritario. De ser así, va a informar que existe un informe con caracter de urgencia. Es un aviso en concepto de advertencia, ya que el médico puede avanzar igualmente con el informe seleccionado.
:::
2. Búsqueda de imagen o imágenes del turno: Buscamos que el turno cuente con una o más imágenes y en caso de ser así tenemos que revisar la parametrización cargada en “Parámetros” [Configuracion.json] en el sistema local. Si está en verdadero la marca de “Estación de informes” entonces tenemos que abrir la imagen automáticamente en el visualizador que se encuentre seleccionado en esta misma pantalla. Podemos tener un caso donde en el mismo turno contamos con distintas series de imágenes y de ser así, tenemos que ofrecerle al profesional a través de un modal que serie es la que desea abrir, ya que no podemos nosotros automáticamente identificar cual es la que corresponde al informe que se abrió (para el caso de múltiples prestaciones con diferentes tipos de estudio).Si la estación cuenta con 2 monitores, la imagen se va a abrir en un segundo monitor.
3. En la apertura de cada uno de estos informes a realizar, buscamos las proformas que se encuentran asociadas al tipo de estudio de la prestación del informe. Si las proformas cuentan con un usuario asociado que NO es el que está informando, no se van a mostrar. Si el tipo de estudio cuenta con una única proforma, no se muestra modal para seleccionar sino que se carga automáticamente en el espacio de texto del informe.Si se encuentran varias proformas, mostramos un modal donde el médico va a seleccionar la que corresponda.
4. Con imagen abierta y proforma seleccionada, el profesional hace la edición necesaria en el campo de texto. Al terminar el informe lo graba (guarda) y se corre el circuito de cierre de informe.

:::danger[Logica de validación de diagnóstico]
En el contrato de las Obras Sociales se puede indicar si la prestación que se informa requiere de una validación del diagnóstico al que arriba el médico según correponda para el informe que se hace. 
    - Si el informe requiere de validación de diagnóstico tenemos grabar el informe y solicitar se indique el tipo de diagnostico a validar [INF_CODDIAGS]. Se valida el diagnostico y luego se pregunta si se desea cerrar el informe definitivamente. Si la validación vuelve rechazada no se puede avanzar con el cierre del informe. Si falta la validación online de la prestación, no se puede hacer la validación del diagnóstico.
    - Si el informe no es requiere dicha validación, se graba el informe y se pregunta si se desea cerrar el informe.
:::

5. Cerrado el informe se va a mostrar en la HC del paciente. Una vez pasado el tiempo configurado [REVISAR DONDE], se va a enviar al paciente vía mail y a habilitar su visualización desde el portal.

***El informe no se puede validar si no se cumplen con las siguientes condiciones:***
- El usuario médico tiene el derecho 5.7 asignado.
- El turno no tiene imágenes relacionadas en el caso de un subtipo de informe que requiere la imagen para permitir validarlo.
- Las prestaciones del turno requieren validación online y no están validadas o se validaron pero con estado rechazado.
- La obra social del turno requiere validación de diagnóstico y no se cumplió con este paso o se obtuvo un rechazo en dicha validación.

## Quitar la validación de un informe

Todo informe validado se puede desvalidar de manera individual para permitir hacer modificaciones sobre el mismo. Solamente se va a poder reabrir si se cumple con una de las siguientes condiciones:
- El usuario que quiere quitar la validación del informe es quien lo validó y no se supero el tiempo limite seteado para la reapertura (este tiempo se puede configurar desde la tabla Empresa) 
- El usuario cuenta con el derecho que permite quitar la validación del informe (derecho 5.8)

En ambos casos se quita la validación del informe y con esto se vuelve a permitir hacer ediciones. El profesional tiene que volver a validar el informe para que el informe queda nuevamente validado. 

## Entrega del informe al paciente

Una vez cerrado el informe se va a poner a disposición del paciente mediante el portal de pacientes y enviandolo por mail. También permite acceder al informe desde el sistema de turnos e imprimirlo.
El mail hacía al paciente se va a enviar una vez transcurrido el tiempo designado para el envío de informes luego de validado. Lo mismo para el portal de pacientes. Solo se podrá acceder una vez que se cumpla con el plazo mencionado.

:::info[Parametrización de tiempo]
El tiempo en el cual se pone a disposición el informe en el portal o mediante el envío de mail es parametrizable. Se modifica desde la tabla informe en el apartado "Archivo". El campo es "Delay mail informe" 
:::