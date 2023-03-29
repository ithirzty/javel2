# Javel2 - A fast and simple HTTP framework for [<img src="https://bah-lang.xyz/logo.png" alt="b" height=30px>ah](bah-lang.xyz)
This is the second version of [javel](https://github.com/ithirzty/javel), better, faster, prettier.

## Get started
Install using BPM:
```sh
bpm -install ithirzty/javel2
```

## Documentation
> ### Table of contents
> 1. **javel2** structure
> 2. **javel2_route** structure
> 3. **javel2_stream** structure
> 4. HTML templating system

---

1. ![structure](assets/struct.svg) **javel2** *(extend http_server)* The javel2 HTTP server instance.
    - ![structure](assets/var.svg) **certFile**: `cpstring` Path to your SSL certificate (PEM file).
    - ![structure](assets/var.svg) **keyFile**: `cpstring` Path to your SSL private key (PEM file).
    - ![structure](assets/var.svg) **routes**: `[]javel2_route*` Array containing all your routes.
    - ![structure](assets/func.svg) **launch**: `launch()` Launch the javel2 HTTP server. This function will block the calling thread.

---

2. ![structure](assets/struct.svg) **javel2_route** A javel2 route.
    - ![structure](assets/var.svg) **base**: `cpstring` Path to the root of your route in your file-system.
    - ![structure](assets/var.svg) **path**: `cpstring` Path to your route in the URL.
    - ![structure](assets/var.svg) **cacheMaxAge**: `int` If >= 0, the number of seconds that this route's request should be kept in cache (will not work if you set a custom handle).
    - ![structure](assets/var.svg) **disableParsing**: `bool` Will disable HTML parsing (will not work if you set a custom handle). This is set to false by default.
    - ![structure](assets/func.svg) **handle**: `function(javel2_route*, http_request*, http_response*)` Custom handle (useful for making API...).
    - ![structure](assets/func.svg) **error**: `function(javel2_route*, int, http_request*, http_response*)` Errors handle, first int argument is the http error code that must be sent to the client.

---

3. ![structure](assets/struct.svg) **javel2_stream** A javel2 stream.
    - ![structure](assets/func.svg) **send**: `send(s cpstring) bool` Will send the `cpstring` s through the stream. Will return true on success and false on error (as if the client as disconnected).
    - ![structure](assets/func.svg) **sendBytes**: `send(arr []char) bool` Works as *send()* but will send an array on characters instead of a string of text.

---

4. **HTML templating system.**
    The HTML templating system works with the following tag: <% type:name %>.    
    - **<% function: myFunc %>** Will call the `myFunc(req http_request*) cpstring` function. **For this to work:** *myFunc* should have a single argument that is `http_request*` and return the HTML code as `cpstring`. Additionaly, the function should be [evaluable](https://bah-lang.xyz/doc/functions#call) (by using the keyword `#eval myFunc`).
    - **<% stream: myFunc %>** Will call the `myFunc(req http_request*, stream javel2_stream*)` function. The HTML code must be sent through the stream. This enables you to push HTML in real-time to the client.
    - **<% file: path/myFile.html %>** Will include the specified HTML file. The HTML file will be parsed.