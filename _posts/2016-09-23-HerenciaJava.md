---
layout: post
title: Herencia en Java
category: [AD, java]
permalink: "blog/herencia-java"
published: no
---

<br>

Hoy hablaremos sobre la herencia en Java, ¿qué es? ¿Por qué es importante? ¿Cómo utilizarla? y las palabras clave y conceptos que necesitamos conocer.

# Visibilidad

Antes de entrar en materia un pequeño repaso a la visibilidad o encapsulación en Java.

| Visibilidad | Desde la clase | Desde el paquete | Desde clase heredada | Todas las clases y paquetes |
|:-:|:-:|:-:|:-:|:-:|
| private | X |   |   |   |
| default | X | X |   |   |
| protected | X | X | X |   |
| public | X | X | X | X |

Gracias a esta tabla debemos ser capaces de entender desde donde serán accesibles los atributos o métodos según el acceso que le demos.

# herencia

Una vez teniendo la visibilidad refrescada podemos dar un vistazo a como esta estructurada una clase, en este caso utilizaré una clase **padre** llamada **Coche**.

## Clase Padre o Superclase

```java
public class Coche {

	// atributos
	private int cilindrada;
	private int potencia;
	
	// constructores
	public Coche() {
		cilindrada = 2000;
		potencia = 150; 
	}
	
	public Coche(int cilindrada, int potencia) {
		this.cilindrada = cilindrada;
		this.potencia = potencia;
	}
	
	// getters and setters
	public int getCilindrada() {
		return cilindrada;
	}
	
	public void setCilindrada(int cilindrada) {
		this.cilindrada = cilindrada;
	}
	
	
	public int getPotencia() {
		return potencia;
	}

	public void setPotencia(int potencia) {
		this.potencia = potencia;
	}
	
	// método que sobreescribe toString para devolver los valores
	@Override
	public String toString() {
		return "Cilindrada: " + this.cilindrada + "\n\tPotencia: " + this.potencia;
	}
}
```

En esta clase lo primero que vemos es `private int cilintrada` y `private int potencia` que es la forma de declarar los atributos de la clase y justo debajo tenemos los constructores 
que nos permitirán poder instanciar (o crear) objetos de esta clase, al primero le damos unos valores por defecto y en el segundo los introduciremos a la hora de instanciar los objetos.
Dentro de este segundo constructor tenemos `this.potencia = potencia` ¿qué significa esto?. Con **this** estamos haciendo referencia que estamos utilizando el atributo de esta clase, para evitar
un posible caso de confusión con otro atributo llamado igual en otra clase, y basicamente con esa linea estamos asignando el valor que introducimos al instanciar un objeto de la clase.

> Ejemplo instanciando un objeto asignando nosotros su valor.
> ´Coche coche = new Coche(1500, 100);´

Justo debajo tenemos los métodos, conocidos popularmente como, getters y setters que nos permitiran acceder a los atributos de la clase desde otras clases, ya que tienen los atributos 
definida su visibilidad como `private`.

Por último vemos un método con `@Override` y quizá os pregunteis ¿Qué significa esto? Pues muy sencillo, escribiendo eso antes de un método indicamos que lo vamos a sobreescribir, es decir,
ya existe un método llamado `toString` y nosotros vamos a sobreescribir ese método indicando que queremos que haga.

## Clase hija o subclase

Una vez tenemos la clase padre explicada vamos a ver un ejemplo de clase hija

```java
public class Deportivo extends Coche {
	
	// atributos
	protected String traccion;
	
	// constructor
	public Deportivo(int cilindrada, int potencia, String traccion) {
		super(cilindrada, potencia);
		
		this.traccion = traccion;
	}
	
	// método que sobreescribe toString para devolver los valores
	@Override
	public String toString() {
		return super.toString() + "\n\tTracción: " + this.traccion;
	}
}
```

Lo primero que tenemos que ser capaces de apreciar son las dos nuevas palabras que vemos al principio `extends Coche` con esto estamos diciendo que la clase Deportivo es hija de la clase Coche,
esto quiere decir que podremos utilizar los atributos y métodos de la clase Coche desde la clase Deportivo.

Justo debajo tenemos el atributo de la clase y puede que os pregunteis como podemos acceder a los atributos de la clase padre, pues muy sencillo, fijaos un momento en el constructor que está justo debajo,
¿qué podeis apreciar diferente con respecto al constructor de la clase padre?, ¿está claro verdad? le estamos pasando como datos los atributos de la clase Padre (Coche) pese a no declaralos en 
la clase hija (Deportivo) así que para poder llamar y utilizar esos atributos de la clase padre utilizamos un nuevo concepto `super`.

Por último volvemos a tener el método sobreescrito toString, pero en esta ocasión no hace falta que llamemos a los atributos de la clase padre obetenr sus valores, podemos utilizar el método **toString**
de está y luego añadir el atributo de la clase hija.
