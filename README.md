# integrador-FP
Integrador de aplicacion FirmaPeru en soluciones Javascript

## instalando

usando npm:

```bash
$ npm install ------
```
## Ejemplo

```js
//
//la inicializacion de la instancia de axios da la posibilidad de personalizarla, con datos como el token de acceso
let instance = axios.create()

import FirmaPeruIntegrador from './FirmaPeruIntegrador';
// la ruta para obtener los parametros sera por POST a http://127.0.0.1/parametros/{file_id}'
let url_parametros 'http://127.0.0.1:8000/parametros'
let id_file=null
let firmador = new FirmaPeruIntegrador({
        //la instancia es opcional, de no pasarse el dato la libreria inicializara una nueva instancia
        instance      : instance,//la instancia de axios con la que se accederan a los requests
        maximumTry    : 60,
        idApp         : 'addComponent',//id de la etiqueta HTML en la que trabajara el componente
        vervose       : false,//si se pone true se imprimiran en consola cada paso de la firma, util en desarrollo
        signatureInit : () => {
            console.log('signatureInit')
            //acciones que se vayan a realizar cuando el proceso de la firma inicie
        },
        signatureOk   : () => {
            console.log('signatureOk')
            //acciones que se vayan a realizar cuando el proceso de la firma culmine satisfactoriamente
        },
        signatureCancel : () => {
            console.log('signatureCancel')
            //acciones que se vayan a realizar cuando el proceso de la firma se cancele
        },

        getParams     : () => {
            //metodo a traves del cual se obtendran los parametros de la firma
            let route = baseurl + "firmaperu/firma_token_params/"+id_file;
            let parametros = {
                "param_url"          : route,
                "param_token"        : "1212121212",//este parametro se puede personalizar segun la necesidad
                "document_extension" : "pdf"
            }
            //aqui se realiza el hasheo, tambien se puede usar axios para obtener los parametros hasheados desde el server
            return btoa(JSON.stringify(parametros))
        }
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
encender el firmador
```js

    firmador.runService()
```
el estado del firmador se accede de la siguiente manera
```js

    console.log(firmador.online)
    //true
```
cuando el servidor se encuentre en linea se puede iniciar el proceso de firma
los parametros de firma se tienen que configurar a traves de la funcion getParams()

```js
//iniciando la firma para el documento con file_id = 5
id_file=5
firmador.startSignature()

//iniciando la firma para el documento con file_id = 6
id_file=6
firmador.startSignature()
```