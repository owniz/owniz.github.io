---
layout: post
title: Hibernate y Spring
category: [AD, java]
permalink: "blog/hibernate-spring"
published: yes
---

<img class="differentSize50" src="/assets/img/hibernate/java.png" alt="Java" style="margin:auto; display:block;">

<br>

Hola a todos, hoy os escribiré sobre dos *frameworks* muy utilizados conjuntamente **[Hibernate](https://es.wikipedia.org/wiki/Hibernate "Hibernate (Wikipedia)")**  y **[Spring](https://es.wikipedia.org/wiki/Spring_Framework "Spring (Wikipedia)")**, para ello y como viene siendo habitual repasaremos un par de conceptos antes de continuar la explicación.

# Conceptos útiles

Si necesitáis refrescar la memoria tenéis abajo una pequeña explicación pero si queréis profundizar más podéis dar un vistazo a sus entradas de la **Wikipedia** *[framework](https://es.wikipedia.org/wiki/Framework "Framework (Wikipedia)")* y [ORM](https://es.wikipedia.org/wiki/Mapeo_objeto-relacional "ORM (Wikipedia)").

## *framework*

Es un concepto definido de muchas formas distintas y según a qué desarrollador le preguntes te contestará con una u otra por lo que intentaré definirlo de una forma sencilla, también desde mi punto de vista, podemos entender un *framework* como un conjunto de utilidades y herramientas proporcionadas al desarrollador para ayudarlo y mejorar su productividad.

## ORM (*Object-Relational mapping*)

Mapeo objeto-relacional más conocido por sus siglas en ingles ORM es un conjunto de técnicas utilizadas para convertir datos entre lenguajes de programación
orientado a objetos y bases de datos relacionales, es decir, con ORM somos capaces de transformar las tablas de una base de datos en una serie de entidades que ayudan al
programador para poder utilizarlas.

# Hibernate

<img class="differentSize65" src="/assets/img/hibernate/hibernate.png" alt="Hibernate" style="margin:auto; display:block;">

Sin más preámbulos y con los conceptos ya refrescados podemos ponernos en materia hablando de **Hibernate**, este *framework* ORM distribuido con una licencia [GNU LGPL](https://es.wikipedia.org/wiki/GNU_Lesser_General_Public_License "GNU LGPL (Wikipedia)") se utiliza junto con Java (aunque también hay una versión para .NET) para relacionar las bases de datos relacionales con Java (orientado a objetos), para ello *mapea* o asigna la relación a través de **archivos XML** como veremos en el ejemplo al final del *post*.

# Spring

<img class="differentSize65" src="/assets/img/hibernate/spring.png" alt="Spring" style="margin:auto; display:block;">

Cuando trabajamos con distintos *frameworks* cada uno de ellos crea sus propios objetos por lo que necesitamos que haya cierto orden entre éstos para poder trabajar conjuntamente, para ello tenemos el *framework* **Spring**, de código libre desarrollado para Java, el cual se encarga de construir todos los objetos que vamos a necesitar y asociarlos mediante **XML** en vez de hacerlo el propio desarrollador por lo que nos aseguramos que podamos trabajar de una forma unificada, integrada y de forma correcta.

# Ejemplo

En el siguiente ejemplo veremos como relacionar una tabla en **SQL** y la clase en **Java** a través del **XML** con **Hibernate** y **Spring**.

## Tabla en SQL

```sql
CREATE TABLE empleado (
   id INT(3) NOT NULL AUTO_INCREMENT,
   nombre VARCHAR(20) DEFAULT NULL,
   apellidos  VARCHAR(20) DEFAULT NULL,
   salario INT(10) DEFAULT NULL,
   PRIMARY KEY (id)
);
```

## Clase Java

```java
public class Empleado {
   private int id;
   private String nombre;
   private String apellidos;   
   private int salario;  

   public Empleado() {

   }

   public Empleado(String nombre, String apellidos, int salario) {
      this.nombre = nombre;
      this.apellidos = apellidos;
      this.salario = salario;
   }

   public int getId() {
      return id;
   }

   public void setId(int id) {
      this.id = id;
   }

   public String getNombre() {
      return nombre;
   }

   public void setNombre(String nombre) {
      this.nombre = nombre;
   }

   public String getApellidos() {
      return apellidos;
   }
   public void setApellidos(String apellidos) {
      this.apellidos = apellidos;
   }

   public int getSalario() {
      return salario;
   }

   public void setSalario(int salario) {
      this.salario = salario;
   }
}
```

## XML

```xml
<?xml version="1.0" encoding="utf-8"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD//EN"
 "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping>
   <class name="Empleado" table="empleado">
      <meta attribute="class-description">inferiorinferiorinferiorinferior
         Esta clase contiene los datos de los empleados.
      </meta>
      <id name="id" type="int" column="id">
         <generator class="native"/>
      </id>
      <property name="nombre" column="nombre" type="string"/>
      <property name="apellidos" column="apellidos" type="string"/>
      <property name="salario" column="salario" type="int"/>
   </class>
</hibernate-mapping>
```

Por último tan sólo despedirme y recordaros que para cualquier duda o corrección no dudéis en escribir a mi correo [iam@jmoral.es](mailto:iam@jmoral.es "iam@jmoral.es") o a mi Twitter [@owniz](https://twitter.com/owniz "Twitter").

Un saludo. ツ
