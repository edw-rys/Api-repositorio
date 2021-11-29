# API PAGOS EPMAPAPAL APP

_Documentación REST API PARA PAGOS_

### Pre-requisitos 📋

_Se necesita la credencial del sistema_

```
Las peticiones por post tendrán que enviar la credencial del dispositivo

URL: https://app.epmapapal.gob.ec/
```

## Ejecutando las pruebas ⚙️

_Explica como ejecutar las pruebas automatizadas para este sistema_
<br>
<br>
<br>
### PETICIÓN API PARA CONSULTAR FACTURAS A PAGAR 🔩

## _EndPoint: api/api-get-invoices_
## _Metodo: POST_

## _Cabeceras_

```javascript
Accept = application/json
Content-Type = application/json
```

## _Cuerpo_

 access_key <-- llave emitida por la empresa para el acceso al sistema

 n_abonado  <--  Número de abonado del medidor del cliente 

```javascript
{
    "access_key": "123", 
    "n_abonado" : "01-25-20" 
}
```


## _Respuestas_

=> Código => 401<br>
=> status => Unauthorized

```javascript
{
    "errors": [
        {
            "key": "access_key",
            "message": "Autenticación no es válida"
        }
    ]
}
```
=> Código => 200<br>
=> status => ok

uuid <-- id para realizar el pago<br>
total <-- Total a pagar para cubrir la factura

```javascript
{
    "data": [
        {
            "uuid": 29,
            "subtotal": "10.33",
            "descuento": "1.03",
            "total_antes_de_impuestos": "9.30",
            "total_impuestos": "1.12",
            "total": "11.41",
            "cliente": "Edward Reyes",
            "fecha_factura": "2021-09-20",
            "vencimiento": "2021-11-26",
            "cantidad_pagos": 1,
            "x_sum_payments": "32.80",
            "total_pagos": "32.80",
            "pagos": [
                {
                    "date": "2021-11-23",
                    "total": "32.80",
                    "canal_pago": "DINERO ELECTRÓNICO "
                }
            ],
            "impuestos": [
                {
                    "name": "IVA 12%",
                    "porcentaje": "12.00"
                }
            ],
            "items": [
                {
                    "descripcion": "Servicio de alcantarillado pluvial",
                    "total": "3.51"
                },
                {
                    "descripcion": "Recolección de residuos sólidos",
                    "total": "0.08"
                },
                {
                    "descripcion": "Servicio de agua potable",
                    "total": "4.27"
                },
                {
                    "descripcion": "Servicio de alcantarillado sanitario",
                    "total": "2.47"
                }
            ]
        },
    ]
}
```


<br>
<br>
<br>


### PETICIÓN API PARA CONSULTAR TIPOS DE PAGOS 🔩

## _EndPoint: api/api-methods-invoice_
## _Metodo: GET_


## _Cabeceras_

```javascript
Accept = application/json
Content-Type = application/json
```

_Respuesta_


```javascript
[
    {
        "code": "01",
        "value": "SIN UTILIZACIÓN DEL SISTEMA FINANCIERO "
    },
    {
        "code": "15",
        "value": "COMPENSACIÓN DE DEUDAS "
    },
    {
        "code": "16",
        "value": "TARJETA DE DÉBITO "
    },
    {
        "code": "17",
        "value": "DINERO ELECTRÓNICO "
    },
    {
        "code": "18",
        "value": "TARJETA PREPAGO "
    },
    {
        "code": "19",
        "value": "TARJETA DE CRÉDITO "
    },
    {
        "code": "20",
        "value": "OTROS CON UTILIZACIÓN DEL SISTEMA FINANCIERO "
    },
    {
        "code": "21",
        "value": "ENDOSO DE TÍTULOS "
    }
]
```

<br>
<br>
<br>

### PETICIÓN API PARA PAGAR FACTURA 🔩

## _EndPoint: api/api-payment-invoices_
## _Metodo: POST_

## _Cabeceras_

```javascript
Accept = application/json
Content-Type = application/json
```

## _Cuerpo_

 access_key <-- llave emitida por la empresa para el acceso al sistema

 uuid  <--  Código de la factura a pagar

```javascript
{
    "access_key": "123", 
    "uuid" : "23",
    "total" : "23",
    "code" : "23"
}
```


_Respuestas_

=> Código => 401<br>
=> status => Unauthorized

```javascript
{
    "errors": [
        {
            "key": "access_key",
            "message": "Autenticación no es válida"
        }
    ]
}
```




=> Código => 422<br>
=> status => Unprocessable Entity

```javascript
{
    "message": "The given data was invalid.",
    "errors": {
        "total": [
            "total es requerido"
        ],
        "code": [
            "code es requerido"
        ]
    }
}


{
    "message": "The given data was invalid.",
    "errors": {
        "total": [
            "total No es un número válido"
        ],
        "code": [
            "code es requerido"
        ]
    }
}
```

=> Código => 404<br>
=> status => Not Found

```javascript
{
    "message": "Factura no encontrada",
    "status": "not_found"
}
```


=> Código => 409<br>
=> status => Conflict

```javascript
{
    "message": "No puede exceder la cantidad adeudada",
    "status": "conflict"
}
```


=> Código => 200<br>
=> status => OK
```javascript
{
    "message": "Saldo pagado",
    "status": "success",
    "data": {
        "uuid": 42,
        "subtotal": "32.80",
        "descuento": "0.00",
        "total_antes_de_impuestos": "32.80",
        "total_impuestos": "0.00",
        "total": "32.80",
        "cliente": "Edward Reyes",
        "fecha_factura": "2021-09-22",
        "vencimiento": null,
        "cantidad_pagos": 3,
        "x_sum_payments": "32.80",
        "total_pagos": "32.80",
        "pagos": [
            {
                "date": "2021-11-24",
                "total": "0.00",
                "canal_pago": "SIN UTILIZACIÓN DEL SISTEMA FINANCIERO "
            },
            {
                "date": "2021-11-24",
                "total": "32.80",
                "canal_pago": "SIN UTILIZACIÓN DEL SISTEMA FINANCIERO "
            },
            {
                "date": "2021-11-24",
                "total": "0.00",
                "canal_pago": "SIN UTILIZACIÓN DEL SISTEMA FINANCIERO "
            }
        ],
        "impuestos": [],
        "items": [
            {
                "descripcion": "Servicio de alcantarillado sanitario",
                "total": "6.82"
            },
            {
                "descripcion": "Servicio de agua potable",
                "total": "11.11"
            },
            {
                "descripcion": "Servicio de alcantarillado pluvial",
                "total": "14.87"
            },
            {
                "descripcion": "Recolección de residuos sólidos",
                "total": "0.08"
            }
        ]
    }
}
```

