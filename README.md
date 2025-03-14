# integrador-FP
Integrador de aplicacion FirmaPeru en soluciones Javascript

## instalando

usando npm:

```bash
$ npm install ------
```
## Ejemplo

```js
let firmador = new FirmaPeruIntegrador({
        /*port : 48596, */
        maximumTry : 60,
        getParams : ()=>{
          let parametros={
            //ruta desde la cual se obtendran los parametros de configuracion, tiene que estar en POST
            "param_url"          : "http://127.0.0.1:8000/app/tramite/firma_token_params/",
            "param_token"        : "********",
            "document_extension" : "pdf"
          }
          return btoa(JSON.stringify(parametros))
        },
        signatureInit : ()=>{console.log('signatureInit')},
        signatureOk   : ()=>{console.log('signatureOk')},
        signatureCancel : (message)=>{console.log('signatureCancel')},//
        idApp   : 'addComponent',
        vervose : false
      })

```
### port : default 48596
puerto en el que se iniciara el servicio de FirmaPeru, solo instanciar en caso de personalizar
### maximumTry : default 60
variable que define la cantidad maxima de intentos de conexion con el servicio antes de detenerse
### getParams : default function(){}
funcion en la que se optienen los parametros de configuracion de FirmaPeru en base64
### signatureInit : default function(){}
funcion que se ejecuta cuando se inicia la firma
### signatureOk : default function(){}
funcion que se ejecuta cuando la firma termico con exito
### signatureCancel : default function(){}
uncion que se ejecuta cuando la firma fue cancelada
### idApp : default 'addComponent'
aqui se precisa el id de la etiqueta html que se usara para invocar el aplicativo
### vervose : default false
aqui se precisa el nivel de detalle de los logs a mostrarse en la consola



en un archivo index.html se pondria el siguiente contenido
el id de la etiqueta tiene que coincidir con la  variable idAPP

```html
    ...

    <div id="addComponent" style="display:none;"></div>
    ...

```

para revisar si es firmador esta en linea se usa el siguiente comando
```js

    firmador.startCheckServer()
```
el estado del firmador se accede de la siguiente manera
```js

    console.log(firmador.online)
    //true
```
cuando el servidor se encuentre en linea se puede iniciar el proceso de firma
los parametros de firma se tienen que configurar a traves de la funcion getParams()

```js
    firmador.startSignature()
```