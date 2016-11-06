---
layout: post
title: ArrayList y StringTokenizer para dummies
category: [AD, java]
permalink: "blog/ArrayList-StringTokenizer"
published: yes
---

<br>

Buenas, hoy vamos a hablar de dos clases muy utilizadas en Java ya que nos ofrecen una gran cantidad de posibilidades a la hora de organizar datos u objetos de forma dinámica con ArrayList y de separar cadenas de caracteres con StringTokenizer

# ArrayList

Heredado de la clase **List** se encuentra en el paquete `java.util.ArrayList`, nos permite almacenar datos de una forma similar a un array típico pero con algunas notables diferencias. Para empezar este array será dinámico, ventaja que nos permite utilizarlo sin declarar un tamaño previamente (a medida que vayamos añadiendo o quitando objetos, su tamaño irá variando). Otra característica interesante es el poder almacenar objetos de distinto tipo en el mismo ArrayList.

Por ejemplo, para declarar un ArrayList de tipo String se haría de la siguiente forma:

```java
ArrayList<String> nombreArrayList = new ArrayList<>();
```

<p id="metodos">Los métodos más utilizados y que debemos conocer para esta clase son los siguientes:</p>

| Método | Descripción |
|:-:|:-|
| .add(elemento); | Añade el elemento al ArrayList |
| .add(n, elemento); | Añade el elemento al ArrayList en la posición 'n' y si existe desplaza una posición los elementos |
| .set(n, elemento); | Reemplaza el valor siendo 'n' la posición |
| .size(); | Devuelve el número de elementos del ArrayList |
| .get(n); | Devuelve el elemento que esta en la posición 'n' del ArrayList |
| .contains(elemento); | Comprueba si existe el elemento que se le pasa como parámetro |
| .indexOf(elemento); | Devuelve la posición de la primera ocurrencia en el ArrayList |
| .lastIndexOf(elemento); | Devuelve la posición de la última ocurrencia en el ArrayList |
| .remove(n); | Borra el elemento de la posición 'n' del ArrayList |
| .remove(elemento); | Borra la primera ocurrencia del elemento que se le pasa como parámetro |
| .clear(); | Borra todos los elementos de ArrayList |
| isEmpty() | Devuelve *true* si el ArrayList esta vacío. Si no, Devuelve *false* |

# StringTokenizer

La clase StringTokenizer proviene del paquete `java.util.StringTokenizer`. Esta clase se encarga de dividir una cadena de texto (String) en otras más pequeñas, *substrings* o *tokens* como se le conoce realmente. Para separarlo hace uso de unos delimitadores que podemos indicarle nosotros, aunque por defecto utiliza comas o espacios.
Es decir, el String "Hola Mundo" lo dividiría en "Hola" + "Mundo".

Para declarar nuestro StringTokenizer lo haremos de la siguiente forma:

```java
StringTokenizer st = new StringTokenizer("Hola Mundo");
```

Si queremos especificar los delimitadores que queremos utilizar lo haremos así:

```java
// En este caso utilizaríamos ", " como delimitador
StringTokenizer st = new StringTokenizer("Hola, Mundo", ", ")
```

Los métodos que utilizaremos son los siguientes:

| Método | Descripción |
|:-:|:-|
| .hasMoreTokens();  | Para saber si quedan más *tokens* por leer |
| nextToken(); | Para avanzar en la lectura de un *token* |
| .countTokens();  | Cuenta cuantos *tokens* contiene el String |

Un ejemplo para recorrer un String y mostrarlo por pantalla sería el siguiente:

```java
StringTokenizer st = new StringTokenizer("Texto de ejemplo");
    
while (st.hasMoreTokens()) {
	System.out.println(st.nextToken());
}
```

Obtendríamos esto:

> Texto
>
> de
>
> ejemplo

# Ejemplo práctico

Una vez explicada la teoría podemos ver un ejemplo para el uso de estas dos clases. Para ello daremos un vistazo rápido a una práctica que realicé [donde estudio](http://www.campusaula.com/ "Aula Campus").

Debajo de estas líneas tenéis el código completo en *spoiler* por si queréis verlo entero pero voy a ir desglosándolo poco a poco para verlo de una forma más cómoda y amena.

[spoiler]

```java
package es.jmoral.ad;

import java.util.ArrayList;
import java.util.StringTokenizer;

public class PruebaArrayList {
	public static void ejercicio01() {
		int numEquipos = 5;
		
		ArrayList<String> liga = new ArrayList<>();
		ArrayList<String> liga2 = new ArrayList<>();
		
		System.out.println();
		
		for(int i = 0; i < numEquipos; i++) {
			System.out.print("Dime el equipo número " + (i + 1) + ": ");
			liga.add(Entrada.cadena());
		}
		
		liga2 =  liga;
		
		System.out.println("\n\tArrayList liga: " + liga);
		System.out.println("\tArrayList liga2: " + liga2);
		
		System.out.println("\n\tCantidad de valores del ArrayList liga es: " + liga.size());
		
		liga.remove(4);
		System.out.println("\n\tPosición 4 del ArrayList liga borrado: " + liga);
		
		System.out.print("\nDime un nuevo equipo para la posición 2 del ArrayList: ");
		String posicion2 = Entrada.cadena();
		liga.set(2, posicion2);
		System.out.println("\n\tPosicón 2 del ArrayList liga sustituido: " + liga);
		
		System.out.print("\nDime el equipo que esta en la posicion 3 del Arralist: ");
		String posicion3 = Entrada.cadena();
		liga.remove(posicion3);
		System.out.println("\n\tArrayList liga con la posición 3 borrada: " + liga);
	}
	
	public static ArrayList<String> ejercicio02() {
		int numUsuario = 5;
		
		ArrayList<String> nombreEdad = new ArrayList<>();
		
		for(int i =0; i < numUsuario; i++) {
			System.out.print("\nDime el nombre del usuario número " + (i + 1) + ": ");
			nombreEdad.add(Entrada.cadena());
			
			System.out.print("Dime la edad del usuario número " + (i + 1) + ": ");
			nombreEdad.add(Entrada.cadena());
		}
		return nombreEdad;
	}
	
	public static void ejercicio03(ArrayList<String> nombreEdad) {
		String datos = "";
		
		for(int i = 0; i < nombreEdad.size(); i++) {
			if(i < (nombreEdad.size() - 1))
				datos += nombreEdad.get(i) + ", ";
			else
				datos += nombreEdad.get(i);
		}
		
		System.out.println("\n\tDatos del ArrayList separados por \", \": " + datos);

		StringTokenizer token = new StringTokenizer(datos, ", ");

		System.out.println("\n\tDatos separados con StringTokenizer:");
		while(token.hasMoreTokens()) {
			System.out.println("\t\t" + token.nextToken());
		}
	}
	
	public static void main(String[] args) {
		boolean exit = false;
		
		ArrayList<String> arrayMetodoEjercicio02 = new ArrayList<>();
		
		while(!exit) {
			System.out.println("\nElige el método");
			System.out.println("\t(1) Método que trabaja con 2 ArrayList pidiendo nombres de equipos");
			System.out.println("\t(2) Método que pide el nombre y la edad de dos usuarios, devuelve luego todo en un ArrayList");
			System.out.println("\t(3) Método que llama al método 2 para trabajar con StringTokenizer");
			System.out.println("\t(4) Para salir del menú y cerrar el programa");
			System.out.print("Opcion elegida: ");
			int opcion = Entrada.entero();
		
			switch(opcion) {
				case 1:
					ejercicio01();
					break;
				case 2:
					arrayMetodoEjercicio02 = ejercicio02();
					System.out.println("\n\tDatos introducidos: " + arrayMetodoEjercicio02);
					break;
				case 3:
					if(arrayMetodoEjercicio02.isEmpty()) 
						System.out.println("\n\tPrimero llama al método 2 para rellenar el ArrayList");
					else 
						ejercicio03(arrayMetodoEjercicio02);
					break;
				case 4:
					exit = true;
					break;
				default:
					System.out.println("\n\tOpción mal introducida");
					break;
			}
		}
	}
}

```

[/spoiler]

Importamos las clases necesarias:

```java
import java.util.ArrayList;
import java.util.StringTokenizer;
```

Primer método con el que trabajamos algunos de los métodos del ArrayList explicados [arriba](#metodos):

```java
public static void ejercicio01() {
	int numEquipos = 5;
	
	ArrayList<String> liga = new ArrayList<>();
	ArrayList<String> liga2 = new ArrayList<>();
	
	System.out.println();
	
	for(int i = 0; i < numEquipos; i++) {
		System.out.print("Dime el equipo número " + (i + 1) + ": ");
		liga.add(Entrada.cadena());
	}
	
	liga2 =  liga;
	
	System.out.println("\n\tArrayList liga: " + liga);
	System.out.println("\tArrayList liga2: " + liga2);
	
	System.out.println("\n\tCantidad de valores del ArrayList liga: " + liga.size());
	
	liga.remove(4);
	System.out.println("\n\tPosición 4 del ArrayList liga borrado: " + liga);
	
	System.out.print("\nDime un nuevo equipo para la posición 2 del ArrayList: ");
	String posicion2 = Entrada.cadena();
	liga.set(2, posicion2);
	System.out.println("\n\tosicón 2 del ArrayList liga sustituido: " + liga);
	
	System.out.print("\nDime el equipo que esta en la posicion 3 del Arralist: ");
	String posicion3 = Entrada.cadena();
	liga.remove(posicion3);
	System.out.println("\n\tArrayList liga con la posición 3 borrada: " + liga);
}
```

Este método nos devuelve un ArrayList después de rellenarlo con los datos que introducimos:

```java
public static ArrayList<String> ejercicio02() {
	int numUsuario = 5;
	
	ArrayList<String> nombreEdad = new ArrayList<>();
		
	for(int i =0; i < numUsuario; i++) {
		System.out.print("\nDime el nombre del usuario número " + (i + 1) + ": ");
		nombreEdad.add(Entrada.cadena());
		
		System.out.print("Dime la edad del usuario número " + (i + 1) + ": ");
		nombreEdad.add(Entrada.cadena());
	}
		
	return nombreEdad;
	}
```

Como último método tenemos éste que hace uso del ArrayList que devuelve el anterior para separar los datos utilizando el StringTokenizer, para ello primero los almacena en un String separándolos con una coma y espacio `", "`:

```java
public static void ejercicio03(ArrayList<String> nombreEdad) {
	
	String datos = "";
		
	for(int i = 0; i < nombreEdad.size(); i++) {
			if(i < (nombreEdad.size() - 1))
				datos += nombreEdad.get(i) + ", ";
			else
				datos += nombreEdad.get(i);
	}
		
	System.out.println("\n\tDatos del ArrayList separados por \", \": " + datos);

	StringTokenizer token = new StringTokenizer(datos, ", ");

	System.out.println("\n\tDatos separados con StringTokenizer:");
	while(token.hasMoreTokens()) {
		System.out.println("\t\t" + token.nextToken());
	}
}
```

Y para finalizar tenemos este pequeño menú para llamar a los métodos:

```java
public static void main(String[] args) {
	boolean exit = false;
	
	ArrayList<String> arrayMetodoEjercicio02 = new ArrayList<>();
	
	while(!exit) {
		System.out.println("\nElige el método");
		System.out.println("\t(1) Método que trabaja con 2 ArrayList pidiendo nombres de equipos");
		System.out.println("\t(2) Método que pide el nombre y la edad de dos usuarios, devuelve luego todo en un ArrayList");
		System.out.println("\t(3) Método que llama al método 2 para trabajar con StringTokenizer");
		System.out.println("\t(4) Para salir del menú y cerrar el programa");
		System.out.print("Opcion elegida: ");
		int opcion = Entrada.entero();
	
		switch(opcion) {
			case 1:
				ejercicio01();
				break;
			case 2:
				arrayMetodoEjercicio02 = ejercicio02();
				System.out.println("\n\tDatos introducidos: " + arrayMetodoEjercicio02);
				break;
			case 3:
				if(arrayMetodoEjercicio02.isEmpty()) 
					System.out.println("\n\tPrimero llama al método 2 para rellenar el ArrayList");
				else 
					ejercicio03(arrayMetodoEjercicio02);
				break;
			case 4:
				exit = true;
				break;
			default:
				System.out.println("\n\tOpción mal introducida");
				break;
		}
	}
}
```

<br>

Como viene siendo habitual os dejo mi correo electrónico de contacto [iam@jmoral.es](mailto:iam@jmoral.es "iam@jmoral.es") para que podáis escribirme si os surge alguna duda con respecto a este *post* o el código.

Un saludo. ツ
