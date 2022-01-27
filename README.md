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
