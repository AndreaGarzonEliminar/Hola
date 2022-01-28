 # ChallengeLoan
Api para el seguimiento de los prestamos de un banco.

### Construido con
* Java
* Spring Boot
* JPA
* Maven
* Mockito
* Junit
* DB h2
* AWS Elastic Beanstalk

## Uso

Acceso a la base de datos H2:

- http://projectchallengeloan-env-1.eba-dip5e443.us-east-1.elasticbeanstalk.com/h2-console
- jdbc url: jdbc:h2:mem:challengeloan
- username: banco
- password: 



## 1. Solicitud de préstamo

Permite solicitar un prestamo.

#### Endpoint
```shell
http://projectchallengeloan-env-1.eba-dip5e443.us-east-1.elasticbeanstalk.com/api/v1/loan-application
```

#### Solicitud de ejemplo:
```shell
curl --request POST \
  --url http://projectchallengeloan-env-1.eba-dip5e443.us-east-1.elasticbeanstalk.com/api/v1/loan-application \
  --header 'Content-Type: application/json' \
  --data '{
    "amount": 1000,
    "term": 12,
    "rate": 0.05,
    "date": "2022-01-28T08:59Z"
}'
```
Datos de entrada:
* amount: monto del préstamo. 
* term: número de meses que tomará hasta que se pague el préstamo. 
* rate: tasa de interés (en formato decimal). 
* date: cuando se solicitó el préstamo (fecha de origen como una cadena ISO 8601).

#### Respuesta de ejemplo
```shell
{
    "loanId": 1,
    "installment": 85.60748178846745
}
```

Datos de salida:
* loan_id: identificador único. 
* installment: cuota mensual del préstamo.

#### Posibles Estados
- 201 Created: registro creado
- 400 Bad Request: datos ingresados no validos
- 500 Internal Server Error: error enterno del proceso



## 2. Lista de préstamos

Permite consultar los préstamos en un rango de fechas.

#### Endpoint
```shell
http://projectchallengeloan-env-1.eba-dip5e443.us-east-1.elasticbeanstalk.com/api/v1/loan
```

#### Solicitud de ejemplo
```shell
curl --request GET 'http://projectchallengeloan-env-1.eba-dip5e443.us-east-1.elasticbeanstalk.com/api/v1/loan?dateFrom=2022-01-28T02:59:55z&dateTo=2022-01-28T21:59:55z'
```
Parametros:
* dateFrom: fecha desde.
* dateTo: fecha hasta.

#### Respuesta de ejemplo
```shell
[
    {
        "id": 1,
        "amount": 1000.0,
        "term": 12,
        "rate": 0.05,
        "date": "2022-01-28T08:59:00Z"
    },
    {
        "id": 2,
        "amount": 2000.0,
        "term": 2,
        "rate": 0.05,
        "date": "2022-01-28T08:59:00Z"
    }
]
```
Datos de Salida:
* id: id único del prestamo
* amount: monto del préstamo. 
* term: número de meses que tomará hasta que se pague el préstamo. 
* rate: tasa de interés (en formato decimal). 
* date: cuando se solicitó el préstamo (fecha de origen como una cadena ISO 8601).

#### Posibles Estados
- 400: Datos ingresados no validos
- 404 No Found: no existen prestamos en la base de datos
- 500 Internal Server Error: error interno del proceso



## 3. Registro de pagos realizados o no realizados

Permite registrar un pago realizado (made) o no realizado (missed)

#### Endpoint
```shell
http://projectchallengeloan-env-1.eba-dip5e443.us-east-1.elasticbeanstalk.com/api/v1/payment-application
```

#### Solicitud de ejemplo
```shell
curl --request POST 'http://projectchallengeloan-env-1.eba-dip5e443.us-east-1.elasticbeanstalk.com/api/v1/payment-application' \
--header 'Content-Type: application/json' \
--data '{
	"payment": "made",
    "date": "2022-01-28T20:59:55Z",
    "amount": 100,
    "idLoan": 1
}'
```
Datos de entrada:
* payment_type: tipo de pago, made or missed. 
* date: fecha del pago. 
* amount: monto del pago realizado o no realizado.

#### Posibles Estados
- 201 Created: registro creado
- 400 Bad Request: datos ingresados no validos
- 404 No Found: no existen prestamos en la base de datos
- 500 Internal Server Error: error interno del proceso



## 4. Obtener la deuda

Permite consultar la deuda pendiente de un prestamo hasta una fecha especificada.

#### Endpoint
```shell
http://projectchallengeloan-env-1.eba-dip5e443.us-east-1.elasticbeanstalk.com/api/v1/payment
```

#### Solicitud de ejemplo
```shell
curl --request GET 'http://projectchallengeloan-env-1.eba-dip5e443.us-east-1.elasticbeanstalk.com/api/v1/payment?idLoan=1&date=2022-01-28T20:59:55Z'
```
Parametros:
* idLoan: id del prestamo a consultar.
* date: fecha limite de la consulta.

#### Respuesta de ejemplo
```shell
{
    "balance": 927.2897814616094
}
```
Datos de Salida:
* balance: valor de la deuda pendiente hasta la fecha especificada. 

#### Posibles Estados
- 400: Datos ingresados no validos
- 404 No Found: no existen prestamos en la base de datos
- 500 Internal Server Error: error interno del proceso
