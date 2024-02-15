---
sidebar_label: 'Beneficios'
---

# Plan de beneficios

Funciones a desarrollar: 
(El proyecto cuenta con funcionalidades básicas ya desarrolladas, no mencionadas en este documento)

Mostrar el detalle de los productos cobrados en la visualización de la cada por quien la opera

Mostrar el detalle de los productos cobrados en la visualización de la cuenta corriente del paciente

Resolver error al hacer una Nota de Crédito de un cobro dentro del esquema de beneficios en cuanto a la vinculación con la factura de compra (subscripción

Inactivación del paciente y limpieza de la inscripción del pago al hacer una Not de Crédito de un cobro de plan de beneficios

Agregar en la tabla movCajaProductos la opción de un campo de observación del producto que se va a reflejar en la Factura de Compra. Serían datos del paciente y los planes a los que subscriben. Esto va a servir especialmente para una familia que se quiera suscribir a un plan de beneficios.

Modificar la impresión de la factura para que salgan las observaciones (si las hay) del producto al que se está subscribiendo. 

Definición de derechos a agregar:

Inscribir un paciente

Editar una inscripción

Cobrar inscripciones

Hacer una Nota de Crédito de una factura correspondiente al pago de planes de beneficios

Activar un paciente o grupo familiar

Inactivar un paciente o grupo familiar

Mostrar y alertar cuanto se encuentre vencida la obra social principal o alternativa, según esté indicada la obra social de plan de beneficios.

Generamos un bloqueo total al vencer la inscripción?

Agregar el campo de número de afiliado a la tabla de turnos para no tener potenciales errores en posibles cambios de información en los datos de la tabla de pacientes al empezar a contar con 2 obras sociales. De esta se toma el dato hoy.
Esto requiere modificar los standard procedures y vistas al turno. Identificar y editar todos los procesos que toman el número de afiliado del paciente.

Alta de afiliados en el portal web. Se tiene que clonar toda la lógica ya creada en el sistema de turnos y generarla a nivel de web service.
Armado de pantallas para portal (depende de alcance de funciones para el paciente)

Cobranza de la inscripción desde el portal a través de Mercado Pago integrando el pago y tocando la registración del pago para lo que es un producto.

Cambios en contratos de laboratorio para obra social alternativa. Manejando un descuento parametrizable automático para la definición de valores de la obra social del plan de beneficios a partir del contrato Particula

Toma de turnos de laboratorio para la obra social alternativa desde el sistema de turnos

Toma de turnos de laboratorio para la obra social alternativa desde el portal de turnos

Toma de turnos de prestaciones (prácticas o consultas) para la obra social alternativa desde el portal de turnos. Se va a buscar siempre por la OS primaria y si no se cubre la prestación, por la obra social alternativa (plan beneficios). Si la OS primaria es una de las de categoría “ABONAN” entonces pasa directamente a la OS del plan de beneficios.

Pruebas y verificación del funcionamiento de toma de turnos para obra social alternativa de prestaciones desde sistema de turnos.

Toma de turnos de Kinesio para la obra social alternativa desde el portal de turno

Toma de turnos de Kinesio para la obra social alternativa desde el sistema de turnos

Activación del paciente y asignación de obra social de beneficios y fechas de vencimiento

Cambios manuales en fechas de vencimiento o bajas. Con trazado

Programación para mail de envío de comprobante de pago y para mail de bienvenida al plan de beneficios
SE REQUIERE ARMADO Y ENTGREGA DE TEMPLATE EN HTML

Reportes varios (Ej.: Total de afiliados, pacientes próximos a vencer, composición por planes, etc)
SE REQUIERE DEFINICIÓN PRECISA DE REPORTES NECESARIOS.