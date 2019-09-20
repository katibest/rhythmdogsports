---
layout: default
permalink: /events/
---

<section class="section--light">
  <h1 class="u-center--xs u-push-bottom--sm"> Events </h1>
  {% capture events-intro %}{% include  /snipets/events-intro.md %}{% endcapture %}
  {% include image-pane.html image-url="../assets/img/meetup-9.jpg" pane-align="right" pane-copy=events-intro %}
</section>

{% assign featured_events = site.event | where: "featured_event", true | reverse %}

{% for event in featured_events limit:1 %}
  <section class="section--dark section--dark-texture">
    <div class="u-contained--wide">
      <h2> Featured Event </h2>
    </div>

    {% capture featured-event %} {% include /snipets/featured-event.md %} {% endcapture %}
    {% capture featured-event-image %} {{ event.featured_event_image }} {% endcapture %}

    {% include image-pane.html image-url=featured-event-image pane-align="left" pane-copy=featured-event %}
  </section>
{% endfor %}

<section class="section--light">
  <div class="u-contained">
    <h2> Upcoming Events </h2>
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

{% capture cta-copy %}{% include /cta/cta-start-a-project.md %}{% endcapture %}
{% include call-to-action.html cta-copy=cta-copy %}
