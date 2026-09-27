---
layout: default
title: District calendar
permalink: /calendar/
description: Every West Orange PTA's events, the district calendar, and community feeds in one place.
---

# West Orange PTA calendars

{%- comment -%}
GitHub Pages builds with Jekyll 3.10, whose where_exp filter cannot take "and"/"or" or a bare
variable, so both lists are built with plain for/if loops (core Liquid only).
{%- endcomment -%}
{%- capture srcs -%}
  {%- for c in site.data.calendars -%}{%- if c.google_id -%}&src={{ c.google_id | url_encode }}&color={{ c.color | url_encode }}{%- endif -%}{%- endfor -%}
{%- endcapture -%}
{%- capture pending_raw -%}
  {%- for c in site.data.calendars -%}{%- if c.google_id == nil and c.ics == nil -%}{{ c.name }}|{%- endif -%}{%- endfor -%}
{%- endcapture -%}
{%- assign pending = pending_raw | split: "|" -%}

One calendar for the whole district: each PTA's events in its own color, plus the school district's calendar and selected community schedules. Use the dropdown in the top-right of the calendar to show or hide individual calendars.

<iframe src="https://calendar.google.com/calendar/embed?height=600&wkst=1&ctz=America%2FNew_York&showPrint=0&showCalendars=1&showTz=0{{ srcs }}" style="border:solid 1px #777" width="800" height="600" frameborder="0" scrolling="no" title="West Orange PTA district calendar"></iframe>

## Each calendar on its own

Every calendar below can be viewed alone, added to your own Google Calendar, or subscribed to from Apple Calendar, Outlook, or any app that reads iCalendar (ICS) links.

<table class="calendars">
  <thead>
    <tr><th>Calendar</th><th>Type</th><th>Links</th></tr>
  </thead>
  <tbody>
  {%- for c in site.data.calendars %}
    <tr id="{{ c.key }}">
      <td><span class="swatch" style="background: {{ c.color }}"></span> {{ c.name }}</td>
      <td>{{ c.kind }}</td>
      <td>
      {%- if c.google_id %}
        <a href="https://calendar.google.com/calendar/embed?src={{ c.google_id | url_encode }}&ctz=America%2FNew_York">View</a> ·
        <a href="https://calendar.google.com/calendar/r?cid={{ c.google_id | url_encode }}">Add to Google Calendar</a> ·
        <a href="https://calendar.google.com/calendar/ical/{{ c.google_id | url_encode }}/public/basic.ics">ICS</a>
      {%- elsif c.ics %}
        <a href="https://calendar.google.com/calendar/r?cid={{ c.ics | url_encode }}">Add to Google Calendar</a> ·
        <a href="{{ c.ics }}">ICS</a>
      {%- else %}
        <em>Coming soon</em>
      {%- endif %}
      {%- if c.site %} · <a href="{{ c.site }}">Website</a>{% endif %}
      </td>
    </tr>
  {%- endfor %}
  </tbody>
</table>

## For PTA leaders

This calendar is a free service run by Mount Pleasant PTA for the {{ site.org.council_name }}. Each PTA keeps full control of its own calendar; the shared page just shows them side by side.

**Getting your PTA's calendar.** Email [{{ site.org.email }}](mailto:{{ site.org.email }}) with the Google account (Gmail or Workspace) of the person who will manage events. You receive a calendar that is already public and already on this page, with permission to add, edit, and delete events. Share it onward with other board members yourself; ownership stays with the shared WOPTA account so nothing is lost when officers change.

**Putting it on your own website.** Every calendar has its own embed. Copy the snippet below, replacing `CALENDAR_ID` with the ID shown in the "View" link for your PTA above:

```html
<iframe src="https://calendar.google.com/calendar/embed?src=CALENDAR_ID&ctz=America%2FNew_York"
        style="border:0" width="800" height="600" frameborder="0" scrolling="no"></iframe>
```

**Sharing with families.** Send them the "Add to Google Calendar" link for your PTA, or the ICS link for Apple and Outlook users. Events you add show up for everyone within minutes.

**Adding a community calendar.** Any organization that publishes a public ICS feed (the school district does) can be included. Anyone can propose one by editing [the calendar list on GitHub](https://github.com/mpe-wopta/mpe-wopta.github.io/edit/main/_data/calendars.yml) or by emailing us.

{%- if pending.size > 0 %}

Calendars still to be set up: {{ pending | join: ", " }}.
{%- endif %}
