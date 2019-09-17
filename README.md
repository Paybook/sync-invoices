

## 0. Facturación (Actualización CFDI 3.3)

A través del servicio REST Sync es poible realizar el timbrado de [CFDI's](http://www.sat.gob.mx/informacion_fiscal/factura_electronica/Paginas/requisitos_factura_cfdi.aspx). Este servicio permite:

1. Simplificar el proceso de timbrado de facturas
2. Toda factura expedida a través de Sync será validada
3. Cancelación de facturas timbradas
4. Consulta de facturas timbradas
5. El usuario podrá elegir con qué proveedor quiere timbrar facturas. 



## 1. Contribuyentes

### Agregar Contribuyentes

```
POST invoicing/mx/taxpayers
```

El primer paso para generar un CFDI es agregar un contribuyente al sistema, cada usuario de Sync puede tener ligados múltiples contribuyentes. Para agregar un nuevo contribuyente se requiere:

- RFC del contribuyente.
- Credenciales [CSD](http://www.sat.gob.mx/informacion_fiscal/tramites/comprobantes_fiscales/Paginas/ficha_108_cff.aspx). Estos son 2 archivos en formato cer y key los cuales deben ser codificados en base64 al ser enviados en la petición.
- Contraseña de los CSDs. 
- API key de Sync.
- Usuario de Sync -- id_user. Este sería el id del usuario al cual se asociará el contribuyente.

Nota: En el folder '/CSD' se incluyen CSD's dummy que proporciona el SAT para poder realizar pruebas.



El siguiente es un ejemplo que usa los CSD públicos del SAT.

```
{
    "api_key" : "{{sync_api_key}}",
    "id_user" : "{{sync_id_user}}",
    "taxpayer" : "AAA010101AAA",
    "password" : "12345678a",
   "cer": "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",
  "key": "MIIFDjBABgkqhkiG9w0BBQ0wMzAbBgkqhkiG9w0BBQwwDgQIAgEAAoIBAQACAggAMBQGCCqGSIb3DQMHBAgwggS+AgEAMASCBMh4EHl7aNSCaMDA1VlRoXCZ5UUmqErAbuck7ujDnmKxSaOGzJzn1hAlfBWJNtr1rgiCXRHB5/2qJ/CnTOkCcgutvs1xl3vxHgY1+N9I60iZUG+yjfEd+ungL4alXXMtKgZ8CkQXaeYIeQXFdyZ5jUU07Cy+LjMrIOAh1m/VnL6U/qW3dY+oJmII6gCG0SKcfCojeCpBVL2ispK2CBTpMDO4hd7vnbFhafl9/wUkAncmz5SHLjXPMKgmK7HvBiUSMRYFCjcNEBvMshI7E1//nG8pi0Xrmbq4MfT1B+SF8vbA39hCqKP32m+QFlOduHlaFSnW96UkMBT5hF1qImwU3HTbtKfAumo3BLzYJ9XP7Y6eVOFFSSsXudrAt94mH7CojUjazGHBsqagsUY85Q7Cz0TTvnnvWFNFAj/xbQm6nT1VL8FkdJm8hEb5YLaOqQZ8y1AEv8sCq/M51aHglexuzGFIIUTF+/XQGeYDBITlS6z2TryoHp8n1+6LpClL51WrIfaSxyMEtG2fmAHN82iNujOP6MBR7aMZ6dfxJctFRAaWlmi89wa5VhyeaoDzkx1roJznF3MLxVKROmYLDYk142IwRtTgWrex4Wnidpo4unrfL+uj6VwTUDk0cizaYvamRhlZ/LXdwyB1syb/GQGu94gSzB1zAzb5/IIbtyofK+/tVTv08OMpCqHfBye1QJQg+vxQHMkbhZH6sEgORSEjuidW13DTKi8xyryQsD5WccMh8WDxMuAVFUldrwWdGilFKg0G99S/XJWLwKB74Nv0v/Ygdp6/d9T0fFD3FXpb9RznErCgfVSMtrPv1svGg3QFwh+qmkzjh+NBwUrmmqEjNshji+9SB4fnJNYlKVvu4WAzMKliixUkRcCID1QYwLtiyWuwZDYxKTnk7Y1LXmRGqqhNbh4kdnTNUdkxEjqp+UVtBdaxswa6s4qrLbNeD7VN+1KJEMN6/zZ6+2Uj2KBMzaDA0zwAHMB1gyPkgX0v47e61iffaVAUQzDGYGbDERG2vT8234NSDdgqzOpsf7il2Pv+uF0oab+db62JiRvOEjNefXG5p8KRudYyaVO8N7iTdRRj/A/yDwjmSq9dDCZEZE0cD6BEaAgmjvqwF5IvGgJGnWYKhrOGBPv+VL6zGOXo/L9zenxYwKNTHzNYlvug/t4gXQmArroqA2YKBGpYb8/FY/q3t3k+u1bXWvNLOzWi81InvxFSCTu6l9GBCthyWwekWdoL6ssSzOmzPr/d2klSRST3ByJmAJzLGJFsj6AL01BaUVWERH0s+GmnSWOU8ZIQVGF7aOEWWbtD0vyjJRxQnxPxn+Tt3oT9Nob10QGwG/2tNZtZuhAMf1yt+cF8jl0hC/LI0FtMqmLAkxaEOiXHmFuKXbAjFxIjdIwgWsAZe1cTLzR44jIKwlB64jvh1LXmJ5jCszLd/fuCEB0XZUWLDRCZVb82MqcZl7U/gaFazSqm71NNafCDzWjWO4ukWN1lcTDJE05KgeRqoYIEcpU8jXy/CAEaoseA1bWDnfnLJk2axVXzmrtYnojyKjTjDz3In41Kjsx3nNOegqtH7O2gl9YBzJfgwZmF0ldk+udcotc0JwIXYWk7b5HmRgXWa+WvDHSwyLzMrbw="
}
```




### Actualizar información de un contribuyente

```
PUT invoicing/mx/taxpayers
```

Una vez que se ha agregado un contribuyente y en caso de que sus credenciales expiren o cambien, se pueden actualizar a través del endpoint PUT con los mismos parámetros que al agregarlo.



### Traer listado de contribuyentes

También es posible obtener el listado de contribuyentes que se han agregado:

```
GET invoicing/mx/taxpayers
```
La respuesta es un arreglo de objetos. Cada uno de estos contiene el RFC del contribuyente, además del rango de fechas en UNIX timestamp que indican el periodo de vigencia de su certificado.

Ejemplo respuesta
```
{
    "rid": "5cdb1520-cfbf-4d72-98d2-XXXXXXXX",
    "code": 200,
    "errors": null,
    "status": true,
    "message": null,
    "response": [
        {
            "taxpayer": "AAA010101AAA",
            "cerValidFrom": 1495079696,
            "cerValidTo": 1621310096
        },
        {
            "taxpayer": "GOYA780416GM0",
            "cerValidFrom": 1351012593,
            "cerValidTo": 1477242993
        },
        {
            "taxpayer": "LAN7008173R5",
            "cerValidFrom": 1477432331,
            "cerValidTo": 1603662731
        }
    ]
}
```
## 2. Facturas

### Timbrar factura

Para timbrar una factura a través del servico de Paybook, se ha generado un [esquema basado en una estructura JSON](CFDI_3.3_JSON_structure.json) con el fin de simplificar el proceso de timbrado, como usuario usted solo tiene que llenar los datos requeridos en el JSON y el sistema se encargará de:

- Construir el XML
- Generar los sellos de timbrado
- Validar el XML
- Enviarlos al SAT a través de los proveedores disponibles (PACs)

Esto se realiza consumiendo el siguinte endpoint.

```
POST invoicing/mx/invoices
```
En el archivo [CFDI33_ejemplo_valid.json](CFDI33_ejemplo_valid.json), se encuentra un ejemplo de una llamada válida incluyendo la estructura del JSON de la factura. Los elementos que debe incluir el apicall son:

- api_key: El api key de sync
- id_user: Id del usuario que timbra la factura
- id_provider: El nombre d eproveedor de timbrado, para pruebas en sandbox se usa "acme"
- invoice_data: La estructura en formato JSON de la factura
- invoice_xml:  Si se tienen los datos de la factura en un XML valido de acuerdo al SAT se pueden enviar en vez del campo invoice_data, el XML debe venir como un string codificado en base64

NOTA: Para realizar facturas de prueba no validas, se puede utilizar el proveedor "acme".

La respuesta que se obtiene del timbrado incluye:
- xml: El XML de la factura sellado y timbrado
- uuid: El UUID de la factura
- errors: En caso de que no se pueda timbrar o los datos tengan errores se incluye un arreglo con los posibles errores


### Conversión a base64 
Para realizar la conversión de los archivos a base64, se puede hacer uso de algún conversor en línea por ejemplo [este](https://jpillora.com/base64-encoder/).

También se puede hacer desde la consola de comandos

#### Windows 
```
certutil -encode archivo_entrada.cer archivo_salida_base64.txt
```
Nota: El string que se envia,no debe incluir esta entre "-----BEGIN CERTIFICATE-----"  y "-----END CERTIFICATE-----", además se deben eliminar los retornos y tabs del string.

#### OS X 
```
openssl base64 -in 'archivo_entrada.cer' -out 'archivo_salida_base64.txt'
```
Nota: se deben eliminar los retornos y y tabs del archivo.

#### Linux 
```
base64 archivo_entrada.cer >archivo_salida_base64.txt
```

### Traer facturas
Para traer las facturas que fueron timbradas previamente se usa el siguiente endpoint:

```
GET invoicing/mx/invoices
```
Algunos parámetros que se pueden enviar para realizar el filtrado de las facturas que se traerán son los siguientes:
```
{
    api_key: la api key de paybook,
    id_user: el id del usuario,
    uuid: el uuid de la factura (opcional),
    issuer: RFC del emisor de la factura (opcional),
    recipient: RFC del receptor (opcional),
    dt_create_from: fecha de inicio la creación UNIX_timestamp (opcional),
    dt_create_to: fecha final de la creación UNIX_timestamp(opcional),
    dt_register_from: fecha inicial del registro UNIX_timestamp(opcional),
    dt_register_to: fecha final de registro UNIX_timestamp(opcional),
}
```

### Cancelar factura
Para realizar la cancelación de una factura lo único que se require es el UUID. El proceso se realiza usando el endpoint:
```
PUT invoicing/mx/invoices/{{uuid}}/cancel
```

La factura cancelada seguirá almacenada en el sistema pero con el estado de cancelación. En la respuesta exitosa de cancelación contiene el parámetro 'success' igual a 'true', además del acuse de cancelación en formato XML, el cual proporcionan los proveedores de servicio de timbrado.

## 3. Proveedores facturación PACs

### Listado de proveedores de facturación.

Como se mencionó anteriormente, Paybook puede realizar el proceso de timbrado a través de múltiples proveedores. Usando el siguiente endpoint se obtiene una lista de los proveedores dados de alta en el sistema, si se desea timbrar con un proveedor en especifico,  se debe agregar el parámetro "id_provider" en la petición POST de invoices, el valor de éste puede ser el nombre o el id proporcionados al consultar la lista de proveedores:

```
GET invoicing/mx/providers?api_key={{sync_api_key}}&id_user={{sync_id_user}}
```

Ejemplo de respuesta proveedores, el atributo  cfdi_version_support indica la versión de CFDI que soporta cada proveedor:


```
{
    "rid": "c053e728-58c9-4363-b3c3-c0e073f8cd2c",
    "code": 200,
    "errors": null,
    "status": true,
    "message": null,
    "response": [
        {
            "id": "57dc9e9308898644058b4568",
            "name": "iofacturo",
            "cfdi_version_support": "3.2"
        },
        {
            "id": "57e9ab29088986e9028b45e3",
            "name": "acme",
            "cfdi_version_support": "3.3"
        }
    ]
}
```


Hay que destacar que al usar el proveedor "acme", las facturas que se timbran no son válidas ya que es un sistema de pruebas de timbrado en el sandbox de paybook.


## 4. Complementos

Actualmente en Paybook estamos trabajando por incluir todos los esquemas de [complementos que el SAT](http://www.sat.gob.mx/informacion_fiscal/factura_electronica/paginas/complementos_factura_cfdi.aspx) utiliza para los diferentes tipos de factura. Si requiere algún complemento que aun no esta incluido en la siguiente lista puede contactarnos a ornelas@paybook.com y a la brevedad lo integraremos en el sistema.

### Complemento recepción de pagos

El SAT implemento este nuevo [complemento para pagos](http://www.sat.gob.mx/informacion_fiscal/factura_electronica/Paginas/Recepcion_de_pagos.aspx), a partir de la versión CFDI 3.3. Este tipo de complemento se utiliza cuando el pago de la factura no se recibe en el momento o cuando se realizarán pagos parciales hasta completar el total de la factura. 

En paybook, los datos necesarios deben ser llenados en el campo "Complemento" de "invoice_data" en el JSON del apicall. Puede consultarse la estructura completa del complemento en el documento [CFDI_3.3_JSON_COMPLEMENTO_PAGO_ESTRUCTURA.json](CFDI_3.3_JSON_COMPLEMENTO_PAGO_ESTRUCTURA.json), y se puede consultar el ejemplo de un CFDI de complemento válido en el archivo [CFDI_3.3_JSON_COMPLEMENTO_PAGO_EJEMPLO1.json](CFDI_3.3_JSON_COMPLEMENTO_PAGO_EJEMPLO1.json)	

En el portal del SAT se encuentra una [guia de llenado](http://www.sat.gob.mx/informacion_fiscal/factura_electronica/Documents/Complementoscfdi/Guia_comple_pagos.pdf) de los parámetros del complemento, todos las reglas aplican a los campos del JSON que se envia en paybook. Además existe un documento con [dudas frecuentes](http://www.sat.gob.mx/informacion_fiscal/factura_electronica/Documents/Complementoscfdi/PregFrec_RP.pdf) acerca de este tipo de CFDI.



### Complemento Impuestos Locales
Los parámetros del complemento para impuestos locales se pueden encontrar en el documento de [complemento otros derechos e impuestos en la factura](http://www.sat.gob.mx/informacion_fiscal/factura_electronica/Paginas/complemento_derechos_impuestos.aspx), en el JSON se coloca en el campo Complemento como se observa en la siguiente estructura:

```
...
"Complemento": {
            "ImpuestosLocales": {
                "version" : "1.0",
                "TotaldeRetenciones": "",
                "TotaldeTraslados": "",
                "RetencionesLocales": [
                    {
                      "ImpLocRetenido": "",
                      "TasadeRetencion": "",
                      "Importe": ""
                    }
                ],
                "TrasladosLocales": [
                    {
                      "ImpLocTrasladado": "",
                      "TasadeTraslado": "",
                      "Importe": ""
                    }
                ]
            }
}
```

Puede ver un ejemplo de JSON valido con el complemento en el archivo [CFDI_3.3_JSON_EJEMPLO_COMPLEMENTO_IMPLOCALES.json](CFDI_3.3_JSON_EJEMPLO_COMPLEMENTO_IMPLOCALES.json)

### Complemento Comercio Exterior
Los parámetros del complemento para Comercio Exterior se pueden encontrar en el documento de [complemento otros derechos e impuestos en la factura](http://omawww.sat.gob.mx/informacion_fiscal/factura_electronica/Documents/Complementoscfdi/GuiaComercioExterior3_3.pdf). La estructura que se debe seguir para enviar como JSON es la siguiente [CFDI_3.3_JSON_ESTRUCTURA_COMPLEMENTO_COMERCIO_EXTERIOR.json](CFDI_3.3_JSON_ESTRUCTURA_COMPLEMENTO_COMERCIO_EXTERIOR.json). En este archivo se puede ver un ejemplo con datos de la estructura: [CFDI_3.3_JSON_EJEMPLO_COMPLEMENTO_COMERCIO_EXTERIOR.json](CFDI_3.3_JSON_EJEMPLO_COMPLEMENTO_COMERCIO_EXTERIOR.json)





### Complemento institucones educativas
La estructura del [complemento instituciones educativas](http://www.sat.gob.mx/informacion_fiscal/factura_electronica/Paginas/complemento_iedu.aspx) se coloca en el campo como se observa en la siguiente estructura:

```
{
    "Concepto" : {
            "ClaveProdServ": "01010101",
            "NoIdentificacion": "UT421510 ",
            "Cantidad": "1",
            "ClaveUnidad": "H87",
            "Unidad": "PZA",
            "Descripcion": "COLEGIATURA",
            "ValorUnitario": "50",
            "Importe": "50",
            "Descuento": "0",
            "ComplementoConcepto" : {
                "instEducativas" : {
                    "version" : "",
                    "nombreAlumno" : "",
                    "CURP" : "",
                    "nivelEducativo" : ",
                    "autRVOE" : "",
                    "rfcPago" : ""
                }
            }
        }
    }
```

Puede ver un ejemplo de JSON valido con el complemento en el archivo [CFDI_3.3_JSON_EJEMPLO_COMPLEMENTO_IEDU.json](CFDI_3.3_JSON_EJEMPLO_COMPLEMENTO_IEDU.json)

### Complemento de nómina

La estructura del [complemento de nómina](http://www.sat.gob.mx/informacion_fiscal/factura_electronica/Paginas/complemento_nomina.aspx), en en el archivo [CFDI33_NOMINA12.jsonon](CFDI33_NOMINA12.json), se encuentra la estructura del JSON que se debe enviar para cumplir con los requisitos del recibo. Los campos siguen las reglas del documento oficial del SAT.


### Complemento donatarias

La estructura del [complemento de donatarias](https://www.sat.gob.mx/consulta/53870/genera-tus-facturas-electronicas-con-el-complemento-para-donatarias). Para incluirlo en el JSON se agrega en el nodo Complemento con la donataria con la respectiva información:
```
{
    "Complemento" : {
            "Donatarias": {
                "version" : "1.1",
                "noAutorizacion" : "325-SAT-28-I-(21)-0000",
                "fechaAutorizacion" : "2019-01-23",
                "leyenda" : "Este comprobante ampara un donativo, el cual será destinado por la donataria a los fines propios de su objeto social."

            }
    }
```


## 5. Obtener PDF y enviar email


#### Enviar una factura por email
```
POST invoicing/mx/invoices
```

Para enviar el email durante el timbrado de la factura se deben agregar el parámetro "email_data" a la petición de timbrado:

```
{
    "api_key" : "{{sync_api_key}}",
    "id_user" : "{{sync_id_user}}",
    "id_provider":"iofacturo",
    "invoice_data": {} //datos json de la factura,
    "email_data": {
        "email": "emailcliente@email.com", //(obligatorio)
        "reply": "miemail@email.com", //(opcional) 
        "body": "<h1> Hola Mundo</h1>", //(opcional) 
        "subject": "Email de factura" //(opcional)  
    }

}
```

- "email", es el email al que se enviará la factua, es obligatorio
- "reply", es el email de respuesta que aparecera en el email de la factura, es opconal.
- "body", es el contenido del email, puede ser texto plano o html, es opcional y si no se agrega, se envia en el contenido el RFC emisor, el receptor, el total de la factura y el UUID
- "subject", es el asunto del email, es opcional y si no se agrega se envia como "Factura de {{RFC del emisor}}

#### Reenviar una factura por email
Se usa el endpoint:
```
POST invoicing/mx/invoices/{{uuid}}/send
```

Con los parámetros

```
{
    "api_key": la api key de paybook,
    "id_user": el id del usuario,
    "email_data": {
        "email": "emailcliente@email.com", //(obligatorio)
        "reply": "miemail@email.com", //(opcional) 
        "body": "<h1> Hola Mundo</h1>", //(opcional) 
        "subject": "Email de factura" //(opcional)  
    }
}
```

#### Obtener el PDF en la respuesta de timbrado y agregar comentarios

Durante el timbrado es posible obtener el archivo PDF de la factura en la repuesta, este se incluye en el campo "pdf", y se regresa como una cadena codificada en base64, la cual se puede convertir al recibirla en un archivo binario pdf.

Adicionalmente se puede enviar el campo "pdf_comments", para que se agregue información en el campo de comentarios del archivo pdf para el cfdi impreso.

Para obtener el archivo se envia:
```
{
    "api_key": la api key de paybook,
    "id_user": el id del usuario,
    "pdf": true,
    "pdf_comments": "Comentarios para el PDF impreso",
    "invoice_data":  ...
}
```


## 6. Información extra

### Cancelación de facturas con notas de crédito

A partir de enero del 2018, habra nuevos requerimientos para la cancelación de facturas electrónicas, por lo que se requiere la aprobación del receptor para la cancelación de una factura.

Por esta razón, algunos usuarios, en vez de realizar el proceso de cancelación han optado por utilizar notas de crédito, que sirven para corregir el primer comprobante emitido, ya sea para hacer un descuento, bonificación o devolución.

Con este CFDI, la factura no queda propiamente cancelada, pero fiscalmente es similar a una cancelación ya que el total de la operación será 0. Un punto importante, es que es altamente recomendable emitir lanota de crédito el mismo mes contable que el CFDI original, de lo contrario quedara un mes con valor negativo y el siguiente con el egreso positivo.

Para generar la nota es necesario:

- Esta nota se emite como un CFDI de egreso por la misma cantidad original del CFDI que se emitió como ingreso.
- Este CFDI debe ser por el total del original.
- En la estructura se debe incluir la relación con el original, con la clave 01 y el UUID del original

```
"CfdiRelacionados" : {
                    "TipoRelacion": "01",
                    "CfdiRelacionado": [
                        {
                            "CfdiRelacionado": {
                            "UUID": "560a8451-a29c-41d4-a716-544676554400"
                                }
                           }
                ]
            }
```
- En el campo TipoDeComprobante debe ser "E" de egreso
- Método de pago lleva "PUE" pago en una sola exhibición
- En Forma de pago se debe elegir la misma del original
- Clave producto  o serivio debe ser "84111506"
- Y la clave de unidad "ACT"

## 7. Addenda
Las addendas que se agreguen en el XML se timbran directamente como se encuentran, es por eso que es importante que se verifique que se encuentren bien construidas.

Para el timbrado de JSON, se soportan las addendas pasandolas directamente el XML en la propiedad "Addenda", ejemploe:

```
...
"Addenda" : "<Addenda><mabeFactura xmlns:mabe=\"http://recepcionfe.mabempresa.com/cfd/addenda/v1\" xmlns:xsi=\"http://www.w3.org/2001/XMLSchema-instance\" xsi:schemaLocation=\"http://recepcionfe.mabempresa.com/cfd/addenda/v1 http://recepcionfe.mabempresa.com/cfd/addenda/v1/mabev1.xsd\" version=\"1.0\" tipoDocumento=\"FACTURA\" folio=\"IA1222\" fecha=\"2011-09-20\"><mabeMoneda tipoMoneda=\"MXN\" /> <mabeProveedor codigo=\"PPIN1120\" /> <mabeEntrega plantaEntrega=\"590\" /> <mabeDetalles><mabeDetalle noLineaArticulo=\"1\" codigoArticulo=\"N40254\" descripcion=\"ALUM.BARRA CUADRADA 6061T6 25.4 MM.\" unidad=\"KG\" cantidad=\"1.120\" precioSinIva=\"73.60\" importeSinIva=\"82.43\" /> <mabeDetalle noLineaArticulo=\"2\" codigoArticulo=\"N80091\" descripcion=\"ALUMINIO SOLERA 6061 T-6 9.5X101.6MM\" unidad=\"KG\" cantidad=\"1.900\" precioSinIva=\"73.60\" importeSinIva=\"139.84\" /> <mabeDetalle noLineaArticulo=\"3\" codigoArticulo=\"N80504\" descripcion=\"ALUMINIO SOLERA 6061 T-6 12.7X50.8MM\" unidad=\"KG\" cantidad=\"3.600\" precioSinIva=\"73.60\" importeSinIva=\"264.96\" /> </mabeDetalles><mabeSubtotal importe=\"487.23\" /> </mabeFactura></Addenda>"
...
```