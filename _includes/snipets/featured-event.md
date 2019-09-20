<h2 class="highlight"><span>{{event.title}}</span></h2>
<h4> {{ event.event_date | date_to_rfc822 | date: '%l:%M%P, %A, %B %e' }} </h4>
<p>{{ event.event_short_description }}</p>
<a href="{{ event.event_rsvp_link }}" target="_blank" class="btn u-push-top--sm">Learn more about {{ event.title }}  &rarr;</a>
