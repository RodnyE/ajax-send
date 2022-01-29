# AjaxSend instructions
 ## What is AjaxSend?
 AjaxSend is an agile and useful library that facilitates the use of the AJAX API, for real-time requests to the server, without having to reload the page.

 ## How to insert the library in my project?

 It is very easy to insert in an html project, just like in any other, just add it with the ```<script>``` tag and the attribute with the url where the file is located:
 ```
 <script src="/folder/ajax_send.min.js"></script>
 ```

 ## Basic functions
 Adding AjaxSend to your project will give you an object with which you can make requests:
 ```
 server
 ```


 ### GET request
 ```
 server.get(url, action)
 ```

 - `url`: {String} (required) you must enter the url where the get request will be made
 - `action`: {Function} (required) This function will be called when the request is completed, whether it was successful or not.  This function has two parameters, the first is the data that the server sent in the request, and the second is the status of the request (200 if it was successful, 404 if it was not found, 500 server error etc).

 *Example of use:*
 ```
 server.get("http://example.com/my-web", function(data, status){
     if(status === 200) console.log(data);
     else console.log("Oops! Error " + status)
 })
 ```

 ### POST request
 ```
 server.post(url, content, action)
 ```

 Similar to GET, but with the difference that with post you can send some information or data to the server:

 - `url`
 - `action`
 - `content`: {String or Object} can send either plain text (string) or an object (ajaxsend processes it as json)

 *Example of use*
 ```
 server.post("http://example.com/my-form",
   {
     username: "John",
     email: "john@mail.com",
     password: "123456"
   }, function(data, status){
     if(status === 200) console.log(data);
     else console.log("Oops! Error " + status)
 })
 ```

 ## advanced features
 AjaxSend has a function to specify and customize requests, its first and only parameter must be an object:
 ```
 server.process({
     URL,
     method,
     data,
     data_type,
     success,
     error,
     loading
 })
 ```

 - `url`: {String} (required) server url
 - `method`: {String} (required) method to be used in the request (GET, POST, PUT, DELETE, etc...)
 - `success`: {Function} (required) this function will be called if the request was completed successfully (200), assigns the data sent by the server.
 - `error`: {Function} (required) will be called if the request had errors, sets the status of the request to error(400, 404, 500, etc...)
 - `loading`: {Function} (optional) called simultaneously and several times during the request process, assigns the current state of the process (1, 2 or 3)
 - `data`: {String or Object} (optional) data to send to the server
 - `data_type`: {String} (optional) the type of data to be sent (json, x-www-form-urlencoded, txt, etc..)

 *Example of use*
 ```
 server.process({
     url: "http://example.com/my-form",
     method: "POST",
     data_type: "json",
     data:   {
         username: "John",
         email: "john@mail.com",
         password: "123456"
     },
    
     success: function(data){
         console.log("Perfect!!")
     },
    
     error: function(status){
         console.log("Oops!! Error " + status)
     },
 })
 ```
