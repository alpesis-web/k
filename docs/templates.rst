######################
Templates
######################

***************
tags
***************

/octopress/_includes/post/tags.html

::

    {% capture tag %}{% if post %}{{ post.tags | tag_links | size }}{% else %}{{ page.tags | tag_links | size }}{% endif %}{% endcapture %}
    {% unless tag == '0' %}
    <span class="categories">
      {% if post %}
        {{ post.tags | tag_links }}
      {% else %}
        {{ page.tags | tag_links }}
      {% endif %}
    </span>
    {% endunless %}


added in `post_art.html`, `post_tech.html`, `post_life.html`


`archives_post.html`

::

    {% capture tag %}{{ post.tags | size }}{% endcapture %}  
    {% if tag != '0' %}
        <span class="categories">{{ post.tags | tag_links }}</span>
    {% endif %}


