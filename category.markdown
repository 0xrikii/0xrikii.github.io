---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---
<style>
    html {
        scroll-behavior: smooth;
    }
</style>
<p class="text-my-gray mb-5">Categories</p>
<div class="">
    {% for category in site.categories %}
        <a href="#{{ category[0] }}" class="category-button-post">{{ category[0] }}</a>
    {% endfor %}
</div>

<div id="index">
    {% assign categories = site.categories | sort %}
    {% for category in categories %}
        <div class="border-[1px] rounded-lg p-5 my-5" id="{{ category[0] }}">
            <h4 class="text-2xl font-bold my-5">Kategori: {{ category[0] }}</h4>
            <h2>{{ category[0] }} ({{ category | last | size }})</h2>
            {% assign sorted_posts = site.posts | sort: 'title' %}
            {% for post in sorted_posts %}
                {%if post.categories contains category[0]%}
                    <div class="">
                        <a class="hover:text-my-black dark:hover:text-my-white text-my-blue dark:text-my-green" href="{{ post.url }}"><h3 class="text-2xl font-bold">{{ post.title }}</h3></a>
                        <a href="{{ post.url }}"><p class="text-lg my-5">{{ post.excerpt }}</p></a>
                        <a href="{{ post.url }}" class="bg-my-blue dark:bg-my-green rounded-lg p-2 my-5 text-my-white dark:text-my-black inline-block font-bold">More >></a>
                    </div>
                {%endif%}
            {% endfor %}
        </div>
    {% endfor %}
</div>
