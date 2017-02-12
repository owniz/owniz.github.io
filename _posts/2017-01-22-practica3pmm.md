---
layout: post
title: Tareas asíncronas en Android
category: [PMM, android]
permalink: "blog/asynctask-android"
published: yes
---

<br>

Hola a todos, hace ya 2 meses desde mi [última práctica](/blog/segunda-app-android) en Android y casi 3 desde la [primera](/blog/primera-app-android) por lo que recomiendo que les deis un vistazo para recordar conceptos olvidados. Hoy veremos como trabajar en segundo plano gracias a la clase **AsyncTask**.

Cómo siempre podéis dar un vistazo al código completo en [su repositorio en GitHub](https://github.com/owniz/AndroidPractise4 "GitHub") o bajaros el *apk* desde [aquí](https://github.com/owniz/AndroidPractise4/releases/download/1.0/Random_Buttons.apk).

<br>

<img class="differentSize65" src="/assets/img/practica2pmm/android.png" alt="android" style="margin:auto; display:block;">

# Conceptos a entender

Repasito rápido de conceptos antes de entrar en materia.

## **AsyncTask**

Cuando nuestra aplicación debe realizar tareas que duren mucho tiempo o una cantidad de tiempo indeterminado, para que el usuario no note lentitud en la aplicación o sienta que se ha congelado (ni Android tampoco lo crea) podemos trabajar con procesos que se ejecutaran en segundo plano. Una forma de trabajar con ellos es a través de la clase **AsyncTask**.

## Cuadros de diálogo

La definición que nos proporciona la web de [Android Developers](https://developer.android.com/guide/topics/ui/dialogs.html?hl=es "Android Developers") es de una pequeña ventana que no ocupa toda la pantalla en la que podemos avisar al usuario de algún evento o pedirle alguna acción para poder continuar.

# Explicación de la práctica y código utilizado

<img class="differentSize50" src="/assets/img/practica4pmm/instrucciones-animacion.gif" alt="main" style="margin:auto; display:block;">

En este *GIF* podemos apreciar una pequeña animación para el título del juego y un diálogo (en este caso en concreto un **AlertDialog**), para la animación es necesario que creemos un archivo *XML* en el que escribiremos su comportamiento; el que he utilizado yo lo podéis ver en el siguiente código:

```xml
<?xml version="1.0" encoding="utf-8"?>
<set android:shareInterpolator="false"
    xmlns:android="http://schemas.android.com/apk/res/android">

    <!-- transparencia -->
    <alpha
        android:interpolator="@android:anim/accelerate_interpolator"
        android:fromAlpha="0.0"
        android:toAlpha="1.0"
        android:duration="2500" />

    <!-- cambio de escala -->
    <scale
        android:interpolator="@android:anim/accelerate_decelerate_interpolator"
        android:fromXScale="0.8"
        android:toXScale="1"
        android:fromYScale="0.8"
        android:toYScale="1"
        android:pivotX="50%"
        android:pivotY="50%"
        android:fillAfter="false"
        android:duration="3000"
        android:repeatMode="reverse"
        android:repeatCount="infinite" />

</set>
```

Para asignarlo a la imagen debemos utilizar el siguiente código en el método *onCreate()*:

```java
Animation animation = AnimationUtils.loadAnimation(this, R.anim.animacion_titulo);
findViewById(R.id.imageViewTitulo).startAnimation(animation);
```

<img class="differentSize50" src="/assets/img/practica4pmm/dialog-jugar.gif" alt="jugar" style="margin:auto; display:block;">

Si pulsamos sobre **JUGAR** nos aparecerá un nuevo **AlertDialog** en el que podremos introducir un nombre y deberemos elegir el nivel de dificultad. Este diálogo está personalizado por lo que primero debemos crear un *layout* para éste, podéis verlo [aquí](https://github.com/owniz/AndroidPractise4/blob/master/app/src/main/res/layout/dialogo_nombre_nivel.xml "GitHub"). A la hora de desarrollarlo tendrá su propia clase, que una vez más podéis ver [aquí](https://github.com/owniz/AndroidPractise4/blob/master/app/src/main/java/es/jmoral/dam2/practicaEvaluable4/dialogs/DialogoNombreNivel.java "GitHub"), en la que asignaremos qué tiene que ocurrir cuando se pulse cada botón, **VOLVER** nos devolverá a la pantalla principal y con **JUGAR** empezaremos la partida.

<img class="differentSize50" src="/assets/img/practica4pmm/partida.gif" alt="partida" style="margin:auto; display:block;">

Una vez hemos elegido jugar tendremos que apretar los botones en el orden correcto antes de que termine el tiempo que podemos apreciar en la *ProgressBar* justo debajo de ellos, para esto hemos trabajado con una tarea en segundo plano, que gestiona cada fase, que es una instancia de una clase anidada que extiende de **AsyncTask** situada dentro de la clase con la actividad que muestra la partida. Como nota indicaros que lo único que se ejecuta en segundo plano es el método *doInBackground()* que es el encargado de llamar al método *onProgressUpdate()* para que vaya actualizando tanto la barra como el texto con el progreso, os dejo el código de ésta justo debajo:

```java
// clase que gestiona el progreso del juego
  private class ControlProgesoTask extends AsyncTask<Integer, Integer, Integer> {

      // devuelve el progreso de la barra
      @Override
      protected Integer doInBackground(Integer... integers) {
          while(progreso <= 100) {
              SystemClock.sleep(integers[0]);
              controlProgesoTask.publishProgress(progreso);
              progreso++;

              // si se cancela sale del bucle
              if(isCancelled())
                  break;
          }

          return progreso;
      }

      // actualiza el progreso de la barra y lo muestra en el TextView
      @Override
      protected void onProgressUpdate(Integer... values) {
          super.onProgressUpdate(values);
          progressBar.setProgress(values[0]);
          tvProgreso.setText(values[0] + getString(R.string.slash) + progressBar.getMax());
      }

      // cuando la barra de progreso llega a 100 termina el juego por lo que muestra un
      // dialogo indicando hasta que fase has llegado
      @Override
      protected void onPostExecute(Integer integer) {
          super.onPostExecute(integer);

          // termina partida
          partidaAcabada = true;

          // dialogo con la información
          new AlertDialog.Builder(SecondActivity.this)
                  .setTitle(R.string.fin)

                  // si anteriormente no ha elegido un nombre no lo muestra en el dialogo
                  .setMessage(getString(R.string.enhorabuena,
                          tvNombre.getText().toString().equals(getString(R.string.random_buttons)) ?
                          getString(R.string.empty) : getString(R.string.space) + tvNombre.getText()) + numFase)
                  .setPositiveButton(R.string.continuar, new DialogInterface.OnClickListener() {

                      // si pulsa CONTINUAR JUGANDO volvemos al dialogo con
                      // las opciones de partida
                      @Override
                      public void onClick(DialogInterface dialogInterface, int i) {
                          tvNombre.setText(getString(R.string.random_buttons));
                          setDialogo();
                          numerarBotones(desordenarArray());
                      }
                  })
                  .setNegativeButton(R.string.inicio, new DialogInterface.OnClickListener() {

                      // si pulsa VOLVER regresamos a la pantalla principal
                      @Override
                      public void onClick(DialogInterface dialogInterface, int i) {
                          finish();
                      }
                  })
                  .setCancelable(false)
                  .show();
      }

      // si pulsamos los 4 botones correctamente termina la tarea e inicia una nueva con
      // mayor velocidad
      @Override
      protected void onCancelled(Integer integer) {
          super.onCancelled(integer);

          // comprobamos que haya pulsado los 4 botones correctamente para volver a colocar
          // todas las variables y opciones del juego
          if(contadorBotones == 5) {
              controlProgesoTask = new ControlProgesoTask();

              numerarBotones(desordenarArray());
              tvFase.setText(getString(R.string.fase, String.valueOf(++numFase)));
              contadorBotones = 1;
              progressBar.setProgress(0);
              progreso = 0;

              // nos aseguramos que nunca sea un número negativo el tiempo de
              // espera del proceso y vamos restando el tiempo en 5 milisegundos
              velocidad = velocidad - 5 <= 0 ? 1 : velocidad - 5;

              // ejecutamos una nueva tarea (nueva fase)
              controlProgesoTask.execute(velocidad, progreso);
          }
      }
  }
```

<img class="differentSize50" src="/assets/img/practica4pmm/volver-continuar.gif" alt="fin-partida" style="margin:auto; display:block;">

Una vez la barra llega al final y nos hemos quedado sin tiempo se ejecuta el método *onPostExecute()* que nos mostrará un diálogo para volver al inicio o seguir jugando.

# Modo inmersivo

Por último comentar que para poner las actividades en pantalla completa (conocido por modo inmersivo) tenemos este método que tan sólo debemos llamar cuando sea necesario.

```java
public class BaseActivity extends AppCompatActivity {

    // método para modo inmersivo
    protected void setImmersiveMode() {
        getWindow().getDecorView().setSystemUiVisibility(
                View.SYSTEM_UI_FLAG_LAYOUT_STABLE
                        | View.SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION
                        | View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN
                        | View.SYSTEM_UI_FLAG_HIDE_NAVIGATION
                        | View.SYSTEM_UI_FLAG_FULLSCREEN
        );

        // si es superior a 18 recuperamos la configuración anterior y además
        // añadimos otra opción para esas versiones
        if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.KITKAT)
            getWindow().getDecorView().setSystemUiVisibility(
                    getWindow().getDecorView().getSystemUiVisibility()
                            | View.SYSTEM_UI_FLAG_IMMERSIVE_STICKY
            );
    }
}
```

Si tienes curiosidad por cómo está hecha alguna parte de la aplicación podéis darle un vistazo a [su repositorio en GitHub](https://github.com/owniz/AndroidPractise4 "GitHub") o preguntarme cualquier duda en mi correo [iam@jmoral.es](mailto:iam@jmoral.es "iam@jmoral.es") o en mi Twitter [@owniz](https://twitter.com/owniz "Twitter").

Un saludo. ツ
