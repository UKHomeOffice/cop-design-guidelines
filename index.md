# Building Forms and BPMNs

These guidelines are intended to help you build forms as quickly and easily as possible whilst also following best practice. They should provide you with a step-by-step approach to building clear, effective forms that your user finds easy to use and that also work well from a technical perspective because they are not overloaded with unnecessary code.


## Principles

{% assign principles = site.pages
  | where: "principle", true   
  | group_by: "category" %}

{% for principle_group in principles %}
{% if principle_group.name != "" %}
### {{ principle_group.name }}
{% else %}
### General principles
{% endif %}

{% for principle in principle_group.items %}
- [{{ principle.title }}]({{ principle.url | relative_url }})
{% endfor %}
{% endfor %}

## Standards

{% assign standards = site.pages
  | where: "standard", true
  | group_by: "category" %}

{% for standard_group in standards %}
{% if standard_group.name != "" %}
### {{ standard_group.name }}
{% else %}
### General standards
{% endif %}

{% for standard in standard_group.items %}
- [{{ standard.title }}]({{ standard.url | relative_url }})
{% endfor %}
{% endfor %}

## Guides

{% assign guides = site.pages
  | where: "guide", true
  | group_by: "category" %}

{% for guide_group in guides %}
{% if guide_group.name != "" %}
### {{ guide_group.name }}
{% else %}
### General guides
{% endif %}

{% assign sorted_guide_groups_items =  guide_group.items | sort: 'order' %}
{% for guide in sorted_guide_groups_items %}
- [{{ guide.title }}]({{ guide.url | relative_url }})
{% endfor %}
{% endfor %}
