---
layout: page
title: Blog
description: "Articles and notes on software development, AI engineering, process automation and the craft of building software."
---

{%- comment -%}
  Tag filter. The filtering is progressive enhancement: without JavaScript
  every post stays visible and the archive is fully usable.
{%- endcomment -%}
<div class="post-filter" data-post-filter>
  <button type="button" class="filter-chip is-active" data-filter="all" aria-pressed="true">
    All <span class="filter-count">{{ site.posts | size }}</span>
  </button>
  {%- comment -%}
    site.tags is a hash, so iterating it yields [name, posts] pairs. The names
    are collected into a delimited string first and sorted as a plain array —
    that avoids relying on a hash being coerced to an array by `sort`.
  {%- endcomment -%}
  {%- capture tag_names_raw -%}
    {%- for tag in site.tags -%}{{ tag[0] }}|{%- endfor -%}
  {%- endcapture -%}
  {%- assign tag_names = tag_names_raw | split: '|' | sort -%}
  {% for name in tag_names %}{% unless name == blank %}
  <button type="button" class="filter-chip" data-filter="{{ name | slugify }}" aria-pressed="false">
    {{ name }} <span class="filter-count">{{ site.tags[name] | size }}</span>
  </button>
  {% endunless %}{% endfor %}
</div>

{%- comment -%}
  Grouped per year. Each year is its own section with its own list, so the
  year heading is never an invalid direct child of a <ul>.
{%- endcomment -%}
{% assign years = site.posts | group_by_exp: "post", "post.date | date: '%Y'" %}
{% for year in years %}
<section class="post-year-group" data-year="{{ year.name }}">
  <h3 class="post-year">{{ year.name }}</h3>
  <ul class="posts">
    {% for post in year.items %}
    <li itemscope data-tags="{% for tag in post.tags %}{{ tag | slugify }} {% endfor %}">
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <p class="post-date">
        <span>
          {% include icon.html name="calendar" %} {{ post.date | date: "%B %-d" }}
          &middot; {% include icon.html name="clock" %} {% include read-time.html %}
        </span>
      </p>
    </li>
    {% endfor %}
  </ul>
</section>
{% endfor %}

<p class="filter-empty" hidden>No articles with that tag yet.</p>

<script>
  (function() {
    var root = document.querySelector('[data-post-filter]');
    if (!root) return;

    var chips = Array.prototype.slice.call(root.querySelectorAll('.filter-chip'));
    var groups = Array.prototype.slice.call(document.querySelectorAll('.post-year-group'));
    var empty = document.querySelector('.filter-empty');

    function apply(filter) {
      var visible = 0;

      groups.forEach(function(group) {
        var shown = 0;
        Array.prototype.slice.call(group.querySelectorAll('.posts > li')).forEach(function(li) {
          var tags = (li.getAttribute('data-tags') || '').split(' ').filter(Boolean);
          var show = filter === 'all' || tags.indexOf(filter) !== -1;
          li.hidden = !show;
          if (show) shown++;
        });
        // A year heading is only meaningful while it still has posts under it.
        group.hidden = shown === 0;
        visible += shown;
      });

      if (empty) empty.hidden = visible !== 0;
    }

    chips.forEach(function(chip) {
      chip.addEventListener('click', function() {
        chips.forEach(function(c) {
          c.classList.remove('is-active');
          c.setAttribute('aria-pressed', 'false');
        });
        chip.classList.add('is-active');
        chip.setAttribute('aria-pressed', 'true');
        apply(chip.getAttribute('data-filter'));
      });
    });
  })();
</script>
