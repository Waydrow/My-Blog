---
title: categories
date: 2016-02-01 20:57:21
---

<div class="category-wrap">
	<% if (site.categories.length){ %>
  		<div class="widget-wrap">
    		<h3 class="widget-title">Categories</h3>
    		<div class="widget">
     			 <%- list_categories({show_count: theme.show_count}) %>
    		</div>
  		</div>
	<% } %>
</div>