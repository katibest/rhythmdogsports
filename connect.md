---
layout: default
permalink: /connect/
---

<section class="section--light">
  <h1 class="u-center--xs u-push-bottom--sm"></h1>
  {% capture connect-intro %}{% include  /snipets/connect-intro.md %}{% endcapture %}
  {% include image-pane.html image-url="../assets/img/agility-1.jpg" pane-align="left" pane-copy=connect-intro %}
</section>

<section class="section--dark">
  <div class="u-contained">
    <h2> Upcoming Events </h2>
    <div class="grid u-push-bottom--lg">

      {% assign sorted_events = site.event | sort: 'event_date' %}
      {% assign future_events = sorted_events | where_exp: "event", "event.event_date >= site.time" %}

      {% for event in future_events limit:2 %}
        <a href="{{ event.event_rsvp_link }}" target="blank" class="card card--feature card--date">
          <div class="card__date">
            <p class="date highlight">
              {{ event.event_date | date: "%m" }}
              <span class="date__divide">/</span>
              {{ event.event_date | date: "%d" }}
            </p>
          </div>
          <div class="card__content">
            {{ event.event_date | date_to_rfc822 | date: '%l:%M%P, %A, %B %e' }}
            <h3> {{ event.title }} </h3>
            <p class="note">{{ event.event_short_description }}</p>
            Learn more about {{ event.title }}  &rarr;
          </div>
        </a>
      {% endfor %}
    </div>
    <div class="u-center--xs">
      <a href="/events" class="btn btn--cta"><span>See All Events</span></a>
    </div>
  </div>
</section>
