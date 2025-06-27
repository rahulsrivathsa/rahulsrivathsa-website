---
layout: default
---

# Rahul Srivathsa

## About Me

I grew up in Bangalore, India and moved to the US in 2010. I've worked in tech for 10+ years as a PM at companies like Tripadvisor and iAsk.ai.

I'm based in Chicago and I enjoy cycling, skiing, yoga, live music, films, and possibly mountain biking very soon.

## Projects

I've started building in my spare time as a new hobby. A few recent projects:

{% assign projects = site.projects | sort: 'title' %}
{% for project in projects %}
<div class="project-item">
    <div class="project-title"><a href="{{ project.url }}">{{ project.title }}{% if project.status == "In Progress" %} [in progress]{% endif %}</a></div>
    <div class="project-description">{{ project.description }}</div>
</div>
{% endfor %}

{% assign thoughts = site.thoughts | sort: 'date' | reverse %}
{% if thoughts.size > 0 %}
## Thoughts

I sometimes write about ideas I've explored and things I've learned; a few are listed below:

{% for post in thoughts %}
- [{{ post.title }}]({{ post.url }}) ({{ post.date | date: "%B %Y" }})
{% endfor %}
{% endif %}

## Interests

This is an incomplete list of topics I'm exploring and reading about

- Reading Prediction Machines by Ajay Aggarwal
- Listening to Awareness by Anthony DeMello
- Listening to Philosophize This! by Stephen West (Aristotle pt 2)
- Trying out the Essentialism planner by Greg McKeown
- Watching Andrej Karpathy's intro to LLMs video

## Contact

Email me [here](mailto:rahul.srivathsa@gmail.com).

*Last updated: {{ site.time | date: "%B %Y" }}*