---
layout: post
title: Aplicación para la gestión de una  biblioteca
category: [AD, java]
permalink: "blog/biblioteca"
published: yes
---

![biblioteca](/assets/img/biblioteca/biblioteca.png "Biblioteca")

Muy buenas a todos, después de hablar sobre **JDBC** y **MySQL** en [este](/blog/jdbc "JDBC") *post* y ver unas consultas básicas, no profundizaré mucho ahora en el código ya que tienes lo básico en el anterior, hoy vamos a ver una de las muchas posibles soluciones a la hora de tratar con una biblioteca y su base de datos (**BBDD**).

# Distribución de las clases

Lo primero que debemos pensar cuando encarrilamos un nuevo proyecto es cómo vamos a distribuir las clases, en mi caso he creado una clase que contendrá el menú principal, una clase para cada acción (consultar datos, actualizarlos, devolver un libro, etc) y por último una clase con métodos comunes para el resto de ellas. Me gustaría indicar que una vez realizado este proyecto si tuviese que volver a hacerlo además habría creado una clase para cada tabla de la BBDD ya que hubiera estado todo mucho más organizado tanto la parte visual como el código.

# Menús

Esta aplicación está diseñada para trabajar sin parte visual, es decir se trabajará todo desde consola.

## Menú principal

Cuando la ejecutemos nos aparecerá el menú de la imagen de abajo en el que podemos elegir la opción que necesitemos, iré por orden enseñando parte por parte cada una de ellas.

<img class="differentSize40" src="/assets/img/biblioteca/menu_general.jpg" alt="menu general" style="margin:auto; display:block;">

## Insertar datos

Una vez entramos a está sección tenemos que elegir en que tabla de la BBDD vamos a introducir los nuevos datos.

<img class="differentSize50" src="/assets/img/biblioteca/insertar_datos.jpg" alt="insertar datos" style="margin:auto; display:block;">

En nuestro caso insertaremos un nuevo socio, al elegir la tabla nos pedirá los datos pertinentes.

<img class="differentSize90" src="/assets/img/biblioteca/insertar_socio.jpg" alt="insertar socio" style="margin:auto; display:block;">

## Realizar prestamo

Cuando realizamos un préstamo nos da la opción de buscar por socio o sacar una lista con todos los socios actuales, accederemos a la primera opción por lo que nos preguntará un nombre, si tenemos coincidencia aparecerá el o los socios.

<img class="differentSize90" src="/assets/img/biblioteca/realizar_prestamo_1.jpg" alt="realizar prestamo" style="margin:auto; display:block;">

Al elegir uno de ellos nos saldrá una lista con los libros disponibles para que elijamos, por último nos preguntará si queremos realizar un nuevo préstamo.

<img class="differentSize90" src="/assets/img/biblioteca/realizar_prestamo_2.jpg" alt="realizar prestamo" style="margin:auto; display:block;">

## Consultar datos

Si accedemos a esta sección tendremos que elegir una de las tablas para mostrar todos los datos, si accedemos a la tabla de los prestamos además nos pedirá un nombre de usuario para ver sus prestamos.

<img class="differentSize90" src="/assets/img/biblioteca/consultar_datos.jpg" alt="consultar datos" style="margin:auto; display:block;">

## Actualizar datos

Cuando entremos a esta opción tendremos que elegir que tabla vamos a actualizar, luego buscaremos que socio o libro actualizar y nos preguntará los nuevos datos.

<img class="differentSize50" src="/assets/img/biblioteca/actualizar_datos.jpg" alt="actualizar datos" style="margin:auto; display:block;">

## Eliminar datos

Una vez dentro y elegida la tabla (en nuestro caso categoría) elegiremos cuál borrar o si queremos salir.

<img class="differentSize80" src="/assets/img/biblioteca/eliminar_datos.jpg" alt="eliminar datos" style="margin:auto; display:block;">

## Devolver libro

Dentro de está sección lo primero que nos pedirá la aplicación es el nombre de un socio, una vez escrito nos mostrará una lista con los libros a devolver para que seleccionemos uno de ellos y por último nos preguntará si queremos realizar otra devolución.

<img class="differentSize65" src="/assets/img/biblioteca/devolver_libro.jpg" alt="devolver libros" style="margin:auto; display:block;">

Este método recibe el título de un libro que hemos preguntado previamente para actualizar las tablas pertinentes de la base de datos.

```java
public static void devolverLibro(String tituloADevolver) {
    try {
        Statement st = MetodosComunes.conectarBaseDatos();

        st.executeUpdate("UPDATE prestamo p, libro l "
            + "SET p.fecha_devuelto = sysdate() "
            + "WHERE p.id_libro = l.id_libro "
            + "AND l.denominacion = '" + tituloADevolver + "';");
        st.executeUpdate("UPDATE prestamo p, libro l "
            + "SET dias_retraso_devolucion = DATEDIFF(sysdate(), fecha_devolucion) "
            + "WHERE p.id_libro = l.id_libro "
            + "AND l.denominacion = '" + tituloADevolver + "';");
        st.executeUpdate("UPDATE libro "
            + "SET devuelto = 1 "
            + "WHERE denominacion = '" + tituloADevolver + "';");

        st.close();

        System.out.println("\nLibro devuelto");

        preguntarOtraDevolucion();
    } catch(SQLException sqlE) {
        System.out.println("Error SQL");
        sqlE.printStackTrace();
    } catch(Exception e) {
        System.out.println("Error vario");
        e.printStackTrace();
    }
}
```

## Filtrado de prestamos

Aquí buscaremos entre los prestamos existentes según el filtro que seleccionemos, en el caso del ejemplo buscaremos por socio y rango de fechas.

<img class="differentSize90" src="/assets/img/biblioteca/filtrado_prestamos.jpg" alt="filtrado prestamos" style="margin:auto; display:block;">

Éste es el código utilizado para la búsqueda anterior, el método *filtradoSocioMasFecha()* primero de todo llama a los métodos que preguntan las fechas *pedirFechas()* y el nombre del socio *pedirNombreSocio()* para luego hacer la consulta con esos datos.

```java
public static void filtradoSocioMasFecha() {
    String[] fechas = pedirFechas(false, true, false);
    String socio = pedirNombreSocio();

    try {
        Statement st = MetodosComunes.conectarBaseDatos();

        ResultSet rsSocioMasFechas = st.executeQuery("SELECT * FROM prestamo p, socio s "
                    + "WHERE s.nombre = '" + socio + "' "
                    + "AND s.id_socio = p.id_socio "
                    + "AND p.fecha_recepcion "
                    + "BETWEEN '" + fechas[0] + "' "
                    + "AND '" + fechas[1] + "';");

        ConsultarDatos.mostraraDatosTablaPrestamo(rsSocioMasFechas);

        rsSocioMasFechas.close();
        st.close();

        System.out.print("\n¿Desea volver a buscar por nombre y fecha? (Si/No): ");
        String opcionElegida = Entrada.cadena();

        if(opcionElegida.toLowerCase().equals("si"))
            filtradoSocioMasFecha();
        else			
            elegirTipoFiltradoPrestamos();
    } catch(SQLException sqlE) {
        System.out.println("Error SQL");
        sqlE.printStackTrace();
    } catch(Exception e) {
        System.out.println("Error vario");
        e.printStackTrace();
    }
}

```

```java
public static String[] pedirFechas(boolean esFecha, boolean esSocio, boolean esTitulo) {
    System.out.println("\nAhora introducirá las fechas, primero la fecha más antigua;");

    String[] fechas = new String[2];

    int dia = 0;
    int mes = 0;
    int anyo = 0;
    boolean esValido = true;
    boolean tamanyoDia = false;
    boolean tamanyoMes = false;
    boolean tamanyoAnyo = false;

    for(int i = 0; i < 2; i++) {
        do {
            try {
                System.out.print("\tDime el día para la fecha número " + (i + 1) + ": ");
                dia = Entrada.entero();
                tamanyoDia = dia > 0 && dia < 32;
                esValido = false;
            } catch(NumberFormatException nfe) {
              System.out.println("\n\t¡Atención! Has introducido un carácter "
                  + "en vez de un número, por favor vuelva a introducir una "
                  + "opción correctamente\n");
                esValido = true;
            }
        }while(esValido);

        do {
            try {
                System.out.print("\tDime el mes para la fecha número " + (i + 1) + ": ");
                mes = Entrada.entero();
                tamanyoMes = mes > 0 && mes < 13;
                esValido = false;
            } catch(NumberFormatException nfe) {
              System.out.println("\n\t¡Atención! Has introducido un carácter "
                  + "en vez de un número, por favor vuelva a introducir una "
                  + "opción correctamente\n");
                esValido = true;
            }
        }while(esValido);

        do {
            try {
                System.out.print("\tDime el año para la fecha número " + (i + 1) + ": ");
                anyo = Entrada.entero();
                tamanyoAnyo = anyo > 1900 && anyo < 2200;
                esValido = false;
            } catch(NumberFormatException nfe) {
                System.out.println("\n\t¡Atención! Has introducido un carácter "
                    + "en vez de un número, por favor vuelva a introducir una "
                    + "opción correctamente\n");
                esValido = true;
            }
        }while(esValido);

        System.out.println();

        if(tamanyoDia && tamanyoMes && tamanyoAnyo) {
            fechas[i] = String.valueOf(anyo) + "-" + String.valueOf(mes)
                + "-" + String.valueOf(dia);
        } else {
            System.out.println("Fecha mal introducida");

            if(esFecha)
                filtradoRangoFechas();
            if(esSocio)
                filtradoSocioMasFecha();
            if(esTitulo)
                filtradoTituloMasFecha();
        }
    }

    return fechas;
}
```

```java
public static String pedirNombreSocio() {
    System.out.print("\n\tDime un nombre de socio: ");
    String socio = Entrada.cadena();

    return socio;
}
```

Como siempre cualquier duda o corrección no dudéis escribir a mi correo [iam@jmoral.es](mailto:iam@jmoral.es "iam@jmoral.es") o a mi Twitter [@owniz](https://twitter.com/owniz "Twitter").

Un saludo. ツ
