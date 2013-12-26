## Yaykuy API

API de Yaykuy Bitcoin Chile
Version Inicial en: http://docs.yaykuy.apiary.io/

### Token
Para cualquier operacion se requiere el uso de un token. Existen token de desarrollo y token de produccion. Para solicitar un token enviar un email a api@yaykuy.cl

### Sell (Venta de BTC)
Un sell es una venta de BTC por parte del usuario. El usuario tiene BTC y desea recibir CLP. Un sell se divide en 3 pasos:

1. Solicitar precios (/prices)
2. Enviar orden de venta (/sell)
3. Enviar los datos bancarios donde enviar los CLP (/bank_pay)

#### Solicitar precios (/prices)
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


#### Enviar orden de venta (/sell)
(TODO)

#### Solicitar precios (/prices)
(TODO)