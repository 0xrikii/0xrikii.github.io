---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
---

<div class="pagination-wrapper">
        <div class="pagination-container">
    {% for category in site.categories %}
            <a class="pagination-btn" href="#{{ category[0] }}">{{ category[0] }}</a>
    {% endfor %}
        </div>
</div>

{% assign categories = site.categories | sort %}
{% for category in categories %}
<h4 class="text-2xl font-bold my-5">Kategori: {{ category[0] }}</h4>
<h2>{{ category[0] }} ({{ category | last | size }})</h2>
<div class="blog-grid" id="{{ category[0] }}">
    {% assign sorted_posts = site.posts | sort: 'title' %}
    {% for post in sorted_posts %}
        {%if post.categories contains category[0]%}
        <article class="blog-card fade-in-up" onclick="window.location.href='{{ post.url }}'">
            <span class="card-category">{{ category[0 }}</span>
            <h2 class="card-title">{{ post.title }}</h2>
            <p class="card-description">
                Penjelasan detail tentang vulnerability IDOR yang ditemukan pada sistem manajemen karyawan UNAMA, 
                termasuk analisis dampak dan rekomendasi perbaikan keamanan.
            </p>
            <a href="#" class="read-more-btn">
                Read More
                <span class="btn-icon">→</span>
            </a>
        </article>
    {%endif%}
    {% endfor %}
    </div>
{% endfor %}
