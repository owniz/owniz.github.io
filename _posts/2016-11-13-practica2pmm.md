---
layout: post
title: Mi segunda APP para Android
category: [PMM, android]
permalink: "blog/segunda-app-android"
published: yes
---

<br>

Hola, después de [mi primera APP](/blog/primera-app-android) vuelvo con la segunda en la que incorporo más conceptos a entender por lo que recomiendo dar un vistazo al primer *post* para seguir el hilo de los conceptos.

Igual que la anterior podéis verla desde [mi repositorio de GitHub](https://github.com/owniz/AndroidPractise2 "GitHub").

<br>

<img class="differentSize65" src="/assets/img/practica2pmm/android.png" alt="android" style="margin:auto; display:block;">

# Conceptos a entender antes de nada

Repasito rápido de conceptos para entender la APP.

## *Fragments*

Con la llegada de pantallas más grandes (sobre todo para las *tables*) era difícil adaptar la interfaz para estas así que como solución nacieron los ***fragments***, podemos entenderlos como un contenedor de complementos que se nos mostrará en una porción de la interfaz aunque también puede ocupar toda, más adelante en la explicación de la aplicación se entenderá mejor este concepto viendo un par de fotos de como se ve en móvil y *tablet*.

## Interfaz

Con las interfaces podemos definir que conjunto de métodos tienen que implementarse cuando se utilices *implements* en una clase, es importante conocer que defines cuáles serán pero no su funcionamiento, por lo que el desarrollador que los implemente tiene que definir la lógica de éstos.

## Callbacks

Los **callbacks** son los métodos que definimos para pasar información entre clases. En esta práctica implementaremos una interfaz en las *activities* con unos métodos que hemos definido nosotros en cada *fragment* para poder compartir datos.

## Método *onSaveInstanceState()*

Este método lo podemos utilizar para guardar el estado de una aplicación, para por ejemplo recuperar los datos cuando giremos el dispositivo, ya que cada vez que giramos el dispositivo todo se vuelve a inflar, estos datos los podemos recuperar desde el ***Bundle*** que tienen como parámetro los métodos *onCreate()* y *onCreateView()* (método nuevo que veremos hoy).

# Explicación de la práctica.

<img class="inlinetwo" src="/assets/img/practica2pmm/main-movil.gif" alt="mainactivity">
<img class="inlinetwo" src="/assets/img/practica2pmm/second-movil.gif" alt="secondactivity">

Esta aplicación tendrá dos tipos de vista según se abra desde un móvil o una *tablet* (gracias a los *fragments*) y además un tipo de vista diferente según si el dispositivo está en *portrait* o *landscape* como puede apreciarse en la imagen de arriba para su versión móvil, para versión *tablet* tenemos la imagen justo aquí debajo.

![tablet](/assets/img/practica2pmm/main-tablet.gif "Tablet")

Una vez vistas las diferencias entre el móvil y *tablet* podemos apreciar para que pueden servir los *fragments*, cuando lo ejecutamos en un móvil necesitamos una actividad que aloje cada uno de ellos mientras que en la *tablet* podemos decirle que la primera actividad contenga a los dos para aprovechar mejor el extra de tamaño en la pantalla.

La función de esta prática es averiguar cómo podemos enviar y recibir datos entre fragmentos pasando por las actividades:

* Para **móvil** el recorrido es: fragmento del menú -> actividad principal -> actividad secundaria -> fragmento que pide dato.
* Para ***tablet*** es: fragmento del menú -> actividad principal -> fragmento que pide dato.

# Cómo podemos hacer esto.

En este *post* en vez de ir poniendo trozos de código explicando que hace cada uno me enfocaré en los cambios más significativos con respecto al de [mi primera APP](/blog/primera-app-android) ya que la gran mayoría de cosas están explicadas allí.

## Guardar estado

<img class="differentSize50" src="/assets/img/practica2pmm/save.gif" alt="save" style="margin:auto; display:block;">

Para poder guardar el estado de la aplicación hemos de sobrescribir el método *onSaveInstanceState()* indicando qué queremos que guarde, pondré como ejemplo el método que nos guarda el nombre que hemos introducido, el cual recupera el valor que tenemos en el **TextView**, en este caso `Nombre: hola`. Este método lo tenemos en la clase que contiene el *fragment*.

```java
@Override
public void onSaveInstanceState(Bundle savedInstanceState) {
    savedInstanceState.putString(ActivityMain.KEY_ETIQUETA, tvDatos.getText().toString());

    super.onSaveInstanceState(savedInstanceState);
}
```

Para poder recuperarlo hemos de indicarlo desde el método *onCreateView()* (los *fragments* en vez de *onCreate* tienen ese método) con estas simples líneas.

```java
public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle saveInstanceState) {
       View v = inflater.inflate(R.layout.fragmento_menu, container, false);

       // código del método

       // si se ha guardado algún valor cuando se giro la pantalla lo recuperamos
        if(saveInstanceState != null) {
            tvDatos.setText(saveInstanceState.getString(ActivityMain.KEY_ETIQUETA));
        }
}
```

## Interfaces

Para poder enviar los datos hemos definido que métodos se implementarán con las interfaces en el fragmento del menú por ejemplo:

```java
public interface FragmentoMenuListener {
    void onPideNombre();
    void onPideEdad();
    void onPideTelefono();
}
```

Cuando desde la actividad principal implementemos la interfaz del fragmento con `implements FragmentoMenu.FragmentoMenuListener` tendremos que sobrescribir esos métodos, como ejemplo tenemos el método que pone el nombre.

```java
// llama a la segunda actividad si pulsamos el botón "PEDIR NOMBRE"
@Override
public void onPideNombre() {

    // si es tablet llamamos al método que pone el nombre de la etiqueta y ademas activamos
    // botón y el EditText (están desactivados por defecto)
    if(isTablet) {
        fragmentoDetalle.ponEtiqueta(DESDE_NOMBRE);
        fragmentoDetalle.setEnabled(true);

    // si no es tablet mandamos los datos a la segunda actividad
    } else {
        startActivityForResult(getIntentToActivitySecond(DESDE_NOMBRE), REQUEST_CODE);
    }
}
```

Cuando definimos la interfaz hemos de crear un método que nos defina un *listener*, para ello creamos una variable:

```java
private FragmentoMenuListener escuchador;
```

Y luego con otro método *seteamos* el *listener* para que pueda ser utilizado:

```java
public void setFragmentoMenuListener(FragmentoMenuListener escuchador) {
    this.escuchador = escuchador;
}
```

Para que por ejemplo al apretar uno de los botones podamos comunicarnos con los métodos que piden los datos desde la primera actividad, por ejemplo el *onPideNombre()* nombrado anteriormente.

```java
@Override
public void onClick(View view) {
    if(botonNombre.getId() == view.getId())
        escuchador.onPideNombre();

    if(botonEdad.getId() == view.getId())
        escuchador.onPideEdad();

    if(botonTelefono.getId() == view.getId())
        escuchador.onPideTelefono();
}
```

Antes de terminar os dejo una imagen de cómo funciona la aplicación desde una *tablet* en la que podemos apreciar el envío de datos y cómo guarda los estados.

![tablet](/assets/img/practica2pmm/full-tablet.gif "Tablet")

Todos estos conceptos pueden parecer caóticos pero una vez los has puesto en práctica se entienden mejor, pero si tienes alguna duda de cómo funcionan te recomiendo que te pases por [mi repositorio de GitHub](https://github.com/owniz/AndroidPractise2 "GitHub") en el que está comentado todo el código parte por parte para que puedas entenderlo de una forma más cómoda, y si todavía te queda alguna duda no tengas problema en escribir a mi correo [iam@jmoral.es](mailto:iam@jmoral.es "iam@jmoral.es") o a mi Twitter [@owniz](https://twitter.com/owniz "Twitter").

Un saludo. ツ
