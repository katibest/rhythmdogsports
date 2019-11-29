---
layout: default
permalink: /events/
---

<section class="section--light">
  <h1 class="u-center--xs u-push-bottom--sm"></h1>
  {% capture events-intro %}{% include  /snipets/events-intro.md %}{% endcapture %}
  {% include image-pane.html image-url="../assets/img/events-1.jpg" pane-align="right" pane-copy=events-intro %}
</section>

<section class="section--light">
  <div class="u-contained">
    <h2> Upcoming Trials </h2>
    <div class="grid">

      {% assign sorted_events = site.event | sort: 'event_date' %}
      {% assign future_events = sorted_events | where_exp: "event", "event.event_date >= site.time" %}

      {% for event in future_events limit:5 %}
        <a href="{{ event.event_rsvp_link }}" target="_blank" class="card card--date">
          <div class="card__date">
            <p class="date highlight">
              {{ event.event_date | date: "%m" }}
              <span class="date__divide">/</span>
              {{ event.event_date | date: "%d" }}
            </p>
          </div>
          <div class="card__content">
            <span class="color--splash-1">{{ event.event_date | date_to_rfc822 | date: '%l:%M%P, %A, %B %e' }}</span>
            <h3> {{ event.title }} </h3>
            <p class="note">{{ event.event_short_description }}</p>
            Learn more about {{ event.title }}  &rarr;
          </div>
        </a>
      {% endfor %}
    </div>
  </div>
</section>
