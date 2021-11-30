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

 identificator  <--  Número de abonado del medidor del cliente o número de cédula

```javascript
{
    "access_key": "123", 
    "identificator" : "01-25-20" 
}
```


## _Respuestas_

=> Código => 401<br>
=> status => Unauthorized

```javascript
{
    "message": "The given data was invalid.",
    "errors": {
        "access_key": [
            "Autenticación no es válida"
        ]
    }
}
```


=> Código => 422<br>
=> status => Unprocessable Entity

```javascript
{
    "message": "The given data was invalid.",
    "errors": {
        "identificator": [
            "identificator es requerido"
        ]
    }
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
    "transaction_id" : "23"
}
```


_Respuestas_

=> Código => 401<br>
=> status => Unauthorized

```javascript
{
    "message": "The given data was invalid.",
    "errors": {
        "access_key": [
            "Autenticación no es válida"
        ]
    }
}
```




=> Código => 422<br>
=> status => Unprocessable Entity

```javascript
{
    "message": "The given data was invalid.",
    "errors": {
        "total": [
            "total es requerido",
            "total No es un número válido"
        ],
        "transaction_id": [
            "transaction id es requerido"
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



<br>
<br>
<br>
<br>

### PETICIÓN API PARA CONSULTAR PAGOS HECHOS SEGÚN UNA FECHA 🔩

## _EndPoint: api/api-info-payments_
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
    "date" : "2021-11-27"
}
```


_Respuestas_

=> Código => 401<br>
=> status => Unauthorized

```javascript
{
    "message": "The given data was invalid.",
    "errors": {
        "access_key": [
            "Autenticación no es válida"
        ]
    }
}
```


=> Código => 422<br>
=> status => Unprocessable  Entity

```javascript
{
    "message": "The given data was invalid.",
    "errors": {
        "date": [
            "fecha la fecha no es válida"
        ]
    }
}
```

=> Código => 200<br>
=> status => ok

```javascript
{
    "data": [
        {
            "payment_id": 37,
            "transaction_id": "111",
            "total": 3
        },
        {
            "payment_id": 38,
            "transaction_id": "222",
            "total": 1
        },
        {
            "payment_id": 39,
            "transaction_id": "333",
            "total": 1.2
        },
        {
            "payment_id": 40,
            "transaction_id": "444",
            "total": 0.8
        },
        {
            "payment_id": 41,
            "transaction_id": "555",
            "total": 2
        }
    ]
}
```


<br>
<br>
<br>
<br>

### PETICIÓN API PARA REVERSAR UN PAGO 🔩

## _EndPoint: api/api-reverse-pay_
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
    "transaction_id" : "34"
}
```


_Respuestas_

=> Código => 401<br>
=> status => Unauthorized

```javascript
{
    "message": "The given data was invalid.",
    "errors": {
        "access_key": [
            "Autenticación no es válida"
        ]
    }
}
```


=> Código => 422<br>
=> status => Unprocessable  Entity

```javascript
{
    "message": "The given data was invalid.",
    "errors": {
        "transaction_id": [
            "transaction id es requerido",
            "transaction id no existe"
        ]
    }
}
```

=> Código => 200<br>
=> status => ok

```javascript
{
    "message": "Pago eliminado",
    "status": "success",
    "data": []
}
```
