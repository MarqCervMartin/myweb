---
title: "OpenCV en React Native"
date: 2021-07-02T12:49:27+06:00
featureImage: images/allpost/opencv.jpg
postImage: images/allpost/opencv.jpg
draft: false
---

### OpenCV

En este blog, les compartire mi experiencia con OpenCV la libreria que es considerada como el estado del arte de la visión por computadora, en donde más de 2500 algoritmos optimizados pueden procesar imagenes o vídeo para el reconocimiento de objetos. 

#### React Native con OpenCV
Como parte de mi busqueda intentando incorporar opencv a react native para poder tener el poder en un dispositivo móvil tanto Android como iOS, únicamente pude implementar el [tutorial](https://brainhub.eu/library/opencv-react-native-image-processing/) brainhub muy completo pero algo enradado, pero que con paciencia podrás correr un ejemplo utilizando opencv en react native.


{{< blogsection image="https://i.ibb.co/pQ6KNJq/Open-CVExample.png" title="Ejemplo" >}}
{{< /blogsection >}}
El ejemplo nos muestra cuando una foto es borroza y cuando no, he aquí el vídeo

#### Configuración extra

Al tratar de realizar el tutorial, nos damos cuenta de algunos errores que se listan: 

##### ReactNativeCamera
Deberemos agregar en la siguiente ruta android/app/build.gradle
```
android {
  ...
  defaultConfig {
    ...
    missingDimensionStrategy 'react-native-camera', 'general' <-- insert this line
  }
}
```
Igualmente en la ruta android/build.gradle
```
allprojects {
    repositories {
        maven { url "https://jitpack.io" }
        maven { url "https://maven.google.com" }
    }
}
```
##### Native Libs
Deberemos agregar en la ruta android/app/build.gradle
```
android {
  ...
    packagingOptions {
        pickFirst 'lib/x86/libc++_shared.so'
        pickFirst 'lib/x86_64/libc++_shared.so'
        pickFirst 'lib/armeabi-v7a/libc++_shared.so'
        pickFirst 'lib/arm64-v8a/libc++_shared.so'
    }
}
```
##### Archivos incompletos, [descargar directorio src](https://drive.google.com/file/d/1hiPTG5O_9wXekX9yu_hh7UrQIp3wI7Be/view?usp=sharing)

