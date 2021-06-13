---
title: "Superficies 3D en Java"
date: 2021-06-12T12:49:27+06:00
featureImage: images/allpost/java3d.png
postImage: images/allpost/java3d.png
draft: false
---
### Graficar en el espacio.
¿Necesitas renderizar objetos 3D y por casualidad utilizando lenguaje Java?, es cierto que es difícil ya que por la naturaleza del lenguaje es bastante vieja la información acerca de como graficar puntos en el plano pero en tercera dimensión, he aquí una solución.

### Netbeans
El entorno de desarrollo que utilizaremos es de entrada Netbeans IDE, nos permitira utilizar nuestra API [JZY3D](http://www.jzy3d.org/)

- Crear proyecto en Netbeans, recomendando la versión 8.2
- [Descargar](http://maven.jzy3d.org/releases/org/jzy3d/pyzy3d/1.0.2/) e incorporar pyzy3d-1.0.2.jar

### Ejemplo para cubo 3D ScatterDemo.java
El siguiente ejemplo muestra un cubo con 500,000 mil puntos distribuidos y acotados.
```java
package org.jzy3d.demos.scatter;

import java.util.Random;

import org.jzy3d.analysis.AbstractAnalysis;
import org.jzy3d.analysis.AnalysisLauncher;
import org.jzy3d.chart.factories.AWTChartComponentFactory;
import org.jzy3d.colors.Color;
import org.jzy3d.maths.Coord3d;
import org.jzy3d.plot3d.primitives.Scatter;
import org.jzy3d.plot3d.rendering.canvas.Quality;


public class ScatterDemo extends AbstractAnalysis {
    public static void main(String[] args) throws Exception {
        AnalysisLauncher.open(new ScatterDemo());
    }

    @Override
    public void init() {
        int size = 500000;
        float x;
        float y;
        float z;
        float a;

        Coord3d[] points = new Coord3d[size];
        Color[] colors = new Color[size];

        Random r = new Random();
        r.setSeed(0);

        for (int i = 0; i < size; i++) {
            x = r.nextFloat() - 0.5f;
            y = r.nextFloat() - 0.5f;
            z = r.nextFloat() - 0.5f;
            points[i] = new Coord3d(x, y, z);
            a = 0.25f;
            colors[i] = new Color(x, y, z, a);
        }

        Scatter scatter = new Scatter(points, colors);
        chart = AWTChartComponentFactory.chart(Quality.Advanced, "newt");
        chart.getScene().add(scatter);
    }
}
```

{{< blogsection image="https://i.ibb.co/6ZyqnS2/kmeans3d.png" title="K-MEANS 3D" >}}
Un ejemplo de aplicación de los demos, gracias al usuario liudongdong1, mi aplicación para ScatterDemo fue orientada a la agrupación de puntos con algoritmo KMEANS con distancia euclidiana en el espacio de tres dimensiones. Se puede observar la agrupación por colores acorde a la distancia de cada punto con todos los demás.
{{< /blogsection >}}

[Demos](https://github.com/liudongdong1/jzy3d-tutorials/tree/master/src/main/java/org/jzy3d/demos)

[K-MEANS 3D](https://github.com/MarqCervMartin/Sistemas-Expertos/tree/master/K-MEANS3D)

Vídeo [Vídeo de Java 3D](https://youtu.be/rYLt5_b_Bsc)

{{< blogsection image="https://i.ibb.co/1TktLRZ/java3d1.png" title="Graficar 3D en Java + JZY3D + Netbeans + Ejemplos." >}}  

{{< /blogsection >}}