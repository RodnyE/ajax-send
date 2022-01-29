# Ajax-Send
# Instrucciones de AjaxSend
## ¿Qué es AjaxSend?
AjaxSend es un librería ágil y útil que facilita el uso de la API de AJAX, para peticiones en tiempo real al servidor, sin tener que recargar la página.

## ¿Cómo insertar la librería en mi proyecto?

Es muy fácil de insertar en un proyecto html, como en cualquier otro, simplemente agréguela con la etiqueta ```<script>``` y el atributo con la url de donde esta ubicado el archivo:
```
<script src="/carpeta/ajax_send.min.js"></script>
```

## Funciones básicas
Al añadir AjaxSend a tu proyecto, te otorgará un objeto con el cual podrás realizar las peticiones: 
```
server
```


### Petición GET
```
server.get(url, action)
```

- `url`: {String} (requerido) debe introducir la url a donde realizará la peticion get
- `action`: {Function} (requerido) esta función será llamada cuando la petición sea completada, ya sea si fue satisfactoria o no. Esta función tiene dos parámetros, el primero son los datos que envió el servidor en la solicitud, y el segundo el estado de la peticion (200 si fue satisfactoria, 404 si no se encontró, 500 error del servidor etc).

*Ejemplo de uso:*
```
server.get("http://example.com/my-web", function(data, status){
    if(status === 200) console.log(data);
    else console.log("Upps! Error " + status)
})
```

### Petición POST
```
server.post(url, content, action)
```

Similar a GET, pero con la diferencia que con post podras enviarle alguna información o datos al servidor:

- `url`
- `action`
- `content`: {String or Object} puede enviar ya sea texto plano (string) o un objeto (ajaxsend lo procesa como json)

*Ejemplo de uso*
```
server.post("http://example.com/my-form", 
  {
    username: "Juan",
    email: "juan@mail.com",
    password: "123456"
  }, function(data, status){
    if(status === 200) console.log(data);
    else console.log("Upps! Error " + status)
})
```

## Funciones avanzadas
AjaxSend dispone de una función para especificar y personalizar las peticiones, su primer y único parámetro debe ser un objeto:
```
server.process({
    url,
    method,
    data,
    data_type,
    success,
    error,
    loading
})
```

- `url`: {String} (requerido) url del servidor
- `method`: {String} (requerido) metodo que se utilizará en la petición (GET, POST, PUT, DELETE, etc...)
- `success`: {Function} (requerido) se llamará esta función si la petición fue completada correctamente (200), asigna los datos enviados por el servidor.
- `error`: {Function} (requerido) se llamará si la petición tuvo errores, asigna el status de la peticion en error(400, 404, 500, etc...)
- `loading`: {Function} (opcional) se llama simultáneamente y varias veces durante el proceso de petición, asigna el estado actual del proceso (1, 2 o 3)
- `data`: {String o Object} (opcional) datos a enviar al servidor
- `data_type`: {String} (opcional) el tipo de dato que se enviará (json, x-www-form-urlencoded, txt, etc..)

*Ejemplo de uso*
```
server.process({
    url: "http://example.com/my-form",
    method: "POST",
    data_type: "json",
    data:   {
        username: "Juan",
        email: "juan@mail.com",
        password: "123456"
    },
    
    success: function(data){
        console.log("Perfect!!")
    },
    
    error: function(status){
        console.log("Upps!! Error " + status)
    },
})
```

