---
layout: default
permalink: /what-we-do/
---

<section class="u-padded--none">
<h1 class="u-center--xs">What We Do </h1>
</section>
{% capture what-we-do-intro %}{% include /snipets/what-we-do-intro.md %}{% endcapture %}
{% include image-pane-splash.html image-url="/assets/img/gaslight-4.jpg" pane-align="center"
copy=what-we-do-intro desktop-confetti="/assets/img/color-shapes-two.svg" mobile-confetti="/assets/img/color-shapes-four.svg" color="light" %}


<section class="section--dark section--dark-texture">
  <div class="u-contained--wide  u-push--auto">
    <h2>Our Approach</h2>
    <div class="grid grid--3">
      <div class="grid__child grid__child--span-2">
        <p>
        We whole-heartedly believe that we are better together—so much so that we made it one of our core values. That's why when we work for you, you work with us. We pride ourselves on forming partnerships with our clients. Not in some legal-beagle kind of way, though we guess we'd be open to that, too; but a true partnership where we are working together toward a common goal—to build the best solution for your users.
        </p>
        <p>
        We prioritize open communication and active involvement from our clients—that's right, you're on this team, too! That's because we know software, you know your business, and our goal is to align the two in perfect harmony. That starts with a daily touchpoint, continues with tight feedback loops, and comes full circle with feature acceptance by—you guessed it—you!
        </p>
      </div>

      <ul class="u-text-xlg  list--confetti">
      <li>Discover</li>
      <li>Build</li>
      <li>Validate</li>
      <li>Repeat</li>
      </ul>
    </div>
  </div>
</section>

<section class="section--light">
  <h2 class="u-center--xs  u-push-bottom  u-contained  u-push--auto">Our Custom Services</h2>
  <p class="u-contained  u-push--auto">
  No matter where you are in the product lifecycle—ideation, validation, discovery, execution, or just need some more man power to bring it home—we're here to help. Let us step into the process with you, we're excited to see what we can do together.
  </p>

  <div class="service__container">
    <input type="radio" id="discovery" name="services" />
    <label class="service__category" for="discovery">
      <h4 class="u-text-right"><span>Discovery</span></h4>
    </label>

    <div class="service__list service__list--discovery">
    {% for service in site.service %}
    {% if service.category == "discovery" %}
    <div>
    <img src="{{ service.image_path }}">
    <h3>{{ service.title }}</h3>
    <p>{{ service.short_description }}</p>
    </div>
    {% endif %}
    {% endfor %}
    </div>


    <input type="radio" id="delivery" name="services" checked />
    <label class="service__category" for="delivery">
      <h4><span>Delivery</span></h4>
    </label>

    <div class="service__list service__list--delivery">
    {% for service in site.service %}
    {% if service.category == "delivery" %}
    <div>
    <img src="{{ service.image_path }}">
    <h3>{{ service.title }}</h3>
    <p>{{ service.short_description }}</p>
    </div>
    {% endif %}
    {% endfor %}
    </div>


    <input type="radio" id="team-augmentation" name="services" />
    <label class="service__category" for="team-augmentation">
      <h4 class="u-text-left"><span>Growth</span></h4>
    </label>
    <div class="service__list  service__list--team-augmentation">
    {% for service in site.service %}
    {% if service.category == "team-augmentation" %}
    <div>
    <img src="{{ service.image_path }}">
    <h3>{{ service.title }}</h3>
    <p>{{ service.short_description }}</p>
    </div>
    {% endif %}
    {% endfor %}
    </div>
  </div>
</section>




{% capture cta-copy %}{% include /cta/cta-start-a-project.md %}{% endcapture %}
{% include call-to-action.html cta-copy=cta-copy %}
