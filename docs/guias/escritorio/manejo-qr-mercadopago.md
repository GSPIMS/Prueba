---
sidebar_label: 'Manejo de QR mercadopago'
---

# Creación de sector de caja y QR para cobrar por MP

Para cobrar por mercadopago con un puesto nuevo se deben hacer 3 pasos:

1. Crear el sector de caja al cual se va a asociar el QR. La información del sector debe ser la de la razón social a donde pertenece el puesto. Para esto se puede ver otro sector de caja WEB del mismo centro. Si es una sociedad nueva, esta información la tiene que gestionar administración (contaduría)Esto se hace en CAJAS > SECTOR DE CAJA
2. Una vez creado el sector de caja se debe dar de alta en mercadopago para generar el QR. Para esto se usa la acción “Alta Mercadopago” que se encuentra en la barra de herramientas de la misma pantalla (“Sector de caja”)

:::info[Impresión de QR]
Solicitar a comunicación la impresión del QR para una calidad estandarizada. Se debe enviar el QR obtenido al sector para que lo impriman.
:::

3. Por último tenemos que asociar el sector de caja creado al puesto en donde se va a usar ese QR. Cada puesto tiene una denominación única la cual se usa para darle un nombre a la computadora que se encuentra en dicho puesto (por ejemplo PCDIM-1000). Se tiene que entrar en ARCHIVO > PUESTOS y primero controlar que la denominación del puesto o el sector de caja no estén en uso. Si se encuentran en uso, hay que controlar que esté bien hecha la relación o si hay que hacer una modificación.En el caso de que el puesto sea nuevo entonces vamos a crear un nuevo puesto mediante el botón “Nuevo Puesto”.

<div style={{textAlign: 'center'}}>

![Alta de puesto](/img/puesto-caja.png)

</div>

- Nombre: Es el nombre de la computadora del puesto en donde se va a ubicar el QR.
- Recepción: La recepción a la que pertenece el sector que se está creando.
- Sector de Mpago: El sector de caja creado que se va a usar mediante la lectura del QR que va en el puesto indicado.
- Pad Firma Digital: Si el puesto cuenta con un dispositivo de toma de firma de pacientes, se tiene que usar esta marca para que el nombre del puesto aparezca en el listado de puestos a seleccionar en la aplicación de firmas.
4. Relacionar el puesto de recepción (PCDIM) con el sector de caja sobre el cual se creo el QR de Mercadopago. Una vez relacionados, ya se debería de poder cobrar por Mercadopago desde el puesto creado mediante el QR impreso.