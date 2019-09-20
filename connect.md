---
layout: default
permalink: /connect/
---

<section class="section--light">
  <h1 class="u-center--xs u-push-bottom--sm">Connect With Us</h1>
  {% capture connect-intro %}{% include  /snipets/connect-intro.md %}{% endcapture %}
  {% include image-pane.html image-url="../assets/img/gaslight-city.jpg" pane-align="left" pane-copy=connect-intro %}
</section>

<section class="section--dark section--dark-texture">
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
      <a href="/events" class="btn"><span>See All Events</span></a>
    </div>
  </div>
</section>

<section class="section--light">
  <div class="u-contained--max grid grid--4">
    <div class="grid__child--span-3">
      <iframe frameborder="0" height="550" src="https://www.google.com/maps/embed?pb=!1m24!1m12!1m3!1d3096.095230951347!2d-84.512097!3d39.104302!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!4m9!1i0!3e6!4m0!4m5!1s0x8841b15758e2163f%3A0xe583a6159a6b0780!2s708+Walnut+St%2C+Cincinnati%2C+OH+45202!3m2!1d39.1043198!2d-84.5118912!5e0!3m2!1sen!2sus!4v1423836575211" style="border:0" width="100%"></iframe>
    </div>
    <div>
      <h3>Contact Us</h3>
       <h4 class="u-push-bottom--none">Email</h4>
       <p><a href="mailto:hello@teamgaslight.com">hello@teamgaslight.com</a></p>
      <h4 class="u-push-bottom--none">Office</h4>
      <p>
       708 Walnut St Suite 500
       <br>
       Cincinnati, Ohio 45202
     </p>
     <div class="u-push-bottom--lg">
       <a target="blank"  href="https://goo.gl/maps/I84jA">Directions &gt;</a>
       <br />
       <a  target="blank"  href="https://teamgaslight.com/blog/how-we-commute-to-gaslights-new-downtown-office-and-how-you-can-too">Tips for parking, biking and busing &gt;</a>
     </div>
    </div>
  </div>
</section>

{% capture cta-copy %}{% include  /cta/cta-start-a-project.md %}{% endcapture %}
{% include call-to-action.html cta-copy=cta-copy %}
