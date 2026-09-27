---
layout: default
title: District calendar
permalink: /calendar/
description: An experiment in showing every West Orange PTA's events, the district calendar, and community schedules in one place.
---
{% comment %}
GitHub Pages builds with Jekyll 3.10, whose where_exp filter cannot take "and"/"or" or a bare
variable, so the embed list is built with a plain for/if loop (core Liquid only). The outer tags
here deliberately do NOT trim whitespace: trimming swallowed the blank line after the heading and
kramdown folded the first paragraph into the H1.
{% endcomment %}
{% capture srcs %}
  {%- for c in site.data.calendars -%}{%- if c.google_id -%}&src={{ c.google_id | url_encode }}&color={{ c.color | url_encode }}{%- endif -%}{%- endfor -%}
{% endcapture %}

# West Orange PTA calendars

**An experiment.** One calendar for the whole district: each PTA's events in its own color, plus the school district's calendar and selected community schedules. Today only the Mount Pleasant calendar is live; the others are placeholders so the shape of the idea is visible. Use the dropdown in the top-right of the calendar to show or hide individual calendars.

<iframe src="https://calendar.google.com/calendar/embed?height=600&wkst=1&ctz=America%2FNew_York&showPrint=0&showCalendars=1&showTz=0{{ srcs | strip }}" style="border:solid 1px #777" width="800" height="600" frameborder="0" scrolling="no" title="West Orange PTA district calendar"></iframe>

## Each calendar on its own

Every live calendar can be viewed alone, added to your own Google Calendar, or subscribed to from Apple Calendar, Outlook, or any app that reads iCalendar (ICS) links.

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
        <em>Placeholder</em>
      {%- endif %}
      {%- if c.site %} · <a href="{{ c.site }}">Website</a>{% endif %}
      </td>
    </tr>
  {%- endfor %}
  </tbody>
</table>

## How this could work

Nothing here is adopted yet; this page is a proof of concept for the {{ site.org.council_name }} to look at. If it catches on, the idea is simple: every PTA keeps full control of its own calendar, and this page just shows them side by side.

**One calendar per PTA.** Each PTA would get a Google calendar of its own, public, already on this page, with its board given permission to add, edit, and delete events. Ownership would sit with a shared account so nothing is lost when officers change, and each board could share it onward with its own volunteers.

**On your own website.** Every calendar has its own embed. The snippet would be the same for everyone, with `CALENDAR_ID` swapped for the ID in that PTA's "View" link:

```html
<iframe src="https://calendar.google.com/calendar/embed?src=CALENDAR_ID&ctz=America%2FNew_York"
        style="border:0" width="800" height="600" frameborder="0" scrolling="no"></iframe>
```

**For families.** The "Add to Google Calendar" link puts a PTA's events into a parent's own calendar, and the ICS link does the same for Apple and Outlook. Events added later show up on their own; nobody has to re-subscribe.

**Community calendars.** Any organization that publishes a public ICS feed can be included; the school district already does, which is why its calendar is live here. The list of calendars is [a plain text file on GitHub](https://github.com/mpe-wopta/mpe-wopta.github.io/blob/main/_data/calendars.yml), so adding one is a small edit.
