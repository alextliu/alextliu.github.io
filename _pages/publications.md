---
layout: page
permalink: /publications/
title: publications
description: A complete publication list can be found on Tianyi Liu's profiles in <a href="https://scholar.google.com/citations?user=SAJ8bL8AAAAJ&hl=en" target="_blank">Google Scholar</a> and <a href="https://www.researchgate.net/profile/Tianyi-Liu-3" target="_blank">Research Gate</a>.
nav: true
nav_order: 1
---

<!-- _pages/publications.md -->

<!-- Bibsearch Feature -->

{% include bib_search.liquid %}

<div class="publications">

<h1>preprints</h1>

{% bibliography -f misc %}

<h1>Journal Articles</h1>

{% bibliography -f article %}

<!-- {% for y in page.years %} -->
<!--   <h2 class="year">{{y}}</h2> -->
<!--   {% bibliography -f papers -q @*[year={{y}}]* %} -->
<!-- {% endfor %} -->

</div>
