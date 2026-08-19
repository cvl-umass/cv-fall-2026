---
layout: schedule
permalink: /lectures/
title: Schedule
# calendar year the M/D dates in _data/lectures.yml fall in, used to
# render the day of the week. Lives here rather than in _config.yml so
# that `jekyll serve` picks up a change without needing a restart.
year: 2026
---

{% assign prev_date = 0 %}

{% for item in site.data.lectures %}
{% if item.date %}
{% assign lecture = item %}
{% assign event_type = "upcoming" %}
{% comment %}
  Dates in _data/lectures.yml are bare M/D strings; pair them with
  page.year so they can be sorted and formatted as real dates.
{% endcomment %}
{% assign date_parts = lecture.date | split: "/" %}
{% if date_parts.size > 1 %}
{% assign full_date = page.year | append: "-" | append: date_parts[0] | append: "-" | append: date_parts[1] %}
{% assign display_date = full_date | date: "%A, %-m/%-d" %}
{% assign today_date = "now" | date: "%s" | divided_by: 86400 %}
{% assign lecture_date = full_date | date: "%s" | divided_by: 86400 %}
{% if today_date > lecture_date %}
    {% assign event_type = "past" %}
{% elsif today_date <= lecture_date and today_date > prev_date %}
    {% assign event_type = "warning" %}
{% endif %}
{% assign prev_date = lecture_date %}
{% else %}
{% comment %} dates without a M/D value, e.g. "Final-exam period", print as written {% endcomment %}
{% assign display_date = lecture.date %}
{% endif %}

<tr class="{{ event_type }}">
    <th scope="row">{{ display_date }}</th>
    {% if lecture.title contains 'No class' or forloop.last %}
    <td colspan="2" align="center">{{ lecture.title }}</td>
    {% else %}
    <td>
        {{ lecture.title }}
        {% if lecture.logistics %}
        <br />
        {{ lecture.logistics }}
        {% endif %}
    </td>
    <td>
        {% if lecture.readings %}
        <ul>
        {% for reading in lecture.readings %}
            <li>{{ reading }}</li>
        {% endfor %}
        </ul>
        {% endif %}
    </td>
    {% endif %}
</tr>
{% else %}
{% assign module = item %}
<tr class="info">
    <td colspan="3" align="center"><strong>{{ module.title }}</strong></td>
</tr>
{% endif %}
{% endfor %}
