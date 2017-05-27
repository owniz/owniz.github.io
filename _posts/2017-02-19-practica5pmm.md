---
layout: post
title: App de ejemplo que consulta una API REST
category: [PMM, android]
permalink: "blog/app-rest-ejemplo"
published: yes
---

<br>

!Hola!, volvemos una vez más al repaso de las prácticas que he realizado en el módulo de Programación Multimedia y dispositivos Móviles (PMM), en este caso el proyecto consistía en crear un pequeño servidor **PHP** utilizando el concepto **API REST** para poder realizar unas consultas a una base de datos MySQL alojada en él.

En esta práctica he utilizado todos los conocimientos adquiridos desde el principio del módulo, por lo que si queréis dar un vistazo a las anteriores prácticas entrad a [este enlace](http://localhost:4000/categorias#PMM "Prácticas Android") y en la categoría de **PMM** las tenéis todas. Como siempre el código completo está en [su repositorio en GitHub](https://github.com/owniz/AndroidPractise5 "GitHub").

<br>

<img class="differentSize65" src="/assets/img/practica4pmm/android.png" alt="android" style="margin:auto; display:block;">

# Conceptos a entender

Qué necesitamos conocer antes de entrar en detalle.

## API REST

Esta arquitectura se utiliza junto con el protocolo **HTTP** para obtener o generar datos desde operaciones en diferentes formatos como pueden ser **XML** o **JSON**. Las operaciones más importantes son: **POST** crear, **GET** leer y consultar, **PUT** editar y **DELETE** eliminar.

## URI *Uniform Resource Identifier*

Las operaciones de arriba se consultan a través de una **URI** (Uniform Resource Identifier) utilizada para identificar los recursos de la red a los que queramos acceder.

# Estructura del proyecto

<img class="differentSize40" src="/assets/img/practica5pmm/estructura.png" alt="estructura" style="margin:auto; display:block;">

El proyecto está estructurado como podéis observar arriba, por lo que si queréis dar un vistazo a [su código](https://github.com/owniz/AndroidPractise5/tree/master/app/src/main/java/es/jmoral/dam2/practicaevaluable5 "Práctica 5") no deberíais tener problema de encontrarlo rápidamente (además está bien comentado, o eso creo <i class="fa fa-smile-o" aria-hidden="true"></i>) y así podemos dejar este *post* libre de código, o con poco, y centrarnos en las explicaciones.

# Explicación de la práctica y código utilizado

<img class="differentSize50" src="/assets/img/practica5pmm/inicio.gif" alt="inicio" style="margin:auto; display:block;">

Nada mas abrir la aplicación un pequeño mensaje de ayuda en forma de **AlertDialog** nos indica brevemente cómo manejarnos por ella, además podemos volver a verlo si pulsamos sobre el *"three dots menu"* y le damos a Ayuda.

Esta aplicación simula un menú para los vendedores de un concesionario en el que podrán tanto modificar sus datos como vendedor, añadir coches o realizar ventas de ellos, todos los datos recibidos y enviados al servidor se hacen a través de **JSON**. En el **GIF** superior vemos una primera pestaña con una lista de vendedores, otra para borrar coches y otra para añadirlos.

<img class="inlinetwo" src="/assets/img/practica5pmm/add-coche.gif" alt="add-coche">
<img class="inlinetwo" src="/assets/img/practica5pmm/del-coche.gif" alt="del-coche">

Aquí podemos observar cómo añadir un coche y una vez añadido borrarlo, para ello hemos mandado un *Broadcast* a través de un *Intent* desde el fragmento de añadir coches al de borrarlo.

```java
// enviamos un BroadCcast para notificar la inserción del coche al
// fragmento que borra los coches para que actualice su Spinner
private void enviarBroadcastCoche() {
  final Intent updateIntent = new Intent(Constantes.ACTUALIZAR_COCHES);
  getActivity().sendBroadcast(updateIntent);
}
```

Ahora desde el fragmento de borrar hemos de colocar el código de abajo en el *onCreateView()* para que cada vez que hagamos una inserción se llame a ese método para que actualice el **Spinner**.

```java
// escucha el Broadcast que puede enviar el FragmentoAnyadir
// para actualizar la lista de coches
private void setBroadcastListener() {
    final IntentFilter intentFilter = new IntentFilter();
    intentFilter.addAction(Constantes.ACTUALIZAR_COCHES);

    broadcastReceiver = new BroadcastReceiver() {
        @Override
        public void onReceive(Context context, Intent intent) {

            // método que actualiza la lista de coches consultando
            // al servidor
            consultaListadoCoches();
        }
    };

    getActivity().registerReceiver(broadcastReceiver, intentFilter);
}
```

## Filtrado en un ListView

<img class="differentSize50" src="/assets/img/practica5pmm/filtro.gif" alt="filtro" style="margin:auto; display:block;">

Aquí vemos cómo hacer un filtro para el **ListView** implementando **TextWatcher** y sobrescribiendo su método *onTextChanged()* al que pasaremos la búsqueda escrita sobre el **EditText**.

```java
// filtra los datos del EditText para hacer búsqueda de vendedores
@Override
public void onTextChanged(CharSequence charSequence, int i, int i1, int i2) {
    arrayAdapter.getFilter().filter(charSequence);
}
```

## Datos del vendedor y realizar una venta

<img class="differentSize50" src="/assets/img/practica5pmm/datos-venta.gif" alt="datos-venta" style="margin:auto; display:block;">

Ahora podemos apreciar cómo al pulsar sobre un vendedor accedemos a otro fragmento en el que además de sus datos podemos ver las ventas que ha realizado y realizar más ventas si introducimos el precio y la matrícula del coche, si alguno de los campos estuviera vacío no nos dejaría realizar la venta.

## Modificar y borrar un vendedor

<img class="inlinetwo" src="/assets/img/practica5pmm/mod-vendedor.gif" alt="mod-vendedor">
<img class="inlinetwo" src="/assets/img/practica5pmm/del-vendedor.gif" alt="del-vendedor">

Por último si en vez de pulsar sobre un nombre, mantenemos la pulsación accederemos un **AlertDialog** en el que podremos modificar los datos del vendedor siempre y cuando todos los campos estén rellenados o también borrar el vendedor.

Como conclusión de esta práctica comentar que he podido utilizar todos los conceptos, o la gran mayoría de ellos, vistos durante el curso y si tienes curiosidad por cómo está hecha recordaros que podéis darle un vistazo a [su repositorio en GitHub](https://github.com/owniz/AndroidPractise5 "GitHub") o preguntarme cualquier duda en mi correo [iam@jmoral.es](mailto:iam@jmoral.es "iam@jmoral.es") o en mi Twitter [@owniz](https://twitter.com/owniz "Twitter").

Un saludo. ツ
