---
layout: post
title: Hola mundo!
permalink: "blog/hola-mundo"
---

# Primer post con chuleta para markdown con Jekyll y algún estilo personalizado (css) :P

<br>

`highlight` <- para utilziarlo escribir ` ` con el texto entre medias

texto normal :D <- `texto normal`

**negrita** <- `**negrita**`

*cursiva* <- `*cursiva*`

***ambas*** <- `***ambas***`

**_ambas-2_** <- `**_ambas-2_**`

~~tachado~~ <- `~~tachado~~`

[link](http://jmoral.es) <- `[link](https://jmoral.es)`

<hr class="codebreak">

# título h1 <- `# título h1`

## título h2 <- `## título h2`

<hr class="codebreak">

Párrafo 1

<br>

Párrafo 2 - Para tener más de un salto de línea usar `<br>`

<hr class="codebreak">

listas con `*`

* uno <- `* uno`
* dos <- `* dos`

<hr class="codebreak">

[ancla](#test) <- `[ancla](#test)`

<h1 id="test">test ancla</h1> <- `<h1 id="test">test ancla</h1>`

<hr class="codebreak">

	public class HelloWorld {
		public static void main(String[] args) {
			System.out.print("Sólo con tabular para código :D");
		}
	}

```
public class HelloWorld {
   public static void main(String[] args) {
      System.out.print("o podemos poner ``` antes y después del código");
   }
}
```

```python
public class HelloWorld {
   public static void main(String[] args) {
      System.out.print("también podemos poner ```java antes y después ``` para que lo pinte con el tema Monokai en java, puedes usar otro lenguajes como python, ruby, css, etc y además tiene scrollbar!");
   }
}
```

<hr class="codebreak">

Con `<hr class="codebreak">` ponemos esta línea discontinua

<hr class="codebreak">

para poner una imagen usamos `![título](/assets/img/kodama.jpg)`

![texto alternativo](/assets/img/kodama.jpg)

o podemos poner `<img src="/assets/img/kodama.jpg" alt="kodama" style="width:50%; margin:auto; display:block;">` para especificar tamaño y centrar la imagen

<img src="/assets/img/kodama.jpg" alt="kodama" style="width:50%; margin:auto; display:block;">

<hr class="codebreak">

para tablas podemos usar

	| Título 1     | Título 2     | Titulo 3  |
	| ------------ |:------------:| ---------:| 
	| izquierda    | centrado     | derecha   |
	| uno          | dos          |   tres    |


| Título 1     | Título 2     | Titulo 3  |
| ------------ |:------------:| ---------:| 
| izquierda    | centrado     | derecha   |
| uno          | dos          |   tres    |
| uno          | dos          |   tres    |
| uno          | dos          |   tres    |

<hr class="codebreak">

# Prueba de texto largo separado por párrafos y con blockquotes para usarlos poner `> ` &nbsp;delante del párrafo

Lorem ipsum dolor sit amet, suas magna dicta qui ei, no vix graecis iracundia, mei eruditi perfecto ut. Placerat necessitatibus nam ne, ne mediocrem abhorreant has. Nec primis legimus menandri at, ad autem apeirian similique vim, impetus reformidans eos et. Zril officiis erroribus ut eum, ex duo vero prima omittantur, id cum mundi facilisi indoctum. Omnis eripuit liberavisse his no, vis ei sumo dico utinam, enim ornatus consequat his eu.

> Est ut graece commune, quo an exerci ceteros aliquando. Sea ad odio vidit. Autem graeci maiorum an sea, veri contentiones eum ad. An blandit forensibus sed, accusata elaboraret vim id, eam inani timeam volumus ei. Est ne probo vituperata, an has vide malorum senserit.

Ad pro invenire evertitur, nisl partiendo qui ei. Te nam dolorum dissentias, te vis consetetur dissentiet delicatissimi. Sea consul dictas ut, usu dolore accumsan quaestio ex. Pro ei evertitur interpretaris concludaturque. His epicurei adipiscing in, ex sit natum prima legendos. Est convenire interpretaris ne, ei eam docendi mediocritatem, vix no elit eripuit.

> Cu vix graeco platonem, per id duis ferri mollis, an delenit evertitur ius. Pri in habeo mollis. Pro ea purto soleat, congue veritus te mea. Dicit mnesarchum te quo, eius animal eripuit ea nam.

Augue libris luptatum ei vix, eum eu commodo vidisse similique. Eu pertinax corrumpit vel, atqui error dicant mei at, in qui alii paulo facilis. Vim doming eleifend te, interesset liberavisse id vel. No vix aliquam bonorum efficiendi, nam an modus debet scribentur, vim timeam scribentur consectetuer at. Cu ius suas mundi errem, has in consul honestatis. Mucius sanctus per ei, ad mel populo laoreet scaevola, id posse iisque elaboraret eam.

<hr class="codebreak">

[spoiler]

```java
Podemos usar [spoiler] y [/spoiler] para ocultar contenido
```

[/spoiler]
