---
title: "Code Push en React Native"
date: 2020-07-13T13:06:26+06:00
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
### Logging in
Una vez instalado ejecutamos: 
```bash
appcenter login
```
Nos abrira el navegador y nos podremos loggear con los diferentes provedores de autenticación, una vez hecho esto nos proporcionara un token. Lo copiamos en nuestra terminal y listo.


![Token Loggin](https://docs.microsoft.com/en-us/appcenter/cli/images/terminallogin.png)

Podemos consultar nuestra información con el comando 
```bash
appcenter profile list
```

### Creando Release
 Una vez realizado el commit a actualizar, deberemos etiquetar el commit con la versión de release.
 ```bash
git tag {VERSIÓN_APP}-{VERSIÓN_CODEPUSH}
 ```
 Creamos versiones para los 2 SO.
```bash
appcenter codepush release-react -a {NOMBRE_APP_ANDROID} -d Production
appcenter codepush release-react -a {NOMBRE_APP_IOS} -d Production
```
Asegurandonos de la versión que existe en nuestro gradle, ejecutamos: 
```bash
appcenter codepush release-react -a MartiMar/Finvero -t "1.0.1" -d alpha
```
![Release Succes](https://i.ibb.co/zx9VKfG/code-push1.png)


### Listando Release Update Metadata
Con el siguiente comando podemos ver nuestra información de deployment.
```bash
code-push deployment ls Finvero -k
```
![Deplorment Data](https://i.ibb.co/SN79kQ8/code-push2.png)

### Estructura de proyecto CodePush by Axel Ramiro React Developer 

Jerarquía general
```
Organización
| Apps
| | Deployments
| | | Releases