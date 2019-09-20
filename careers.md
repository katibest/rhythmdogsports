---
layout: default
permalink: /careers/
---
<section class="section--light">
  <h1 class="u-center--xs u-push-bottom--sm">Careers</h1>
  {% capture careers-intro %}{% include /snipets/careers-intro.md %}{% endcapture %}
  {% include image-pane.html image-url="../assets/img/stock-59.jpg" pane-align="right" pane-copy=careers-intro %}
</section>

<section class="section--light">
  <div class="u-contained--wide">
    <div class="grid grid--2">
      {% for career in site.careers %}
        <a href="{{ career.url }}" class="card card--feature">
          <div class="card__content">
            <h3>{{ career.title }}</h3>
            <p class="note">{{ career.short_description }}</p>
            Learn more about the {{ career.title }} role  &rarr;
          </div>
        </a>
      {% endfor %}
    </div>
  </div>
</section>

<section class="section--dark section--dark-texture">
  <div class="section__copy">
    <h2 class="u-text-center">It comes with perks, too.</h2>
    <div class="grid grid--3  u-push-top--lg">
      {% for perk in site.perks %}
        <div>
          <img src="{{ perk.image_path }}" class="u-push-bottom--sm">
          <h4>{{ perk.title }}</h4>
          <p>{{ perk.short_description }}</p>
        </div>
      {% endfor %}
    </div>
  </div>
</section>

{% capture cta-copy %}{% include cta/cta-start-a-project.md %}{% endcapture %}
{% include call-to-action.html cta-copy=cta-copy %}
