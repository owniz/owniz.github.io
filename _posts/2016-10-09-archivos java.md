---
layout: post
title: Gestión de archivos en Java
category: [AD, java]
permalink: "blog/IO-java"
published: yes
---

<br>

Hola lectores, hoy hablaré sobre cómo podemos trabajar con archivos desde Java, nombrando las clases más utilizadas para ello y cómo usarlas a través de algún ejemplo.

<img src="/assets/img/archivosjava/txt-java.png" alt="txt" style="width:35%; margin:auto; display:block;">

# Archivos, ¿qué son?

Antes de hablar sobre cómo gestionamos los archivos debemos de conocer qué son, para ello tenemos esta sencilla explicación: un archivo es un conjunto de datos estructurados guardados en algún medio de almacenamiento que pueden ser utilizados por aplicaciones.

Está compuesto por:

* **Nombre**: Identificación del archivo.
* **Extensión**: Indica el tipo de archivo.

Tenemos también esta definición más técnica sacada directamente de la [Wikipedia](https://es.wikipedia.org/wiki/Archivo_(inform%C3%A1tica) "Wikipedia")

> Un archivo o fichero informático es un conjunto de bits que son almacenados en un dispositivo. Un archivo es identificado por un nombre y la descripción de la carpeta o directorio que lo contiene. A los archivos informáticos se les llama así porque son los equivalentes digitales de los archivos escritos en expedientes, tarjetas, libretas, papel o microfichas del entorno de oficina tradicional.

# Clase File

La clase **File** además de proporcionarnos información sobre los archivos y directorios nos permite crearlos y eliminarlos.

Para ello esta clase nos permite crearlos utilizando:

```java
File nombreFile = new File(“/carpeta/archivo”);
```

y borrarlos con:

```java
nombreFile.delete();
```

Además de los anteriores disponemos de estos métodos para gestionarlos:

| Método | Descripción |
|:-:|:-|
| *createNewFile()* | Crea (si se puede) el fichero indicado |
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

Ya que somos capaces de trabajar tanto archivos como directorios tenemos métodos que nos permiten diferenciarlos. Debajo de estas líneas tenemos un ejemplo para ello.

```java
/* En este ejemplo asignamos la ruta del directorio utilizando
   la primera posición del array args[] del método main. En este segmento
   de código comprobamos si el array no está vacío para luego
   comprobar si el fichero es un directorio, en este caso
   guarda los nombres del contenido del directorio en un array
   para mostrarlos luego gracias al foreach */
if(args.length > 0) {
  File f = new File(args[0]);

    if(f.isDirectory()) {
      File[] ficheros = f.listFiles();

      System.out.println("Lista de los nombres de ficheros dentro del directorio");

      for(File file : ficheros)
        System.out.println("\t" + file.getName());
    }
}
```

A la hora de pasar el nombre de un archivo a través del array *args[]* podemos comprobar si éste existe o no, para que lo cree en caso contrario, y evitar posibles errores utilizando el código a continuación.

```java
if(args.length > 0) {
  File fichero = new File(args[0]);

  if(!fichero.exists()) {
    try {
      fichero.createNewFile();
    } catch (IOException e) {
      e.printStackTrace();
    }
  }
}
```

Las demás clases de lectura y escritura de archivos que veremos a continuación son susceptibles de generar excepciones, por lo que debemos tratarlas siempre con *try/catch* y además tener la buena costumbre de utilizar el método *`close()`* para cerrarlos.

Un ejemplo por adelantado:

```java
/* Este ejemplo de uso del try/catch escribe en un archivo txt,
   números aleatorios entre 1 y 10 */
try {
  FileWriter fl = new FileWriter("test.txt");
  PrintWriter pw = new PrintWriter(fl);

  for(int i = 0; i < 10; i++) {
    pw.println(i + 1);
    System.out.println(i + 1);
  }

  pw.close();
} catch(IOException e) {
  e.printStackTrace();
}
```

# Clases FileWriter y PrintWriter

Ambas clases sirven para escribir caracteres en ficheros, simplemente **PrintWriter** es una mejora de la clase **FileWriter** (comparten el mismo padre **java.io.Writer**) que nos permite utilizar métodos adicionales.

```java
/* Este ejemplo pregunta a un usuario unos datos para luego
   almacenarlos en el archivo ejercicio1.txt */
int numPersonas = 5;

int[] edad = new int[numPersonas];
String[] nombre = new String[numPersonas];
String[] apellido = new String[numPersonas];

try {
  /* Aunque no se utilice normalmente de esta forma aquí
     os pongo un ejemplo de uso para ambas clases */
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

  pw.close();
} catch(IOException e) {
  e.printStackTrace();
}
```

De los métodos más útiles podemos destacar:

|:-:|:-|
| **FileWritter** ||
| *write()* | Escribe uno o varios caracteres |
| *flush()* | Limpia el flujo de datos |
| *close()* | Cierra **FileWriter** para terminar la gestión con el archivo (internamente llama al método *flush()*) |
| **PrintWriter** ||
| *println()* | Escribe en el archivo el parámetro que le introduzcamos |
| *append()* | Añade el carácter (*char*) o caracteres (CharSequence) específicos |
| *checkError()* | *Boolean* que llama al método *flush()* y comprueba si hay algún error |
| *close()* | Cierra **PrintWriter** para terminar la gestión con el archivo |

# Clases FileReader y BufferedReader

Es una buena práctica utilizar estas clases conjuntamente pues **FileReader** simplemente lee caracteres de un fichero y **BufferedReader** nos ayuda a guardarlos en un *buffer* para tratarlos de una forma más segura.

```java
/* Este fragmento de código lee un archivo
   y lo muestra por la terminal */
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

  br.close();
} catch(IOException e) {
  e.printStackTrace();
}
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

## Comparar líneas

Si queremos comparar el texto que contiene un archivo con un texto que introduzcamos nosotros, a través de un *String* por ejemplo, hemos de ser conscientes que existe la posibilidad de que las mayúsculas no nos coincidan o incluso que se nos colara algún espacio al final o al principio del *String*, para ello podemos utilizar los métodos *`toLowerCase()`* para pasar todo el texto a minúscula o *`toUpperCase()`* para mayúscula y también utilizar el método *`trim()`* para quitar los espacios.

```java
String lineaAComparar = "hola mundo";

if(lineaArchivo.toLowerCase().trim().equals(lineaAComparar.toLowerCase().trim()))
   // Las líneas son iguales
```

Como viene siendo habitual antes de despedirme recordaros que para cualquier duda con respecto a este *post* o el código tenéis mi correo electrónico [iam@jmoral.es](mailto:iam@jmoral.es "iam@jmoral.es") para que podáis escribirme o contactar a través de mi  Twitter [@owniz](https://twitter.com/owniz "Twitter").

Un saludo. ツ
