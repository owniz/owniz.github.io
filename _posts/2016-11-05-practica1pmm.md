---
layout: post
title: Mi primera APP para Android
category: [PMM, android]
permalink: "blog/primera-app-android"
published: yes
---

<br>

Hola a todos, hoy escribiré sobre la primera aplicación que he hecho para Android, bueno no es realmente la primera pero si la que tiene alguna funcionalidad más allá de lo básico, las anteriores eran simples pruebas didácticas.

La función de esta pequeña aplicación es la de gestionar el intercambio de datos entre actividades y además ser capaz de llamar a otra aplicación, en nuestro caso **Google Maps**.

Si os hace especial ilusión verla al completo la tenéis disponible en mi repositorio de GitHub, [aquí](https://github.com/owniz/AndroidPractise1 "GitHub") en concreto.

<br>

<img class="differentSize65" src="/assets/img/practica1pmm/android.png" alt="android" style="margin:auto; display:block;">

# Conceptos a entender antes de nada

Primero de todo vamos a dar un pequeño repaso a algún que otro concepto para entender mejor de que trata esta aplicación.

## *Activities*

Ya hemos nombrado a las actividades, pero qué es realmente una actividad (o como se les conoce en inglés ***Activities***), podemos definir como cada actividad a cada pantalla que tenemos en una aplicación, es decir, si tenemos 5 pantallas, tendremos 5 *activities*. Esta definición con un conocimiento más avanzado de **Android** no es realmente correcta, pero por ahora podemos trabajar con ella así, por lo tanto cada pantalla que trataremos será una *activity*.

## *Intent* y *Bundle*

***Intent*** es una clase que utilizamos como mecanismo para podernos comunicar entre ***activities***, ya sea invocándola, mandándole servicios o datos.

***Bundle*** como bien indica su nombre es un contenedor de datos muy utilizado junto con ***Intent*** a la hora de añadirlos a éste.

## *startActivity() y startActivityForResult()*

Tenemos dos métodos similares a la hora de llamar a otra ***activity***, con *startActivity()* la llamamos, mientras que con *startActivityForResult()* además de llamarla la preparamos para que sea capaz de devolver datos a la *activity* anterior.

## Constantes

Las constantes son variables que nunca cambian su valor, podemos y debemos utilizarlas para trabajar entre distintas actividades para poder comprobar que el envío y recibo de datos mediante pares clave/valor se haga correctamente, en nuestro caso hemos definido las constantes de abajo en la clase principal.

```java
// constantes definidas para usar como claves
public static final String KEY_NOMBRE = "nombre";
public static final String KEY_EDAD = "edad";
public static final String KEY_TELEFONO = "telefono";
public static final String KEY_LATITUD = "latitud";
public static final String KEY_LONGITUD = "longitud";

// constante para el requestCode
public static final int REQUEST_CODE = 1234;
```

# Explicación de la práctica.

<img class="differentSize40" src="/assets/img/practica1pmm/main.png" alt="mainactivity" style="margin:auto; display:block;">

<div id="ancla" />Como podemos apreciar en la foto de arriba tenemos unas etiquetas sin datos (*TextView* en **Android**) ya que de momento no hemos introducido ninguno de ellos, un botón para ver en *Google Maps* y otro para editar que nos llevara a la siguiente *activity* para introducir los datos. El botón de "ver en *Google Maps* esta desactivado, ya veremos más adelante el por qué.</div>

<img class="differentSize40" src="/assets/img/practica1pmm/second.png" alt="secondctivity" style="margin:auto; display:block;">

En esta segunda *activity* ya podemos introducir los valores, para ellos tendremos dos *EditText* (campo de texto editable) para nombre y teléfono y una barra conocida como *SeekBar* (como curiosidad la barra no enviará ningún dato hasta que se haya movido al menos una vez).

Justo debajo tenemos un *ToggleButton* (Obtener Coordenadas) que cómo bien indica su nombre sirve para poder obtener las coordenadas desde el GPS y por último un botón de aceptar que nos devolverá a la *activity* principal enviándole los datos introducidos.

# Cómo podemos hacer esto.

<img class="differentSize40" src="/assets/img/practica1pmm/demo.gif" alt="demo" style="margin:auto; display:block;">

Una vez tenemos claro el funcionamiento de la práctica podemos ya ponernos a escribir código. Ahora os iré poniendo secciones de éste explicando su cometido para no extender innecesariamente el *post*.

## MainActivity
Esta clase es la que que contiene la actividad principal tenemos 3 métodos en ella:

* ***onCreate()***: Las *activities* en *Android* se pintan, por lo general, desde este método, por lo que a la hora de mostrar la primera actividad hemos de inicializar los componentes que aparecerán en ella.

  ```java
  @Override
  protected void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      setContentView(R.layout.activity_main);

      // inicializamos los TextView que aparecerán en pantalla
      tvNombre = (TextView) findViewById(R.id.textViewNombreDevuelto);
      tvEdad = (TextView) findViewById(R.id.textViewEdadDevuelto);
      tvTelefono = (TextView) findViewById(R.id.textViewTelefonoDevuelto);
      tvLatitud = (TextView) findViewById(R.id.textViewLatitudDevuelto);
      tvLongitud = (TextView) findViewById(R.id.textViewLongitudDevuelto);

      // inicializamos los botones que aparecerán en pantalla
      botonEditar = (Button) findViewById(R.id.buttonEditar);
      botonMaps = (Button) findViewById(R.id.buttonMaps);

      // añadimos los listeners a los botones
      botonEditar.setOnClickListener(this);
      botonMaps.setOnClickListener(this);
  }
  ```

* ***onClick()***: Método encargado de gestionar que ocurre cuando pulsamos los botones a los que hemos añadido el *listener*.

  ```java
  @Override
  public void onClick(View v) {

      // Si pulsamos el botón Editar
      if(R.id.buttonEditar == v.getId()) {

          // declaramos e inicializamos un Intent y un Bundle para enviar los datos
          // a la actividad secundaria
          Intent intent = new Intent(this, SecondActivity.class);
          Bundle b = new Bundle();

          // comprobamos si los campos tienen valor para enviarlos a la segunda actividad,
          // utilizamos clave/valor para que las actividades conozcan a qué dato nos
          // estamos refiriendo
          if(!tvNombre.getText().toString().isEmpty())
              b.putString(KEY_NOMBRE, tvNombre.getText().toString());
          if(!tvEdad.getText().toString().isEmpty())
              b.putInt(KEY_EDAD, Integer.valueOf(tvEdad.getText().toString()));
          if(!tvTelefono.getText().toString().isEmpty())
              b.putInt(KEY_TELEFONO, Integer.valueOf(tvTelefono.getText().toString()));

          // insertamos el contenedor (Bundle) que contiene los datos al intent
          intent.putExtras(b);

          // llamamos a la segunda actividad
          startActivityForResult(intent, REQUEST_CODE);
      }

      // Si pulsamos el botón para acceder a Google Maps
      if(R.id.buttonMaps == v.getId()) {

          // definimos un Uri que pueda recibir Google Maps
          String uri = String.format(Locale.ENGLISH, "geo:%f,%f", latitud, longitud);
          Intent intentMaps = new Intent(Intent.ACTION_VIEW, Uri.parse(uri));

          // hacemos la llamada
          startActivity(intentMaps);
      }
  }
  ```

* ***onActivityResult()***: Este método es el encargado de recibir los datos desde la actividad secundaria. Aquí esta explicado por qué esta desactivado el botón de **Google Maps** que comente [más arriba](#ancla).

  ```java
  @Override
  protected void onActivityResult(int requestCode, int resultCode, Intent data) {
      super.onActivityResult(requestCode, resultCode, data);

      // comprobamos si hemos recibido los datos correctamente
      if(requestCode == REQUEST_CODE && resultCode == RESULT_OK) {

          // para cada clave comprobamos que el intent contiene ese extra para mostrarlo
          if(data.hasExtra(KEY_NOMBRE))
              tvNombre.setText(data.getExtras().getString(KEY_NOMBRE));
          else
              // si el usuario borra el dato en la segunda actividad mostramos
              // un String vacío
              tvNombre.setText("");
          if(data.hasExtra(KEY_EDAD))
              tvEdad.setText(String.valueOf(data.getIntExtra(KEY_EDAD, -1)));
          else
              tvEdad.setText("");
          if(data.hasExtra(KEY_TELEFONO))
              tvTelefono.setText(String.valueOf(data.getIntExtra(KEY_TELEFONO, -1)));
          else
              tvTelefono.setText("");
          if(data.hasExtra(KEY_LATITUD)) {
              tvLatitud.setText(String.valueOf(data.getDoubleExtra(KEY_LATITUD, -1d)));
              latitud = data.getDoubleExtra(KEY_LATITUD, -1d);
          } else {
              tvLatitud.setText("");
          }
          if(data.hasExtra(KEY_LONGITUD)) {
              tvLongitud.setText(String.valueOf(data.getDoubleExtra(KEY_LONGITUD, -1d)));
              longitud = data.getDoubleExtra(KEY_LONGITUD, -1d);
          } else {
              tvLongitud.setText("");
          }

          // hacemos Enabled el botón si recibimos una latitud y una longitud
          botonMaps.setEnabled(data.hasExtra(KEY_LATITUD) && data.hasExtra(KEY_LONGITUD));

          // si el usuario aprieta el botón back devolveremos un Toast avisándole
      } else if(requestCode == REQUEST_CODE && resultCode == RESULT_CANCELED) {
          Toast.makeText(this, R.string.usuarioBack, Toast.LENGTH_SHORT).show();
      }
  }
  ```

## SecondActivity

Clase que contiene la segunda actividad que nos permite introducir los datos que mostraremos en la primera actividad, en ella utilizaremos estos métodos:

* ***onCreate()***: Definimos que aparecerá en pantalla, además controlamos si ya hemos introducido algún valor anteriormente para que lo muestre.

  ```java
  @Override
  protected void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      setContentView(R.layout.activity_second);

      // inicializamos la tabla que se mostrará cuando el ToggleButton este enabled
      tablaCoordenadas = (TableLayout) findViewById(R.id.tablaCoordenadas);

      // inicialiamos los EditText y TextView que aparecerán en pantalla
      etNombre = (EditText) findViewById(R.id.editTextNombre);
      tvEdad = (TextView) findViewById(R.id.textViewMostrarEdad);
      etTelefono = (EditText) findViewById(R.id.editTextTelefono);
      tvLatitud = (TextView) findViewById(R.id.textViewMostrarLatitud);
      tvlongitud = (TextView) findViewById(R.id.textViewMostrarLongitud);

      // inicializamos la variable que accede al servicio de localización
      lm = (LocationManager) getSystemService(Context.LOCATION_SERVICE);

      // inicializamos la SeekBar
      seekBarAnyos = (SeekBar) findViewById(R.id.seekBar);

      // inicializamos los botones que aparecerán en pantalla
      botonAceptar = (Button) findViewById(R.id.buttonAceptar);
      botonToggle = (ToggleButton) findViewById(R.id.toggleButtonCoordenadas);

      // añadimos los listeners a los botones y la barra
      botonToggle.setOnCheckedChangeListener(this);
      botonAceptar.setOnClickListener(this);
      seekBarAnyos.setOnSeekBarChangeListener(this);

      // solicitamos que nos notifique cuando hay cambios en la localización
      lm.requestLocationUpdates(LocationManager.GPS_PROVIDER, 0, 0, this);

      // si anteriormente ya hemos mandado un nombre, edad o teléfono a la actividad
      // principal y volvemos a la secundaria para volverlos a editar, éstos aparecerán
      // ya escritos, los recuperamos con getIntent() y los mostramos con setText()
      etNombre.setText(getIntent().getExtras().getString(MainActivity.KEY_NOMBRE));

      // colocamos el cursor a la derecha de la palabra
      etNombre.setSelection(etNombre.getText().length());

      // comprobamos que la edad tenga un valor válido para mostrarlo
      if(getIntent().getExtras().getInt(MainActivity.KEY_EDAD) >= 18) {
          seekBarAnyos.setProgress(getIntent().getExtras()
                                          .getInt(MainActivity.KEY_EDAD) - 18);
          tvEdad.setText(getString(R.string.anyos, String.valueOf(getIntent()
                                          .getExtras().getInt(MainActivity.KEY_EDAD))));
      }

      // comprobamos que el teléfono tenga un valor para mostrarlo (si no hacemos esta
      // comprobación y accedemos a la actividad secundaria sin tener un teléfono en la
      // principal escribe un 0 ya que el int no tiene definido un valor todavía)
      if(getIntent().hasExtra(MainActivity.KEY_TELEFONO)) {
          etTelefono.setText(String.valueOf(getIntent().getExtras()
                                          .getInt(MainActivity.KEY_TELEFONO)));

          // colocamos el cursor a la derecha de los números
          etTelefono.setSelection(etTelefono.getText().length());
      }
  }
  ```

* ***onClick()***: Igual que en la clase principal es el método encargado de gestionar que ocurre cuando pulsamos los botones.

  ```java
  @Override
   public void onClick(View v) {

       // declaramos e inicializamos el intent que recogerá y mandará los datos
       // a la actividad principal
       Intent intent = new Intent();

       // comprobamos que el botón pulsado es el de aceptar para todos los campos
       if(R.id.buttonAceptar == v.getId()) {

           // comprobamos si está vacío el campo para enviar datos sólo si hemos
           // introducido algún valor para cada uno de los campos
           if(!etNombre.getText().toString().isEmpty())
               intent.putExtra(MainActivity.KEY_NOMBRE, etNombre.getText().toString());
           if(!tvEdad.getText().toString().isEmpty())
               intent.putExtra(MainActivity.KEY_EDAD, seekBarAnyos.getProgress() + 18);
           if(!etTelefono.getText().toString().isEmpty())
               intent.putExtra(MainActivity.KEY_TELEFONO, Integer.valueOf(etTelefono
                                                              .getText().toString()));
       }

       // comprobamos que el ToggleButton esté activo para poder enviar la latitud
       // y la longitud
       if(botonToggle.isChecked()) {

           // comprobamos que el valor sea distinto de 0 para enviar los datos (si no
           // se realiza esta comprobación se envían los valores 0 y 0 si el GPS
           // esta desactivado)
           if(latitud != 0)
               intent.putExtra(MainActivity.KEY_LATITUD, latitud);
           if(longitud != 0)
               intent.putExtra(MainActivity.KEY_LONGITUD, longitud);
         }

       // define el resultado que será recuperado por la actividad llamante que lo espera
       setResult(RESULT_OK, intent);

       // termina la actividad
       finish();
   }
```

* ***onCheckedChanged()***: Método que debemos sobrescribir cuando implementamos los *listeners* de un ToggleButton (*CompoundButton.OnCheckedChangeListener*), en este caso su función es hacer visible la tabla (TableLayout) que muestras las coordenadas cuando está activo.

  ```java
  @Override
  public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
      tablaCoordenadas.setVisibility(isChecked ? View.VISIBLE : View.INVISIBLE);
  }
  ```

* ***onLocationChanged()***: Este método es necesario utilizarlo cuando implementamos *LocationListener* para trabajar con el GPS, en nuestro caso nos guardará la latitud y la longitud en unas variables (para poderlas enviar luego a la actividad principal) y las pintará en pantalla cada vez que se actualicen.

  ```java
  @Override
  public void onLocationChanged(Location location) {
      latitud = location.getLatitude();
      longitud = location.getLongitude();

      tvLatitud.setText(String.valueOf(latitud));
      tvlongitud.setText(String.valueOf(longitud));
  }
  ```

* ***onProgressChanged()***: Al implementar la barra (SeekBar.OnSeekBarChangeListener) podemos sobrescribir este método, en nuestro caso nos mostrará la edad por pantalla en un *TextView* cada vez que movamos la barra.

  ```java
  @Override
   public void onProgressChanged(SeekBar seekBar, int progress, boolean fromUser) {
       tvEdad.setText(getString(R.string.anyos, String.valueOf(progress + 18)));
   }
  ```

Bueno, con esto ya hemos dado un repaso a las dos clases que componen la actividad principal y secundaria de nuestra pequeña aplicación, tan sólo me queda comentaros que para poder utilizar el GPS debemos definir unos permisos que varían según la versión de Android que queramos utilizar, por lo que no voy a poner cuales he utilizado yo, ya que podría llevaros a algún problema, esto debéis mirarlo por vosotros o preguntarme.

Como siempre cualquier duda o corrección no dudéis escribir a mi correo [iam@jmoral.es](mailto:iam@jmoral.es "iam@jmoral.es") o a mi Twitter [@owniz](https://twitter.com/owniz "Twitter").

Un saludo. ツ
