---
layout: default
permalink: /who-we-are/
---

<section class="u-padded--none">
  <h1 class="u-center--xs">Who We Are </h1>
</section>
{% capture who-we-are-intro %}{% include /snipets/who-we-are-intro.md %}{% endcapture %}
{% include image-pane-splash.html image-url="/assets/img/stock-2.jpg" pane-align="center" copy=who-we-are-intro desktop-confetti="/assets/img/color-shapes-three.svg" mobile-confetti="/assets/img/color-shapes-four.svg" color="light" %}

<section class=" splash-intro section--dark section--dark-texture">
  <div class="grid grid--3 grid--mobile-2 u-contained u-push--auto">
    {% for team in site.team %}
      <div class="card card--people">
        <img src="../{{ team.image_path }}" alt="{{team.first_name}}-{{team.last_name}}">
        <div class="content">
          <h3>{{ team.first_name }} {{ team.last_name }}</h3>
          <p class="note">{{ team.job_title }}</p>
        </div>
      </div>
    {% endfor %}
  </div>
</section>

<div class="cta section--dark">
  <img src="/assets/img/color-shapes-one.svg" class="cta__confetti" />
  <div class="cta__container">
    <div>
      <h3> Do you like to build cool things, too? </h3> <a href="/careers" class="btn btn--cta"> Join our team! </a>
    </div>
  </div>
  <img src="/assets/img/color-shapes-two.svg" class="cta__confetti" />
</div>

<section class="section--light">
  <div class="u-padded">
    <div>
      <h2 class="u-center--xs u-push-bottom--xlg">The Gaslight Five: Our <span class="highlight">Core Values</span></h2>
      <div class="grid grid--stack u-center--xs">
        <div class="grid__item--a">
          <div class="u-center--xs"><img src="../assets/img/icons/better_together.svg"></div>
          <h4>We’re Better Together</h4>
          <p>Together we can do things we can’t do on our own. We value in-person collaboration because it’s still the best way to communicate.</p>
        </div>
        <div class="grid__item--b">
          <div class="u-center--xs"><img src="../assets/img/icons/trust_is_everything.svg"></div>
            <h4>Trust Is Everything</h4>
            <p>Trust allows us to move decisions close to the information. When trust goes up, speed goes up. Transparency is how we build and maintain trust: Always talk to the other person, even if it’s hard. Especially when it’s hard.</p>
        </div>
        <div class="grid__item--c">
          <div class="u-center--xs"><img src="../assets/img/icons/practice_empathy.svg"></div>
          <h4>Practice Empathy</h4>
          <p>Listen twice as much as you talk. Always assume the other person did the absolute best they could. Resolve conflicts. Don’t avoid them. Disagree about the idea, not the individual. Be kind to each other.</p>
        </div>
        <div class="grid__item--d">
          <div class="u-center--xs"><img src="../assets/img/icons/deliver_value.svg"></div>
          <h4>Continuously Deliver Value</h4>
          <p>Focus: always minimize work in progress. Deliver continuously. Do the next thing. Be proud to put your name on the work we’re delivering. Be accountable for the work that you’ve committed to doing. Don’t leave anyone guessing. Hold others accountable, too.</p>
        </div>
        <div class="grid__item--e">
          <div class="u-center--xs"><img src="../assets/img/icons/grow_sustainably.svg"></div>
          <h4>Grow Sustainably</h4>
          <p>Growth creates opportunities for our team, our clients and our company. Sustainable growth ensures our long-term success. We embrace constraints and use them to our advantage. Personal growth is necessary for each of us to improve. Ask others how you’re doing. Improve every day. Support the growth of others.</p>
        </div>
      </div>
    </div>
  </div>
</section>

{% capture cta-copy %}{% include /cta/cta-newsletter.md %}{% endcapture %}
{% include call-to-action.html cta-copy=cta-copy %}

{% capture popup-copy %}{% include /cta/cta-newsletter-popup.md %}{% endcapture %}
{% include modal.html popup-copy=popup-copy %}
