---
title: "Detox Android React Native"
date: 2021-09-09T12:49:27+06:00
featureImage: images/allpost/detox.png
postImage: images/allpost/detox.png
draft: false
---

Existen diferentes tipos de pruebas en el desarrollo de software.

Las pruebas Unitarias enfocadas a poder probar un componente como lo es un botón, un textinput y demás, básicamente asegurarse de que el usuario pueda dar click al botón y pueda ingresar texto.

Las pruebas de Integración que puede ser representada por la unión de diferentes componentes y asegurarse que todos se conformen y funcionen perfectamente.

Las pruebas punto a punto o E2E (end to end), se refieren a poder testear flujos de desarrollo completos, como lo podría ser la parte de registro de una aplicación.

## Detox

Detox es una herramiente desarrollada por la comunidad de wix que contiene dos importantes test runners, Espresso y Moca para poder realizar E2E testing.

En está ocasión podremos realizar la configuración para poder correr algunos tests de prueba y posteriormente integrarlos en nuestra aplicación.

Se recomienda leer la [documentación](https://github.com/wix/Detox), este blog tiene como intención poder hacer la configuración más rápido y sencillo.


## Emulador Android (AOSP)

Como en la documentación oficial nos comentan, para realizar este tipo de tests exigentes, necesitaremos de un emulador que contenga menos interfaz grafica y servicios de google que puedan entorpecer o alentar las pruebas, para ello google desarrollo AOSP (Android Open Source Project), la base abierta disponible para todo el mundo a diferencia de los emuladores tradicionales Android con los servicios de Google, este promete ser más liviano.

Unicamente debemos crearlo desde el sdkmanager de Android, [Crear Emulador AOSP](https://github.com/wix/Detox/blob/master/docs/Introduction.AndroidDevEnv.md#android-aosp-emulators)

## React Native Init

Crearemos nuestro proyecto de react native

```
npx react-native init Detox
```

Seguidamente nos movemos a nuestro nuevo proyecto
```
cd Detox
```
Deberemos instalar detox cli en nuestro proyecto y adicionalmente jest circus
```
yarn add --dev detox jest-circus
```
Y por último deberemos ejecutar detox init para inicializar detox con
```
detox init
```
Esto nos creara la siguiente carpeta e2e con los siguientes archivos: 

- e2e/firstTest.e2e.js
- e2e/environment.js
- e2e/config.json

Y el archivo 
- .detoxrc.json

### Detox Android

Configuraremos los archivos necesarios para poder correr pruebas de ejemplo en Android gracias al siguiente vídeo y canal de [Youtube - CreativeJE](https://www.youtube.com/watch?v=9jhxE3u16F4)

#### 1 .detoxrc.json
Modificaremos el objeto "android" por la siguiente configuración para encontrar el path de .apk y construir .apk en modo debug con el comando release.
```
"android": {
    "type": "android.apk",
    "binaryPath": "android/app/build/outputs/apk/debug/app-debug.apk",
    "build": "cd android ; ./gradlew assembleDebug assembleAndroidTest -DtestBuildType=debug ; cd -"
}
```
Igualmente en el objeto "emulator" especificamos el nombre de nuestro emulador AOSP
```
"emulator": {
    "type": "android.emulator",
    "device": {
        "avdName": "Pixel_API_29_AOSP"
    }
}
```
#### 2 App.js
El componente App.js contendra componentes a testear con testID
```
import React, {useState} from 'react';
import {View, Text, Button, TextInput, SafeAreaView, Alert} from 'react-native';

const App = () => {
  const [input, setInput] = useState();
  return (
    <SafeAreaView style={{flex: 1}}>
      <View style={{flex: 1, alignItems: 'center', justifyContent: 'center'}}>
        <Text testID="title" style={{fontSize: 26, textAlign: 'center'}}>Hello Detox!</Text>
        <TextInput
          testID="input"
          placeholder="Name"
          value={input}
          onChangeText={setInput}
          style={{padding: 5}}
        />
        <Button testID="button" title="Submit" onPress={() => Alert.alert(input)} />
      </View>
    </SafeAreaView>
  );
};

export default App;
```
#### 3 android/app/build.gradle
Deberemos agregar en diferentes secciones, lo siguiente

```
android{
    defaultConfig {
        ...
        testBuildType System.getProperty('testBuildType', 'debug')  // This will later be used to control the test apk build type
        testInstrumentationRunner 'androidx.test.runner.AndroidJUnitRunner'
    }
    buildTypes {
        release {
            ...
            // Typical pro-guard definitions
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            // Detox-specific additions to pro-guard
            proguardFile "${rootProject.projectDir}/../node_modules/detox/android/detox/proguard-rules-app.pro"
        }
    }
}

dependencies {
    implementation "androidx.swiperefreshlayout:swiperefreshlayout:1.0.0"

    //Add this line - detox dependencies
    androidTestImplementation('com.wix:detox:+')

```
#### 4 android/app/src/androidTest/java/com/com.detox/DetoxTest.java

Crearemos la siguiente ruta de carpetas con el archivo DetoxTest.java  

En donde reemplazaremos en dos ocasiones nuestro paquete, en la línea 2 y 32
```
// Replace "com.example" here and below with your app's package name from the top of MainActivity.java
package com.detox;

import com.wix.detox.Detox;
import com.wix.detox.config.DetoxConfig;

import org.junit.Rule;
import org.junit.Test;
import org.junit.runner.RunWith;

import androidx.test.ext.junit.runners.AndroidJUnit4;
import androidx.test.filters.LargeTest;
import androidx.test.rule.ActivityTestRule;

@RunWith(AndroidJUnit4.class)
@LargeTest
public class DetoxTest {
    // Replace 'MainActivity' with the value of android:name entry in
    // <activity> in AndroidManifest.xml
    @Rule
    public ActivityTestRule<MainActivity> mActivityRule = new ActivityTestRule<>(MainActivity.class, false, false);

    @Test
    public void runDetoxTests() {
        // This is optional - in case you've decided to integrate TestButler
        // See https://github.com/wix/Detox/blob/master/docs/Introduction.Android.md#8-test-butler-support-optional
        //TestButlerProbe.assertReadyIfInstalled();

        DetoxConfig detoxConfig = new DetoxConfig();
        detoxConfig.idlePolicyConfig.masterTimeoutSec = 90;
        detoxConfig.idlePolicyConfig.idleResourceTimeoutSec = 60;
        // Replace "com.example" here and below with your app's package name
        detoxConfig.rnContextLoadTimeoutSec = (com.detox.BuildConfig.DEBUG ? 180 : 60);

        Detox.runTests(mActivityRule, detoxConfig);
    }
}  
```
#### 5 android/app/src/main/AndroidManifest.xml
Añadiremos la línea para incluir el archivo xml/network_security_config
```
<application
    ...
    android:networkSecurityConfig="@xml/network_security_config"
    android:theme="@style/AppTheme">
```
#### 6 android/app/src/main/res/xml/network_security_config.xml
Crearemos la carpeta xml y el archivo network_security_config.xml
```
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <domain-config cleartextTrafficPermitted="true">
        <domain includeSubdomains="true">10.0.2.2</domain>
        <domain includeSubdomains="true">localhost</domain>
    </domain-config>
</network-security-config>
```

#### 7 android/build.gradle
Añadiremos las siguientes líneas en nuestro build.gradle pero antes deberemos instalar el plugin de kotlin y saber su versión.
{{< blogsection image="https://media.giphy.com/media/L78P7f9QNyaSZYrXtO/giphy.gif?cid=790b7611dfe67f203f07ab31b0228a51a25f1658ed2fd089&rid=giphy.gif&ct=g" title="Kotlin version" >}}


{{< /blogsection >}}  
Es importante descargar el plugin de Kotlin desde Android Studio y colocar la versión de tú kotlin.

```
buildscript {
    ext {
        ...
        //Colocar tú versión de kotlin
        kotlinVersion = "1.5.30"
    }
    dependencies {
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlinVersion"
    }
}
allprojects {
    repositories {
        maven {
            // All of Detox' artifacts are provided via the npm module
            url "$rootDir/../node_modules/detox/Detox-android"
        }
    }
}
```
#### 8 android/settings.gradle
Incluir en la línea 1 y 2 las siguientes líneas.
```
include ':detox'
project(':detox').projectDir = new File(rootProject.projectDir, '../node_modules/detox/android/detox')
...
```

#### 9 e2e/firstTest.e2e.js
Construiremos los tests para poder escribir en un input y dar click en un botón para lanzar un Alert con el mensaje en el input.
```
describe('Example', () => {
  beforeAll(async () => {
    await device.launchApp();
  });

  it('Should have hello text', async() => {
    await expect(element(by.id("title"))).toBeVisible();
  })

  const typedText = 'Test && Test';
  it('Should type Test && Test', async() => {
    const input = element(by.id("input"));
    await input.typeText(typedText);
  });

  it('Should press on the submit button', async() => {
    await (element(by.id("button"))).tap();
  })

  it('Should have a alert with typed text', async() => {
    await expect(element(by.text(typedText)).atIndex(0)).toBeVisible();
    await element(by.text('OK')).tap();
  })
});
```
En el siguiente [commit](https://github.com/MarqCervMartin/react-native-learn/commit/924146f28dd0cffaba9743f52324daa4a8dc651c) podrás observar detenidamente los cambios en cada archivo.  

Por último deberemos de ejecutar la aplicación y una vez teniendola corriendo ejecutamos detox build: 
```
npx detox build -c android
```
Una vez creado el build, ejecutamos detox test para visualizar detox:
```
npx detox test -c android
```

Felicidades!! ahora puedes probar Detox🥳.