---
title: "Crea tu blog fácil y rápido"
date: 2021-03-08T12:49:27+06:00
featureImage: images/allpost/hugo.png
postImage: images/allpost/hugo.png
---

¿Cómo cree, está página y este blog?

---

# Te presento a HUGO
Hugo es un generador de archivos Web estáticos escrito en Go y aunque hay otras herramientas con esta misma utilidad, con Hugo encontrarás muchos temas desarrollados por la comunidad, es fácil de configurar y los artículos que escribas son fácilmente exportables de un tema a otro.

## Primer Paso

___

---

***


## Descargar Hugo

Lo primero es identificar tu sistema operativo, Windows, MAC o alguna distro de Linux, perfecto es hora de entrar a https://gohugo.io/

Instalaremos para macOS:
```bash
$ brew install hugo
```
Instalaremos para Windows:
```bash
$ choco install hugo -confirm
```
Instalaremos para Linux:
```bash
$ snap install hugo
```


## Vídeo 1 Descarga

**¿Tienes alguna duda?**

Checa este [Vídeo de Intalación](https://youtu.be/aFBO45NVIhY)

{{< blogsection image="images/blog/hola-hugo/hugo-1.png" title="[1] Portafolio Digital HUGO + Blog + Deploy + Dominio Free Parte 1" >}}  

{{< /blogsection >}}

## Paso 2

Descargaremos un tema, para ello creamos un sitio con el siguiente comando:

```bash
$ hugo new site myweb
```
Felicidades, creamos nuestro nuevo sitio local HUGO, ahora accederemos e instalaremos un tema, pero antes iniciamos un repositorio git.

```bash
$ cd myweb
$ git init
```
Listo, ahora escogemos un tema, en este caso portio y ejecutamos:
```bash
$ git submodule add git@github.com:StaticMania/portio-hugo.git themes/portio
```
Teniendo instalado nuestro tema, ahora simplemente copiamos con el comando:
```bash
$ cp -a themes/portio/exampleSite/* .
```
Todo bien, ahora corremos de forma local nuestro sitio, mediante:

```bash
$ hugo server
```

Ahora HUGO nos dira en que puerto se encuentra nuestro servidor local, por ejemplo puede ser http://localhost:1313/ lo copiamos y vamos a nuestro navegador, listo!
## Vídeo 2 Web Local

**¿Tienes alguna duda?**

Checa este [Vídeo de Como correrlo local](https://youtu.be/iT19X0SehlA)

{{< blogsection image="images/blog/hola-hugo/hugo-2.png" title="[2] Portafolio Digital HUGO + Blog + Deploy + Dominio Free Parte 2" >}}  

{{< /blogsection >}}

## Paso 3 Deployment

Antes de realizar el despliegue, exploraremos nuestro sitio, modificar cosas elementales del config.toml en el siguiente vídeo lo mostraremos a detalle pero antes vamos a enlazar nuestro proyecto con un repositorio en GitHub o GitLab.  
Incluimos themes y realizamos un commit:
```bash
$ git add themes
$ git add .
$ git commit -m "Mi pagina web"
```
Cuando creamos un proyecto en GitHub nos muestra instrucciones para crear o linkear un repositorio que ya creamos, en este caso cuando aplicamos git init, creamos uno. En la segunda parte podremos ver algo así:
```bash
$ git remote add origin https://
$ git branch -M main
$ git push -u origin main
```
Deberemos reemplazar https por nuestro URL. Y listo deberíamos tener nuestro proyecto en GitHub  

Seguidamente realizaremos otro commit de configuración, deberemos crear un archivo llamado package.json que le dira a vercel que debe descargar HUGO. Nos vamos a nuestra raíz de archivos, dentro de la carpeta myweb y creamos un archivo package.json con extensión .json y luego pegamos lo siguiente:
```json
{
    "name": "myweb",
    "version": "0.1",
    "scripts": {
        "install": "curl -L -O https://github.com/gohugoio/hugo/releases/download/v0.58.3/hugo_0.58.3_Linux-64bit.tar.gz && tar -xzf hugo_0.58.3_Linux-64bit.tar.gz",
        "build": "./hugo"
    }
}
```
Podemos cambiar la versión de hugo, pero ya lo tenemos listo, hacemos un commit y push:

```bash
$ git add package.json
$ git commit -m "Añadiendo package"
$ git push origin main
```

Una vez teniendo nuestro package.json nos dirigimos a https://vercel.com/ y accedemos con GitHub, iniciamos un nuevo proyecto, seleccionamos nuestro repositorio de GitHub, le damos en deploy y si todo sale excelente, ya lo tenemos! nuestro sitio web. ¿Pero se ve algo raro no? eso es por que debemos de cambiar nuestra BASEURL, vercel nos da un nombre de dominio como myweb.vercel.com, copiamos y nos dirigimos a nuestro config.toml y en BASEURL pegamos la nueva ruta. Listo, hacemos commit: 
```bash
$ git add config.toml
$ git commit -m "Añadiendo nueva BASEURL"
$ git push origin main
```

Si visitamos nuestro sitio ahora se vera mejor.

## Vídeo 3 Deploy

**¿Tienes alguna duda?**

Checa este [Vídeo de Como correrlo local](https://youtu.be/iT19X0SehlA)

{{< blogsection image="images/blog/hola-hugo/hugo-3.png" title="[3] Portafolio Digital HUGO + Blog + Deploy + Dominio Free Parte 3" >}}  

{{< /blogsection >}}


## Final

Por último, quedaría añadir un dominio. Te recomendaría si eres estudiante aplicar al Student Pack de GitHub y obtener 1 año gratis en tu dominio ;)  
Gracias por leer.

BY Martín.

