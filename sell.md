### Sell (Venta de BTC)
Un sell es una venta de BTC por parte del usuario. El usuario tiene BTC y desea recibir CLP. Un sell se divide en 3 pasos:

1. Solicitar precios (`/prices`)
2. Enviar orden de venta (`/sell`)
3. Enviar los datos bancarios donde enviar los CLP (`/bank_pay`)

#### Solicitar precios (`/prices`)
Esta accion no requiere parametros (solo un token) y entrega los precios del momento para compra y venta de BTC. Para realizar una
venta de BTC el precio que se debe usar es el valor `sell_BTC_CLP`.
Para obtener el valor en CLP a recibir, basta multiplicar el valor de BTC a vender por el precio en `sell_BTC_CLP`. 

```
Vender 0.25 BTC
sell_BTC_CLP = 371418.89 
CLP a recibir = 0.25 * 371418.89 = 92 855
```

Si se desea calcular cuantos BTC se deben enviar para recibir un cierto monto el CLP, se debe dividir por el valor de `sell_BTC_CLP`.
```
Recibir 100000 CLP
sell_BTC_CLP = 371418.89 
BTC a enviar = 100000 / 371418.89 = 0.26923778
```

Los valores de `sell_BTC_CLP` cambian constantemente, por lo que antes de realizar cualquier orden, tanto de venta como de compra, se debe ejecutar un `/prices` inicialmente

#### Enviar orden de venta (`/sell`)
Esta accion permite colocar una orden de venta de BTC por medio de la
API. Una orden de venta representa una solicitud del usuario por realizar una venta de BTC. Una vez colocada la orden el usuario se compromete a enviar los BTC y Yaykuy se compromete a entregar los CLP acordados en la orden (independiente de las variaciones de precios posteriores).

Los parametros de la orden de venta son:
- `amount_BTC`   : Monto de BTC que el usuario desea enviar
- `sell_BTC_CLP` : Precio al cual el usuario desea vender los BTC. Este precio es el obtenido previamente con `/prices`
- `email`		 : Este parametro es opcional y permite que yaykuy se pueda comunicar con el usuario en caso de problemas con la orden.
- `pin`			 : Este parametro es opcional. Actualmente no esta en uso puesto que la unica forma de entregar los CLP es por transferencia bancaria. 

Los parametros de respuesta son:
- `status`         : Estado de la orden de venta. "ok" o "error"
- `message`        : Mensaje en caso de error
- `deposit_BTC`    : Direccion bitcoin de yaykuy a donde se deben enviar los BTC
- `amount_CLP`     : Monto en CLP que se pagaran al usuario. Es la multiplicacion de los parametros de entrada `amount_BTC` y `sell_BTC_CLP`
- `yky_code`       : Codigo yky que identifica la orden. Este codigo debe usarse para la accion `/bank_pay`.

Los errores posibles para la orden de compra son
- Parametros faltantes
- valor `sell_BTC_CLP` superior al ofertado. En este caso se recomienda solicitar nuevamente `/prices` y reintentar la orden
- valor `amount_BTC` supera stock disponible. Yaykuy maneja unos limites de stock de BTC disponibles para la operacion, si es posible intentar mas tarde o con un valor menor
- Hubo un error interno en la transacción


#### Enviar los datos bancarios donde enviar los CLP (`/bank_pay`)
Esta accion permite indicar al liquidador de CLP cuales son los datos a los cuales se deben enviar los CLP acordados en `amount_CLP` de la accion `/sell`. Una vez recibidos los BTC en la direccion `deposit_BTC` indicados en `/sell`, el liquidador enviara los CLP a la cuenta registrada con esta accion.

Los parametros de entrada son:

- `yky_code`      : Codigo yky que identifica la orden.
- `acc_bank`      : Nombre del banco 
- `acc_number`    : Numero de cuenta
- `acc_type`      : Tipo de cuenta: 'Corriente', 'Ahorro', 'Vista'
- `acc_name`      : Nombre completo del dueño de la cuenta
- `acc_run`       : RUN del dueño de la cuenta
- `acc_email`     : Email de aviso de transferencia.

#### Consideraciones del sell
Algunas consideraciones de operacion de Sell. 

- En caso que no se reciban los BTC dentro de un plazo prudente la orden se cancela.
- Se deben enviar los BTC exactos comprometidos en `/sell`
- Las transferencias se realizan manualmente por lo que pueden llgar a demorar entre 1 y 2 dias hábiles, sin embargo hacemos nuestro mejor esfuerzo para que el usuario reciba sus CLP lo antes posible.