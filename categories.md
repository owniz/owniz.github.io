---
layout: default
title: Categorías
permalink: /categorias
---

<script type="text/javascript">
function handle_selection(element) {
    window.location = element.value;
    if (element.value != "#all") {
        $('.catbloc_showing_all').removeClass('catbloc_showing_all').addClass('catbloc');
    } else {
        $('.catbloc').removeClass('catbloc').addClass('catbloc_showing_all');
    }
}
</script>

<nav>
    <select id="category_selector" name="Categories" onchange="handle_selection(this);">
        <option value="#all" selected="selected">Todos los posts</option>
        {% for category in site.categories %}
            <option value="#{{ category | first | remove:' ' }}">{{ category | first }}</option>
        {% endfor %}
    </select>
</nav>

<div id="post">
    {% for category in site.categories %}
	<div class="catbloc_showing_all" id="{{ category | first | remove:' ' }}">
    	<h2 class="category_header">{{ category | first }}</h2>
         
    	<ul class="posts">
        	{% for posts in category %}
            	{% for post in posts %}
                	{% if post.url %}
                    	<li>
                        	<span class="date">
                                {{ post.date | date: '%d de'}}
                                {% assign m = post.date | date: "%-m" %}
                                {% case m %}
                                    {% when '1' %}Ene.
                                    {% when '2' %}Feb.
                                    {% when '3' %}Mar.
                                    {% when '4' %}Abr.
                                    {% when '5' %}May.
                                    {% when '6' %}Jun.
                                    {% when '7' %}Jul.
                                    {% when '8' %}Ago.
                                    {% when '9' %}Sep.
                                    {% when '10' %}Oct.
                                    {% when '11' %}Nov.
                                    {% when '12' %}Dic.
                                {% endcase %}
                                {{ post.date | date: '%Y' }}</span> - <a href="{{ post.url }}">{{ post.title }}</a> 
                    	</li>
                	{% endif %}
            	{% endfor %}
        	{% endfor %}
    	</ul>
	</div>
	{% endfor %}
</div>