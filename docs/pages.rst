##########################
Pages
##########################

create a new page

::

    $ rake new_page['page_name']
    $ cd source/_includes/custom/navigation.html   # update the navigation

*************
Page Asides
*************

_config.yml

::

    # Each layout uses the default asides, but they can have their own asides instead. Simply uncomme    nt the lines below
    # and add an array with the asides you want to use.
    # blog_index_asides:
    # post_asides:
    # page_asides:
    tech_asides: [asides/recent_posts_tech.html]


source/_layouts/_page_name.html

::

     {% unless page.sidebar == false %}
     <aside class="sidebar">
        {% include_array tech_asides %}
     </aside>
     {% endunless %}

source/_includes/asides/recent_posts_tech.html

::

    <section>
      <h1>Recent Posts</h1>
      <ul id="recent_posts">
        {% for post in site.categories.tech limit: site.recent_posts %}
          <li class="post">
            <a href="{{ root_url }}{{ post.url }}">{% if site.titlecase %}{{ post.title | titlecase }}{% else %}{{ post.title }}{% endif %}</a>
          </li>
        {% endfor %}
      </ul>
    </section>


