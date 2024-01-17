---
sidebar_label: 'Desactivar validación'
---

# Desactivar requisito de validación online

:::info[Objetivo]
**Permitir a un operador desactivar el requisito de validación online del circuito del presente para cuando la validación se encuentra caída e irrumpe con el uso de las terminales**
:::

Las caídas de los servicios de validación online afectan de manera directa el circuito de la terminal ya que todos los pacientes de la obra social que se ve afectada por una caída de este tipo van a ser enviados directamente a punto de control. Para resolver esto necesitamos una funcionalidad donde un usuario pueda indicar la caída del servicio validador de una obra social y esto permita avanzar al paciente para asignar el presente del turno cuando corresponda. El objetivo de esta mejora es entonces que no se frene al paciente por un problema con la validación online.

Cada obra social que valida online tiene indicado un método de validación online (ONLINEVALIDAPOR en OBRASOCIAL_PARAMETROS) lo cual nos indica cual va a ser el canal de comunicación a utilizar a la hora de validar prestaciones de turnos de estas obras sociales. Cuando se cae la validación por un problema externo a la institución se tiene que indicar sobra la obra social sobre la que se confirma el problema. Para esto el usuario va a ingresar a “Manejo de validaciones” y crear un nuevo registro que va a representar la baja de solicitud de validación online para la obra social seleccionada. Esto tiene que disparar una búsqueda de todas las obras sociales que compartan el mismo método de validación online y generar dichos registros.

Cuando el paciente se esté recepcionando desde las terminales y llegue al paso de la validación online, se tiene que revisar si se cuenta con un registro activo para la obra social y de ser así revisar que: **El paciente cuenta con un turno con estado de ATENDIDO (=1) en los últimos 60 días desde el día en que se está recepcionando.**

Si se cumple con la condición mencionada, se va a obviar el paso de validación online requerido y avanzar al pago (de tener un pago definido por contrato) o a asignar el presente. El turno tiene que entrar en el circuito de validación automática posterior para que se vuelva a probar validar más adelante. 

Tenemos que crear una ventana en donde se puedan crear y eliminar estos registros temporales los cuales se conforman de la siguiente data:

- Obra social
- Fecha y hora de alta
- Fecha y hora de baja
- Comentario

Si la obra social no valida online o tiene indicado validación interna, no es posible guardar el registro que se está queriendo crear.

En la ventana de alta (de registro) se tienen que mostrar todas las obras sociales afectadas por la generación del mismo. Vamos a traer esta información de buscar las obras sociales que tengan el mismo método de validación que la que se agrega al mismo. Para cada una de estas obras sociales se tiene que poder indicar si se agrega un registro para saltear la validación online y la posibilidad de indicar si pedimos o no autorización y si es un código único o no. En caso de hacerlo, se habilita un campo para indicar cual es el número de autorización que se va a usar. Si se requiere autorización pero no se indica un único código, la terminal va a tener que mandar a punto de control a los pacientes de esa OS. En conclusión, cada obra social que comparte el mismo método de validación puede llegar a tener una definición distinta para lo que hace al circuito de saltearla.

Adicionalmente vamos a agregar un espacio de texto libre para escribir un comentario el cual se va a usar para indicar por que se está creando el registro y referenciar la comunicación realizada con el servicio de validación o la obra social de donde deriva el problema por el cual se usa esta funcionalidad.

Para dar de baja manualmente un registro el usuario se debe parar en uno activo y tocar un botón “Desactivar”. Al igual que con el alta, tenemos que revisar todas las OS que compartan el método de validación y preguntar si se quieren dar de baja todos esos registros. Si se responde que si entonces se dan de baja todos los registros y si la respuesta es que no, solamente el que se encuentra seleccionado. (podemos usar multiselección??)

Aparte de la baja manual, estos registros se van a dar de baja automáticamente al pasar X minutos con lo cual tenemos que setear automáticamente una fecha y hora de baja desde su alta. Estos minutos tienen que ser parametrizables desde la tabla EMPRESA para que puedan hacer las modificaciones necesarias en base a cuestiones impredicibles en la etapa de análisis de este proyecto.

Agregar traza para el alta y la baja manual de estos registros.

Reporte:

Vamos a crear un reporte donde se puedan ver todos los registros creados con la siguiente data:
- Obra Social
- Método de validación
- Fecha de creación
- Fecha de baja
- Usuario de alta
- Usuario de baja
- Comentario

Este reporte se tiene que poder filtrar por fecha desde y hasta no pudiendo traer registros de más de 2 meses