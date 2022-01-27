# ChallengeLoan
Api para el seguimiento de los prestamos de un banco.

### Construido con
* Java 8
* Spring Boot
* JPA
* Maven
* Mockito
* Junit
* DB h2
* AWS Elastic Beanstalk

## Uso

Acceso a la base de datos H2:

- /console-h2
- url: jdbc:h2:mem:challengeloan
- username: banco
- password: 

## Solicitud de préstamo

Permite solicitar un prestamo.

#### Endpoint
```shell
/api/v1/loan-application
```

#### Entrada
```shell
curl
```
Datos de entrada:
* amount: monto del préstamo. 
* term: número de meses que tomará hasta que se pague el préstamo. 
* rate: tasa de interés (en formato decimal). 
* date: cuando se solicitó el préstamo (fecha de origen como una cadena ISO 8601).

#### Salida
```shell
json
```

Datos de salida:
* loan_id: identificador único. 
* installment: cuota mensual del préstamo.

#### Posibles Estados
- 400: Datos ingresados no validos
- 500 Internal Server Error: error interno del proceso


## Lista de préstamos

Permite consultar los préstamos en un rango de fechas.

#### Endpoint
```shell
/api/v1/loan
```

#### Entrada
```shell
curl
```
Parametros:
* dateFrom: fecha desde.
* dateTo: fecha hasta.

#### Salida
```shell
json
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


## Registro de pagos realizados o no realizados

Permite registrar un pago realizado (made) o no realizado (missed)

#### Endpoint
```shell
/api/v1/payment-applicant
```

#### Entrada
```shell
curl
```
Datos de entrada:
* payment_type: tipo de pago, made or missed. 
* date: fecha del pago. 
* amount: monto del pago realizado o no realizado.

#### Posibles Estados
- 400: Datos ingresados no validos
- 404 No Found: no existen prestamos en la base de datos
- 500 Internal Server Error: error interno del proceso



## Obtener la deuda

Permite consultar la deuda pendiente de un prestamo hasta una fecha especificada.

#### Endpoint
```shell
/api/v1/payment
```

#### Entrada
```shell
curl
```
Parametros:
* idLoan: id del prestamo a consultar.
* date: fecha limite de la consulta.

#### Salida
```shell
json
```
Datos de Salida:
* balance: valor de la deuda pendiente hasta la fecha especificada. 

#### Posibles Estados
- 400: Datos ingresados no validos
- 404 No Found: no existen prestamos en la base de datos
- 500 Internal Server Error: error interno del proceso
