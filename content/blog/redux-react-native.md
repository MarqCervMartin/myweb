---
title: "Redux en React Native"
date: 2021-04-10T12:49:27+06:00
featureImage: images/allpost/react-redux.jpg
postImage: images/allpost/react-redux.jpg
draft: false
---

La app de Finvero por su naturaleza es compleja y necesita del estado global que ofrece Redux, las instrucciones para incorporar nueva información al estado global o Store de redux son las siguiente:  

- Crear Action.
- Crear Reducer.
- Comprobar el store.

### Estructura de archivos
Lo primero es identificar la sección en donde viven los actions y reducers. La estructura de Finvero es la siguiente: 

![Estructura de archivos](https://i.ibb.co/vHfcSZQ/redux-1.png)

### Paso 1: Actions
Ahora bien necesitamos es añadir nuestro tipo de Action en la carpeta types, en index.js para almacenar a todos.  

![Paso 1: Actions](https://i.ibb.co/kJZM4FJ/redux-2.png)


```javascript
export const TOAST_NOTIFICATION = 'TOAST_NOTIFICATION'
```

Creamos nuestro Action en la carpeta actions.

![](https://i.ibb.co/gPzCnRv/redux-3.png)

En donde importamos nuestro type previamente exportado desde type/index.js. Otorgaremos un nombre a la función que ocuparemos más adelante para establecer nuestro estado en el store, unicamente le pasamos el tipo y la información a almacenar.

```javascript
import { TOAST_NOTIFICATION } from '../types'

export const setMessageToastNotification = (data) => {
  return (dispatch) => {
    dispatch({ type: TOAST_NOTIFICATION, payload: data })
  }
};
```
![](https://i.ibb.co/GTSvMHG/redux-4.png)

Listo, paso 1 completo. Solo nos aseguramos de exportarlo en el index de los Actions.

```javascript
export * from './ToastNotificationAction'
```
### Paso 2: Reducers
Nos movemos a la carpeta reducers y creamos el mismo archivo, solo que está vez sera un Reducer.js como el siguiente ejemplo: 

![Paso 2: Reducers](https://i.ibb.co/Qrv5dmq/redux-5.png)

```javascript
import {
    TOAST_NOTIFICATION
  } from '../types';
  
  const toastNotification = {
    message: '',
    title: '',
  }
  
  export default setTokenPushNotification = (state = toastNotification, action) => {
    switch (action.type) {
      case TOAST_NOTIFICATION:
        return { ...state, body: action.payload };
      default:
        return { ...state }
  
    }
    return state;
  }
```

![](https://i.ibb.co/G3vpCrz/redux-6.png)

Por último agregamos a nustro index nuestro Reducer, en la sección combineReducers. Habemus State en el Store!

### Comprobar Store
![](https://i.ibb.co/rG4yPc5/redux-7.png)

Es momento de probarlo, para ello podremos ir a la sección en donde llamamos a esa API, notificación, datos que generamos y quisieramos que toda la aplicación tuviera acceso a ellos.   
Para poder mandarlo a redux, necesitamos importar ese action y ejecutar el comando dispatch. Ejemplo para importar los Actions y ejecutar dispatch: 

```javascript
import * as Actions from '../PATH/actions';
      
dispatch(Actions.setMessageToastNotification(notification))

```
### Comprobar Store
Ahora extraemos esa información de el store de redux, probaremos en una screen especifica como la de perfil, para ello solo necesitaremos importar y asegurarnos bien de acceder al store : 

![Comprobar Store](https://i.ibb.co/7trbv7f/redux-8.png)

```javascript
import * as Actions from '../PATH/actions';
import { useDispatch, useSelector } from 'react-redux';

const ToastNotification = useSelector((store) => store.ToastNotification.body);

console.log("REDUX: ",ToastNotification)

```

Genial, ya podremos agregar mas Actions y estados a nuestro Store de Redux! 🥳