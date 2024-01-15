---
sidebar_label: 'Guardado de firma (Px)'
---

# Almacenamiento de firmas de pacientes en base de datos

:::info[Objetivo]
**Almacenar las firmas digitalizadas de los pacientes para que no tengan que ingresarla cada vez que la necesitamos y así quitar un paso de varios circuitos como la recepción o la asiganción de un turno de prácticas.** 
:::

## Funcionalidad

Se solicita empezar a almacenar las firmas de los pacientes en los momentos donde se solicita en el circuito que el paciente se encuentra tarnsitando (ej.: tomando un turno de prácticas y subiendo la orden). Los 3 circuitos donde solicitamos la firma del paciente son:
- Desde el portal de turnos o aplicación en la toma de turno de un estudio médico o de laboratorio. En ambos casos, luego de subir la orden, solicitamos al paciente que ingrese y confirme el envío de su firma.
- Desde las terminales de autogestión al momento de subir la orden para control. Cuando el paciente se está recepcionando desde una de las terminales, el paso siguiente a subir la orden del estudio es ingresar su firma.
- Desde puestos de recepción a través de la aplicación de toma de firma de pacientes. Esto se puede hacer para cualquier orden subida en un turno o para protocolos que se originan de una orden digitál solicitada por un profesional desde la historia clínica. 

Para todos los casos anteriores tenemos que agregar una opción donde al confirmar la firma, vamos a preguntarle al paciente si podemos guardarla para ser usada a futuro en las instancias mencionadas o en potenciales próximos desarrollos donde se interactue con la firma del paciente. Esto nos exige almacenar la respuesta del paciente para poder determinar quien autoriza a usar su firma y quien no ya en cada circuito el sistema tiene que saber si va a solicitar o no la firma.

:::note[Dudas a resolver]
**Que pasa si el paciente indica que no quiere que guardemos su firma? Le seguimos preguntando? Permitimos hacerlo desde su perfil en el portal? Le preguntamos a un representante de DIM cuando abra el F6 cada vez que eso pase para que hable con el paciente?**
**Seguimos trabajando con la marca de solicitud de firma por obra social? Revisamos siempre primero este parámetro? Para una OS que no solicita firmas, ni ofrecemos el almacenamiento a los pacientes que tengan dicha OS?**
:::

# Circuito para almacenamiento de firma

El circuito va a ser el mismo para todas las instancias donde se le puede estar solicitando la firma al paciente. Una vez que el paciente hace su firma en el dispositivo con el que interactua, se le va a preguntar mediante un modal si desea guardar su firma en DIM para ahorrar tiempo cada vez que se la tengamos que solicitar. Al seleccionar que si, avanza con el circuito en el que estaba y generamos el registro en base de datos para la firma (guardando el archivo en multimedia). Si selecciona que no, también avanza con el circuito en el que se encontraba y no se guarda el registro mencionado.

# Circuito para uso de firma de paciente

En todas las instancias donde solicitamos la firma del paciente tenemos que agregar que se debe controlar si el paciente cuenta con firma digitalizada y de ser así, buscar la firma y crear un nuevo archivo usandola en conjunto con su nombre y número de DNI (como se hace actualmente). Es basicamente el mismo proceso, con la diferencia de que en vez de solicitar la firma, se busca en la base de datos multimedia. 
Esta lógica debe estar despues de revisar si la Obra Social requiere o no firma del paciente. Ya que si no requiere firma, se avanza sin usarla.