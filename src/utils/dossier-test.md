---
title: Dossier test
hidden: true
---


{% assign thisDestination = 'keen' %}

{% assign destinationInfo = site.data.catalog.destinations.items | where: "slug", thisDestination | first %}
{% comment %}There are probably prettier ways to generate a list of links to these methods, but this was good enough for me.{% endcomment %}
{% assign destMethods = "" | split: ", " %}
{% assign methodName = "" | split: " " %}
{% for method in destinationInfo.methods %}
{%if method[1]%}
{% assign methodName = method[0] | split: "_" | first | capitalize%}
{% assign methodUrl = '<a href="/docs/connections/spec/' | append: methodName | downcase%}
{% assign methodUrl = methodUrl | append: '/">' %}
{% assign methodHTML = methodUrl | append: methodName %}
{% assign methodHTML = methodHTML | append: '</a>' %}
{% assign destMethods = destMethods | push: methodHTML %} {%endif%}
{% endfor %}
{% assign destMethods = destMethods| reverse %}

<div class="quick-info">
<h4>{{ destinationInfo.display_name }} quick info </h4>
<p>{% if destMethods.size > 0 %}
<strong>{{destinationInfo.display_name}}</strong> {% if destinationInfo.status == "PUBLIC_BETA" %} is in beta, and{% endif %} accepts {% for method in destMethods%}{% if destMethods.size == 1 %}{{method}} calls.{% else %}{% unless forloop.last == true %}{{method}}, {% endunless %}{% if forloop.last == true%}and {{method}} calls.{%endif%}{% endif %}{% endfor %} If you reference it in the <a href="/docs/guides/filtering-data/#filtering-with-the-integrations-object">Integrations object</a>, call it &ldquo;{{destinationInfo.previous_names | join: '", or "' }}&rdquo;.

{% elsif destinationInfo.status == "PUBLIC_BETA" and  destMethods.size == 0 %}
<strong>{{ destinationInfo.display_name }}</strong> is in beta. If you reference it in the <a href="/docs/guides/filtering-data/#filtering-with-the-integrations-object">Integrations object</a>, call it &ldquo;{{destinationInfo.previous_names | join: '", or "' }}&rdquo;.

{% elsif destinationInfo.status == "PUBLIC" and  destMethods.size == 0 %}
If you reference <strong>{{destinationInfo.display_name}}</strong> in the <a href="/docs/guides/filtering-data/#filtering-with-the-integrations-object">Integrations object</a>, call it &ldquo;{{destinationInfo.previous_names | join: '", or "' }}&rdquo;.
{% endif %}

{% if destinationInfo.replay %} {{ destinationInfo.display_name }} is eligible for <a href="/docs/guides/what-is-replay/">Replay</a>. {% endif %}</p>

{% unless destinationInfo.connection_modes.case == "0"%}
{% if page.cmode-override %}
{% assign destinationInfo = site.data.catalog.overrides.items % | where: "slug", thisDestination | first %}
{% endif %}
<p>
{{ destinationInfo.display_name }} {{ destinationInfo.connection_modes.summary }} {% if destinationInfo.connection_modes.case == "mixed" %}  {{destinationInfo.display_name}} accepts device-mode data from {% if destinationInfo.connection_modes.device.mobile and destinationInfo.connection_modes.device.web %} mobile and web{% elsif destinationInfo.connection_modes.device.mobile %} mobile{% elsif destinationInfo.connection_modes.device.web %} web{% endif %} sources.{% endif %} {% if destinationInfo.browserUnbundlingSupported %} You can change the {{destinationInfo.display_name}} connection mode for Analytics.js sources from the destination settings in the Segment web app.
{% endif %}
</p>
{% endunless %}
</div>

{%if destinationInfo.replay %} YES IT REPLAYS {%endif%}


### Raw dump here

{{destinationInfo}}

<hr>