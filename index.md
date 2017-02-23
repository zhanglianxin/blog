---
layout: default
title: Home
---
<h2>Home</h2>
<!-- This loops through the paginated posts -->
{% for post in paginator.posts %}
  <h3><a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></h3>
  <p class="author">
    <span class="date">{{ post.date | date_to_string }}</span>
  </p>
{% endfor %}

<!-- Pagination links -->
<div class="pagination">
  {% if paginator.previous_page %}
    <a href="{{ paginator.previous_page_path }}" class="previous">Previous</a>
  {% else %}
    <span class="previous">Previous</span>
  {% endif %}
  <span class="page_number">({{ paginator.page }} / {{ paginator.total_pages }})</span>
  {% if paginator.next_page %}
    <a href="{{ paginator.next_page_path }}" class="next">Next</a>
  {% else %}
    <span class="next">Next</span>
  {% endif %}
</div>
