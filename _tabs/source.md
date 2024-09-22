---
layout: page
title: source
icon: fas fa-code
order: 6
---
# content
{% assign main_categories = "Tutorial" | split: "," %}
{% assign max_depth = 3 %}

{% for main_category in main_categories %}

  {% comment %} 获取包含主分类的所有文章，并按日期倒序排序 {% endcomment %}
  {% assign posts_in_main_category = site.posts | where_exp: "post", "post.categories contains main_category" | sort: 'date' | reverse %}
## {{ main_category }}

  {% comment %} 收集唯一的子分类路径 {% endcomment %}
  {% assign subcategory_list = "" | split: "" %}
  {% for post in posts_in_main_category %}
    {% for depth in (1..max_depth) %}
      {% if post.categories[depth] %}
        {% assign subcategory_path = post.categories | slice: 1, depth | join: "/" %}
        {% unless subcategory_list contains subcategory_path %}
          {% assign subcategory_list = subcategory_list | push: subcategory_path %}
        {% endunless %}
      {% endif %}
    {% endfor %}
  {% endfor %}
  {% assign sorted_subcategories = subcategory_list | sort %}
  {% for subcategory in sorted_subcategories %}
### {{ subcategory | split: "/" | last }}
    {% assign subcategory_depth = subcategory | split: "/" | size %}
    {% for post in posts_in_main_category %}
      {% assign post_subcategory_path_array = post.categories | slice: 1, subcategory_depth %}
      {% assign post_subcategory_path = post_subcategory_path_array | join: "/" %}
      {% if post_subcategory_path == subcategory %}
- [{{ post.title }}]({{ post.url }})  {{ post.author }} ({{ post.date | date: "%Y-%m-%d" }})
      {% endif %}
    {% endfor %}
  {% endfor %}
{% endfor %}
