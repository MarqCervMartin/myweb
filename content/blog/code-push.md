---
title: "Code Push en React Native"
date: 2021-06-11T13:06:26+06:00
featureImage: images/allpost/codepush.png
postImage: images/allpost/codepush.png
draft: false
---
## ¿Qué es CodePush?
CodePush es un plugin o servicio que nos ayuda a lanzar actualizaciones rápidas y no tener que esperar a compilar una aplicación en esté caso de React Native para luego ser lanzadas en las Stores. Actualmente está disponible en [App Center](https://appcenter.ms/) de Microsoft.

La intención del post es ir directo a la implementación para incorporar este servicio con un ejemplo de app real disponible en PlayStore.

Recomiendo leer la [documentación](https://github.com/microsoft/react-native-code-push) con mayor detenimiento que principalmente explica a detalle todos los pasos que se mostraran en este post.

### Descargando cli.
La versión más reciente es appcenter cli y podras descargarla con el siguiente comando: 
```bash
npm install -g appcenter-cli
```
Igualmente necesitaremos de code-push cli para listar las keys de deployment.
```bash
npm install -g code-push-cli
```
### Logging in
Una vez instalado ejecutamos: 
```bash
appcenter login
```
Para el caso de code-push cli:
```bash
code-push register
```


{{< blogsection image="https://docs.microsoft.com/en-us/appcenter/cli/images/terminallogin.png">}}
Nos abrira el navegador y nos podremos loggear con los diferentes provedores de autenticación, una vez hecho esto nos proporcionara un token. Lo copiamos en nuestra terminal y listo.

{{< /blogsection >}}
Podemos consultar nuestra información con el comando 
```bash
appcenter profile list
```
### Configurando Entorno
Como lo indica en su documentación originalmente las aplicaciones en CodePush tenían automáticamente dos despliegues (Staging y Production). En App Center, debe crearlas o mediante los siguientes comandos:
```bash
appcenter codepush deployment add -a <ownerName>/<appName> Staging
appcenter codepush deployment add -a <ownerName>/<appName> Production
```
##### Ejemplos: 
En mi caso particular cree dos despliegues para Android y iOS
#### Android
```bash
appcenter codepush deployment add -a FinveroApp/Finvero-Android Staging
```
Estás nos daran nuestras keys de despliegue.
{{< blogsection image="https://i.ibb.co/3pX3LVJ/Deployment-Android-key.png">}}
{{< /blogsection >}}

#### iOS

```bash
appcenter codepush deployment add -a FinveroApp/Finvero-iOS Staging
```
{{< blogsection image="https://i.ibb.co/dfbKwNt/Deploymenti-OS-key.png">}}
{{< /blogsection >}}

#### AppCenter
En caso de tener agregado las palicaciones podemos ocupar code-push cli para listar nuestra tabla con esas keys, para el mismo ejemplo: 

```bash
code-push deployment ls FinveroApp/Finvero-Android -k
```
{{< blogsection image="https://i.ibb.co/KNdVJpj/table-keys.png" >}}  
{{< /blogsection >}}
### Conectando Code-Push a un proyecto react native
1. Instalamos el paquete ya sea con yarn o npm

```bash
yarn add code-push
```
```bash
npm install code-push --save
```
2. En App.js agregamos codepush
Importamos
```javascript
import codePush from "react-native-code-push";
```
Agregamos configuración de actualización de forma Manual, para más referencia [aquí](https://github.com/Microsoft/react-native-code-push).
```javascript
let codePushOptions = { checkFrequency: codePush.CheckFrequency.MANUAL };
```
Envolvemos a nuestra App cuidando que no se declare como constante, Ejemplo de código completo:
``` javascript
import codePush from "react-native-code-push";

let codePushOptions = { checkFrequency: codePush.CheckFrequency.MANUAL };
let App: () => React$Node = () => {
  return (
    <Provider store={store}>
      <CodePushLoading/> {/* Componente personalizado para mostrar al usuario*/}
      <Validator component={AppNavigator} />
    </Provider>
  );
}
App = codePush(codePushOptions)(App);  // Wrap de codepush sobre componente principal App

export default App;

```
3. Agregar componente de descarga.
Para mostrar al usuario la descarga y porcentaje se propone el siguiente componente: [código ejemplo](https://gist.github.com/MarqCervMartin/eaa0e08ba2e3946cd64a23f6e1153c42) esto es gracias a los métodos de code-push en su [documentación](https://github.com/microsoft/react-native-code-push/blob/master/docs/api-js.md#codepushsync)

Por ahora el componente que se muestra: 

```javascript
    <CodePushLoading/> {/* Componente personalizado para mostrar al usuario*/}
```
Incluye etiquetas para mostrar lo Actualización y porcentaje: 
{{< blogsection image="https://i.ibb.co/GnmmNmV/Update-Percent.png" >}}  
{{< /blogsection >}}

4. Agregar Keys
Una vez instalado y agregado el componente es hora de agregar las claves, para el caso de Android en la dirección android/app/src/main/res/values/strings.xml
deberemos colocar la siguiente línea en donde la key sera el de nuestro ambiente de Stagging o Production.
```xml
    <string moduleConfig="true" name="CodePushDeploymentKey">rxbekzSrhf6TL5EubCdteHZwfQDUtCWHdZq</string>
```
Igualmente agregamos un archivo llamado appcenter-config.json en la ruta android/app/src/main/assets con la clave que está disponible en appcenter.ms
```json
    //appcenter-config.json
    {
        "app_secret": "8c62d662-fa3e-46c2-bb5c-e1c86396cffc"
    }
```

### Creando Release
#### Staging
Para prueba podremos realizar un release en Staging, para saber en que versión se encuentra el caso de Android podemos saberlo en la ruta: android/app/build.gradle y nos mostrara el versionName, en mi caso me encuentro en la versión 1.2.0, ejecutamos el siguiente comando como ejemplo mi Organización: FinveroApp y como aplicación Finvero-Android, como versión: 1.2.0 y en el ambiente Staging

```bash
appcenter codepush release-react -a FinveroApp/Finvero-Android -t "1.2.0" -d Staging
```
#### Production
Para producción: 
 Una vez realizado el commit a actualizar, deberemos etiquetar el commit con la versión de release.
 ```bash
git tag {VERSIÓN_APP}-{VERSIÓN_CODEPUSH}
 ```
 Creamos versiones para los 2 SO.
```bash
appcenter codepush release-react -a {NOMBRE_APP_ANDROID} -d Production
appcenter codepush release-react -a {NOMBRE_APP_IOS} -d Production
```
#### Realease exitoso
{{< blogsection image="https://i.ibb.co/h97Z4nK/code-push1-2.png" >}}  
{{< /blogsection >}}

<!---

### Listando Release Update Metadata
Con el siguiente comando podemos ver nuestra información de deployment.
```bash
code-push deployment ls Finvero -k
```
![Deplorment Data](https://i.ibb.co/SN79kQ8/code-push2.png)
-->

### Estructura de proyecto CodePush by Axel Ramiro React Developer 

Jerarquía general
```
Organización
    Apps
        Deployments
            Releases
```