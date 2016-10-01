---
layout: post
title: Herencia en Java
category: [AD, java]
permalink: "blog/herencia-java"
published: yes
---

<br>

Hoy os hablaré sobre la herencia en Java, qué es, por qué es importante, cómo utilizarla, las palabras clave y conceptos que necesitamos conocer.

# Visibilidad

Antes de entrar en materia un pequeño repaso en forma de tabla a la visibilidad o encapsulación en Java.

| Visibilidad | Desde la clase | Desde el paquete | Desde clase heredada | Todas las clases y paquetes |
|:-:|:-:|:-:|:-:|:-:|
| private | <i class="fa fa-check" aria-hidden="true"></i> |   |   |   |
| default | <i class="fa fa-check" aria-hidden="true"></i> | <i class="fa fa-check" aria-hidden="true"></i> |   |   |
| protected | <i class="fa fa-check" aria-hidden="true"></i> | <i class="fa fa-check" aria-hidden="true"></i> | <i class="fa fa-check" aria-hidden="true"></i> |   |
| public | <i class="fa fa-check" aria-hidden="true"></i> | <i class="fa fa-check" aria-hidden="true"></i> | <i class="fa fa-check" aria-hidden="true"></i> | <i class="fa fa-check" aria-hidden="true"></i> |

Ahora deberíamos ser capaces de entender desde donde serán accesibles los atributos o métodos según el acceso que le demos a la hora de declaralos.

# Herencia

<br>

<img class="differentSize65" src="/assets/img/herenciajava/diagrama.png" alt="diagrama" style="margin:auto; display:block;">

<br>

Una vez tenemos la visibilidad refrescada podemos dar un vistazo a cómo está estructurada una clase, en este caso utilizaré una clase **padre** llamada **Coche**.

## Clase padre o superclase

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
	
	// método que sobrescribe toString para mostrar los valores
	@Override
	public String toString() {
		return "Cilindrada: " + this.cilindrada + "\n\tPotencia: " + this.potencia;
	}
}
```

En esta clase lo primero que vemos es `private int cilintrada` y `private int potencia` que son la forma de declarar los atributos de la clase y justo debajo tenemos los constructores 
que nos permitirán poder instanciar objetos de esta clase, al primero le damos unos valores por defecto y en el segundo los introduciremos a la hora de instanciar los objetos.
Dentro de este segundo constructor tenemos `this.potencia = potencia` ¿qué significa esto?. Con **this** estamos haciendo referencia a la instancia concreta de ese objeto para evitar
un posible caso de confusión con otro atributo llamado de la misma forma en otra clase y en general con esa línea estamos asignando el valor al atributo que introduciremos al instanciar un objeto de la clase.

> Ejemplo instanciando un objeto asignando nosotros su valor:
>
> **Coche cocheUno = new Coche(1500, 100);**

Justo debajo tenemos los métodos, conocidos popularmente como getters y setters que nos permitirán acceder a los atributos de la clase desde otras clases, ya que tienen los atributos 
definida su visibilidad como `private`.

Por último vemos un método con `@Override` y quizá os preguntéis ¿qué significa esto? Pues muy sencillo, escribiendo esa línea antes de un método indicamos que lo vamos a sobrescribir, es decir
ya existe un método llamado `toString()` y nosotros vamos a sobrescribir ese método indicando qué queremos que haga.

## Clase hija o subclase

Una vez tenemos la clase padre explicada vamos a ver un ejemplo de clase hija.

```java
public class Deportivo extends Coche {
	
	// atributos
	protected String traccion;
	
	// constructor
	public Deportivo(int cilindrada, int potencia, String traccion) {
		super(cilindrada, potencia);
		
		this.traccion = traccion;
	}
	
	// método que sobrescribe toString para mostrar los valores
	@Override
	public String toString() {
		return super.toString() + "\n\tTracción: " + this.traccion;
	}
}
```

Lo primero que tenemos que ser capaces de apreciar son las dos nuevas palabras que vemos al principio `extends Coche` con esto estamos diciendo que la clase Deportivo es hija de la clase Coche,
esto quiere decir que podremos utilizar los atributos y métodos de la clase **Coche** desde la clase **Deportivo**.

Justo debajo tenemos el atributo de la clase y puede que os preguntéis cómo podemos acceder a los atributos de la clase padre, pues muy sencillo, fijaos un momento en el constructor que está justo debajo,
¿qué podéis apreciar diferente con respecto al constructor de la clase padre? Está claro ¿verdad? Le estamos pasando los atributos de la clase Padre **Coche** pese a no declararlos en 
la clase hija **Deportivo** así que para poder llamar y utilizar esos atributos de la clase padre utilizamos un nuevo concepto `super`.

Por último volvemos a tener el método sobrescrito `toString()`, pero en esta ocasión no hace falta que llamemos a los atributos de la clase padre para obtener sus valores, podemos utilizar el método `toString()`
de ésta y luego añadir el atributo que queremos mostrar de la clase hija.

Y hasta aquí este pequeño repaso sobre la herencia en Java, espero que haya sido capaz de explicarlo de una forma sencilla y amena para todo el mundo pero sobretodo que lo hayáis entendido.

Como siempre para cualquier duda o sugerencia no tengais problemas en escribir a mi correo [iam@jmoral.es](mailto:iam@jmoral.es "iam@jmoral.es").

Un saludo. ツ
