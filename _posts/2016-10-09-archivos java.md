---
layout: post
title: Gestión de archivos en Java
category: [AD, java]
permalink: "blog/IO-Java"
published: yes
---

<br>

Hola, hoy hablaré sobre como podemos trabajar con archivos desde Java, nombrando las clases más utilizadas para ello y como utilizarlas a través de algún ejemplo.

<img class="differentSize40" src="/assets/img/archivosjava/txt-java.png" alt="txt" style="margin:auto; display:block;">

# Archivos, ¿Qué son?

Antes de hablar sobre cómo gestionamos los archivos debemos de conocer qué son, para ello tenemos esta sencilla explicación: un archivo es un conjunto de datos estructurados guardados en algún medio de almacenamiento que pueden ser utilizados por aplicaciones.

Está compuesto por:

* **Nombre**: Identificación del archivo.
* **Extensión**: Indica el tipo de archivo.

Tenemos también esta definición más técnica sacada directamente desde la [Wikipedia](https://es.wikipedia.org/wiki/Archivo_(inform%C3%A1tica) "Wikipedia")

> Un archivo o fichero informático es un conjunto de bits que son almacenados en un dispositivo. Un archivo es identificado por un nombre y la descripción de la carpeta o directorio que lo contiene. A los archivos informáticos se les llama así porque son los equivalentes digitales de los archivos escritos en expedientes, tarjetas, libretas, papel o microfichas del entorno de oficina tradicional.

# Clase File

Las clases de lectura y escritura de archivos son susceptibles de generar excepciones, por lo que debemos tratarlas siempre con *try/catch* y además tener la buena costumbre de utilizar el método `close()` para cerrarlos.

```java
// Este ejemplo de uso del try/catch escribe en un archivo txt,
// el cual se ha creado desde Arguments, números aleatorios entre 1 y 10
try {
  FileWriter fichero = new FileWriter(args[0]);
  PrintWriter pw = new PrintWriter(fichero);

  for(int i = 0; i < 10; i++) {
    pw.println(i + 1);
    System.out.println(i + 1);
  }

  pw.close();
} catch(IOException e) {
  e.printStackTrace();
}
```

Esta clase nos permite crear los archivos utilizando:

```java
File nombreArchivo = new File(“/carpeta/archivo”);
```

Además disponemos de estos métodos para gestionarlos:

| Método | Descripción |
|:-:|:-|
| *creatNewFile()* | Crea (si se puede) el fichero indicado |
| *delete()* | Borra el fichero indicado |
| *mkdirs()* | Crea el directorio indicado |
| *getName()* | Devuelve un *String* con el nombre del fichero |
| *getPath()* | Devuelve un *String* con la ruta relativa |
| *getAbsolutePath()* | Devuelve un *String* con la ruta absoluta |
| *getParent()* | Devuelve un *String* con el directorio que tiene por encima |
| *renameTo()* | Renombra un fichero al nombre del fichero pasado como parámetro (se puede mover el fichero resultante a otra ruta, en caso de ser la misma se sobrescribirá) |
| *exists()* | *Boolean* que nos indicará si el fichero existe |
| *canWrite()* | *Boolean* que nos indicará si el fichero puede ser escrito |
| *canRead()* | *Boolean* que nos indicará si el fichero puede ser leído |
| *isFile()* | *Boolean* que indica si el fichero es un archivo |
| *listFiles()* | Método que devuelve un *array* con los ficheros contenidos en el directorio indicado |
| *isDirectory()* | *Boolean* que indica si el fichero es un directorio |
| *lastModified()* | Devuelve la última hora de modificación del archivo |
| *length()* | Devuelve la longitud del archivo |

# Clases FileWriter y PrintWriter

Con estas clases somos capaces de escribir en los archivos, por un lado tenemos **FileWriter** que nos permite escribir caracteres y se usa para escribir texto en un archivo de texto y por el otro lado **PrintWriter** que se utiliza para escribir en archivos de texto, o dicho de otra forma más coloquial, **FileWriter** nos prepara el archivo para que podamos escribir desde **Printwriter**.

```java
// Este ejemplo pregunta a un usuario unos datos para luego
// almacenarlos en el archivo ejercicio1.txt
int numPersonas = 5;

int[] edad = new int[numPersonas];
String[] nombre = new String[numPersonas];
String[] apellido = new String[numPersonas];

try {
  FileWriter fichero = new FileWriter("ejercicio1.txt");
  PrintWriter pw = new PrintWriter(fichero);

  for(int i = 0; i < numPersonas; i++) {
    System.out.print("\nDime el nombre del usuario " + (i + 1) + ": ");
    nombre[i] = Entrada.cadena();

    System.out.print("Dime el apellido del usuario " + (i + 1) + ": ");
    apellido[i] = Entrada.cadena();

    System.out.print("Dime la edad del usuario " + (i + 1) + ": ");
    edad[i] = Entrada.entero();

    pw.println("\nUsuario " + (i + 1) + "\tNombre: " + nombre[i] + "\tApellido: "
              + apellido[i] + "\tEdad: " + edad[i]);
  }
} catch(IOException e) {
  e.printStackTrace();
} finally {
  try {
    fichero.close();
  } catch (IOException e) {
    e.printStackTrace();
  }
```

De los métodos más útiles podemos destacar:


|:-:|:-|
| **FileWritter** ||
| *write()* | Escribe uno o varios caracteres |
| *flush()* | Escribe los datos almacenados en el *buffer* en el archivo |
| *close()* | Cierra **FileWriter** para terminar la gestión con el archivo (internamente llama al método *flush()*) |
| **PrintWriter** ||
| *println()* | Escribe en el archivo el parámetro que le introduzcamos |
| *append()* | Añade el carácter (*char*) o caracteres (CharSequence) específicos |
| *checkError()* | *Boolean* que llama al método *flush()* y comprueba si hay algún error |
| *close()* | Cierra **PrintWriter** para terminar la gestión con el archivo |

# Clases FileReader y BufferedReader

Estas clases son las que necesitaremos para el leer el contenido de los archivos, tenemos a **FileReader** que nos permite leer caracteres y se usa para leer el contenido de un archivo de texto y **BufferedReader** clase muy utilizada para leer archivos de texto plano. De una forma similar a las clases de escritura, **FileReader** nos prepara el archivo para leerlo desde **BufferedReader**.

```java
// Este fragmento de código lee un archivo
// y lo muestra por la consola
File f = new File("ejercicio1.txt");

try {
  FileReader fr = new FileReader(f);
  BufferedReader br = new BufferedReader(fr);

  String linea = br.readLine();

  System.out.println();

  while(linea != null) {
    System.out.println(linea);
    linea = br.readLine();
  }

  fr.close();
```

Los métodos con los que comúnmente trabajaremos son:

|:-:|:-|
| **FileReader** ||
| *mark()* | Marca la posición actual |
| *read()* | Lee uno o varios caracteres |
| *reset()* | Reinicia la lectura |
| *toString()* | Devuelve en forma de *String* el contenido del objeto |
| **BufferedReader** ||
| *readLine()* | Lee una línea de texto |
| *skip()* | Se salta "n" caracteres |
| *close()* | Cierra la lectura del archivo |

<br>

Todos estos ejemplos están sacados de una actividad que realicé donde [estudio](http://www.campusaula.com/ "Aula Campus") para el módulo de acceso a datos, así que os pondré el código entero (oculto para no extender mucho el *post*) por si alguien quiere darle un vistazo junto al enunciado de cada ejercicio (4 en total).

Para cualquier duda con respecto a este *post* o el código tenéis mi correo electrónico [iam@jmoral.es](mailto:iam@jmoral.es "iam@jmoral.es") para que podáis escribirme o mi Twitter [@owniz](https://twitter.com/owniz "Twitter").

[spoiler]

> Ejercicio 1
>
> * Crea una clase Ejercicio1 en la que se creará un fichero “ejercicio1.txt”.
> * Dicho fichero lo creará en el mismo directorio en el que se encuentra dicho proyecto del *workspace*.
> * El contenido del fichero será el nombre, apellido y edad de cinco personas que habremos solicitado al usuario.
> * Además de rellenar el contenido del fichero, mostraremos por pantalla dicha información.

```java
package es.jmoral.ad;

import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

public class Ejercicio1 {
	public static void main(String[] args) {

		int numPersonas = 5;

		int[] edad = new int[numPersonas];
		String[] nombre = new String[numPersonas];
		String[] apellido = new String[numPersonas];

		FileWriter fichero = null;
		PrintWriter pw = null;

		try {
			fichero = new FileWriter("ejercicio1.txt");
			pw = new PrintWriter(fichero);

			for(int i = 0; i < numPersonas; i++) {
				System.out.print("\nDime el nombre del usuario " + (i + 1) + ": ");
				nombre[i] = Entrada.cadena();

				System.out.print("Dime el apellido del usuario " + (i + 1) + ": ");
				apellido[i] = Entrada.cadena();

				System.out.print("Dime la edad del usuario " + (i + 1) + ": ");
				edad[i] = Entrada.entero();

				pw.println("\nUsuario " + (i + 1) + "\tNombre: " + nombre[i] + "\tApellido: " + apellido[i] + "\tEdad: " + edad[i]);
			}
		} catch(IOException e) {
			e.printStackTrace();
		} finally {
			try {
				fichero.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}

		System.out.println("\nDatos de usuario:");

		for(int i = 0; i < numPersonas; i++) {
			System.out.println("\n\tUsuario " + (i + 1) + "\n\t\tNombre: " + nombre[i] + "\n\t\tApellido: " + apellido[i] + "\n\t\tEdad: " + edad[i]);
		}
	}
}
```

[/spoiler]

[spoiler]

> Ejercicio 2
>
> * Crea una clase Ejercicio2 la cual accederá al ya creado archivo “ejercicio1.txt”.
> * Mostraremos por pantalla información referente a dicho archivo: nombre, ruta absoluta y longitud del fichero en bytes.
> * Leerá dicho contenido y lo mostrará por pantalla.
> * Una vez leído y mostrado el contenido del fichero volverá a acceder al mismo añadiendo la siguiente linea: “Añadiendo contenido del ejercicio02”.

```java
package es.jmoral.ad;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

public class Ejercicio2 {
	public static void main(String[] args) {

		File f = new File("ejercicio1.txt");

		try {

			FileReader fr = new FileReader(f);
			BufferedReader br = new BufferedReader(fr);

			System.out.println("\nNombre del fichero: " + f.getName());
			System.out.println("Ruta Absoluta del fichero: " + f.getAbsolutePath());
			System.out.println("Longitud del fichero en bytes: " + f.length());

			String linea = br.readLine();

			System.out.println();

			while(linea != null) {
				System.out.println(linea);
				linea = br.readLine();
			}

			fr.close();

			FileWriter fichero = new FileWriter("ejercicio1.txt", true);
			PrintWriter pw = new PrintWriter(fichero);

			pw.println();
			pw.println("A�adiendo contenido del ejercicio02");

			pw.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}

```

[/spoiler]

[spoiler]

> Ejercicio 3
>
> * Crea una clase Ejercicio3, recibirá como argumento de entrada un nombre de archivo “Entrada.txt”.
> * Utilizaremos dicho nombre para crear un archivo.
> * El contenido del fichero lo rellenaremos haciendo uso de un bucle *for* y escribiendo de 1 a 10.

```java
package es.jmoral.ad;

import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

public class Ejercicio3 {
	public static void main(String[] args) {

		try {

			FileWriter fichero = new FileWriter(args[0]);
			PrintWriter pw = new PrintWriter(fichero);

			for(int i = 0; i < 10; i++) {
				pw.println(i + 1);

				System.out.println(i + 1);
			}

			pw.close();
		} catch(IOException e) {
			e.printStackTrace();
		}
	}
}
```

[/spoiler]

[spoiler]

>Ejercicio 4
>
> * Creamos una clase Ejercicio4.
> * Leemos el contenido del archivo de texto “Entrada.txt” creado anteriormente.
> * A partir de ese contenido creamos un fichero de texto “Salida.txt” cuyo contenido sea el factorial de cada uno de los valores leídos del fichero Entrada.
> * Para calcular el factorial crear un método llamado factorial estático en la propia clase.
> * Por último mostrar los valores calculados con la función factorial.

```java
package es.jmoral.ad;

import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.PrintWriter;

public class Ejercicio4 {
	public static int factorial(int linea) {
		if(linea == 0)
			return 1;
		else
			return linea * factorialEstatico(linea - 1);
	}

	public static void main(String[] args) {

		String linea;

		int valorFact;

		File f = new File("Entrada.txt");

		try {
			FileReader fr = new FileReader(f);
			BufferedReader br = new BufferedReader(fr);

			linea = br.readLine();

			// create and prepare the new file
			FileWriter fw = new FileWriter("Salida.txt");
			PrintWriter pw = new PrintWriter(fw);

			while(linea != null) {

				valorFact = factorialEstatico(Integer.valueOf(linea));

				pw.println(valorFact);

				linea = br.readLine();

				System.out.println(valorFact);
			}
			pw.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}
```

[/spoiler]

Un saludo. ツ
