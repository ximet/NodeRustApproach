
## How it works

1. The Rust function source code is in the src/lib.rs file.
2. Use the ssvmup tool to compile the Rust function into a WebAssembly module in the pkg directory.
3. Optional: Use the node node/app.js command to test the function locally in Node.js.
4. Upload the pkg/*.wasm file to the Second State FaaS service, OR to a Node.js server, to turn it into a web service.


### Docker 

build WebAssembly Module

```
$ docker build -t ssvm .
$ docker run -p 3000:3000 --rm -it -v $(pwd):/app ssvm
(docker) # cd /app
(docker) # ssvmup build
```

 
## How To Use:

### Option 1: Upload to the FaaS and test

Upload the wasm file in the pkg folder to the FaaS. Double check the .wasm file name before you upload.

```
(docker) # curl --location --request POST 'https://rpc.ssvm.secondstate.io:8081/api/executables' \
--header 'Content-Type: application/octet-stream' \
--header 'SSVM-Description: say hello' \
--data-binary '@pkg/hello_lib_bg.wasm'
```

The FaaS returns

```
{"wasm_id":161,"wasm_sha256":"0xfb413547a8aba56d0349603a7989e269f3846245e51804932b3e02bc0be4b665","usage_key":"00000000-0000-0000-0000-000000000000","admin_key":"00xxxxxx-xxxx-xxxx-xxxx-4adc960fd2b8"}
```

Make a function call via the web.

```
(docker) # curl --location --request POST 'https://rpc.ssvm.secondstate.io:8081/api/run/161/say' \
--header 'Content-Type: text/plain' \
--data-raw 'Second State FaaS'
hello Second State FaaS
```

You can easily incorporate this web service into your HTML web pages. [See how](https://www.secondstate.io/articles/getting-started-with-function-as-a-service-in-rust/#web-ui)

### Option 2: Local test and debug

From the first terminal window, start the Node.js application.

```
(docker) # node node/app.js
```

From a second terminal window, you can test the local server.

```
$ curl http://localhost:3000/?name=SSVM
hello SSVM
```
