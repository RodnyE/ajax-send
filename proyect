/*

 * AjaxSend Send
 * 1.7.1 beta

 */ 


"use-strict";
var server;

class AjaxSend {
    constructor() {
        var http = false;

        //crear instancia XMLHttpRequest
        if (window.XMLHttpRequest) {
            http = new XMLHttpRequest();
            if (http.overrideMimeType)
                http.overrideMimeType('text/xml');
        } else if (window.ActiveXObject) {
            try {
                http = new ActiveXObject("Msxml2.XMLHTTP");
            } catch (err) {
                try {
                    http = new ActiveXObject("Microsoft.XMLHTTP");
                } catch (err) {}
            }
        }

        //si el navegador no soporta la API
        if (!http) {
            window.alert('ERROR!\nAjax Send Library:\n\nIt is not possible to use AjaxSend in this navigator');
            console.error(new Error("It is not possible to use AjaxSend in this navigator"))

            return false;
        }
        this.http = http;
    }

     ////////////////////
    /////Método GET/////
   ////////////////////
    get(url, action, asincrono = true) {

        //realizar petición
        var http = this.http;
        http.onreadystatechange = actionListen;
        http.open('GET', url, asincrono);
        http.send();

        var this_ = this;
        function actionListen() {
            if (http.readyState == 4) {
                //al finalizar la petición
                action(this_.__json_parse(http.responseText), http.status)
            }
        }
        this.__ajax_set()
    }

     /////////////////////
    /////Método POST/////
   /////////////////////
    post(url, datos, action, asincrono = true) {

        //realizar petición
        var http = this.http;
        http.onreadystatechange = actionListen;
        http.open('POST', url, asincrono);
        http.setRequestHeader('Content-Type', 'application/'+(typeof datos == "object"?"json": 'x-www-form-urlencoded'));
        http.send(this.__json_send(datos));

        var this_ = this;
        function actionListen() {
            if (http.readyState == 4)
                //al finalizar la petición
            action(this_.__json_parse(http.responseText), http.status);
        }
        this.__ajax_set()
    }

      ///////////////////////
     ////// Petición ///////
    //// Personalizada ////
   ///////////////////////
    process(datos, asincrono = true) {
        var http = this.http;
        if (typeof datos == 'object') {

            //verificar si existen los parámetros requeridos
            if (datos.url && datos.method && datos.success) {
                http.onreadystatechange = actionListen;
                http.open(datos.method.toUpperCase(), datos.url, asincrono);

                //si se van a enviar datos
                if (!datos.data) {
                    http.setRequestHeader('Content-Type', 'application/'+(datos.data_type?datos.data_type: 'x-www-form-urlencoded'));
                    http.send(this.__json_send(datos.data));
                } else http.send();

                var this_ = this;
                function actionListen() {
                    if (http.readyState == 4) {
                        if (http.status == 200) {
                            //ejecutar success
                            datos.success(this_.__json_parse(http.responseText))
                        } else {
                            if (datos.error) {
                                //ejecutar error
                                datos.error(http.status);
                            }
                        }
                    } else if (datos.loading) 
                       //ejecutar loading
                       datos.loading(http.readyState);
                }
            } else {
                
                //si falta un parámetro requerido
                console.error(new Error("Arguments required:\nurl\nmethod\nsuccess"));
                window.alert("ERROR!\nAjax Send Library:\n\nArguments required:\nurl\nmethod\nsuccess");
            }

        } else {
            //si se asignó erróneamente otro dato a process
            console.error(new Error("The argument must be an object"));
            window.alert("Type ERROR\nAjax Send Library\n\nThe argument must be an object");
        }
        this.__ajax_set()
    }
    
    __ajax_set() {
        //reestablecer variable <server>
        server = new AjaxSend();
    }
    
    /*
    __form_parser(_obj){
      var _string = '';
      var i = 0;
      for (var elem in _obj) {
        if (i) _string += '&';
        else i++;
         _string += elem + '=' + _obj[elem].replace(/\&/g, "\\&").replace(/\=/g, "\\=");
      }
      return _string
    }
    */
    
    __json_send(_obj) {
        //preparar datos para enviar
        if (typeof _obj == "object") return JSON.strigify(_obj);
        else return String(_obj)
    }

    __json_parse(_string) {
        //preparar datos al recibir
        try {
            _string = JSON.parse(_string);
        } catch(_e) {}
        return _string
    }
};

//crear instancia con AjaxSend
server = new AjaxSend();
