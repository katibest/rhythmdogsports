---
layout: default
permalink: /our-work/
---

<section class="u-padded--none">
<h1 class="u-center--xs">Our Work </h1>
</section>
{% capture our-work-intro %}{% include /snipets/our-work-intro.md %}{% endcapture %}
{% include image-pane-splash.html image-url="/assets/img/gaslight-user-thinking.jpg" pane-align="center"
copy=our-work-intro desktop-confetti="/assets/img/color-shapes-two.svg" mobile-confetti="/assets/img/color-shapes-four.svg" color="dark" %}

<section class="section--light">
  <div class="u-contained--max">
    <div class="grid grid--4">
      {% for casestudy in site.work %}
      {% capture services %} {{ casestudy.related_services | join: ", " }} {% endcapture %}
      {% capture url %} {{ casestudy.url }} {% endcapture %}
      {% include feature-cards.html card-url=casestudy.url card-title=casestudy.title card-copy=services card-image-path=casestudy.card_preview_image %}
      {% endfor %}
    </div>
  </div>
</section>

<section class="section--dark">
  {% capture awards-blurb %}{% include /snipets/awards-blurb.md %}{% endcapture %}
  {% include image-pane.html image-url="../assets/img/awards.jpg" pane-align="right" pane-copy=awards-blurb image-align="top" %}
</section>

<section class="section--light">
  <h2 class="u-text-center">Who we've <span class="highlight">worked</span> with</h2>
  <div class="logo-grid  u-contained--wide">
    <img src="../assets/img/client-logos/fern.png" alt="Fern Logo" class="logo--sm">
    <img src="../assets/img/client-logos/cincinnati-childrens.png" alt="Cincinnati Children's Logo">
    <img src="../assets/img/client-logos/computerease.png" alt="ComputerEase Logo">
    <img src="../assets/img/client-logos/e-beam.jpg" alt="E-Beam Logo">
    <img src="../assets/img/client-logos/haulright.svg" alt="Haulright Logo">
    <img src="../assets/img/client-logos/ilawnlogo.png" alt="iLawn Logo">
    <img src="../assets/img/client-logos/knovation.svg" alt="Knovation Logo" class="logo--sm">
    <img src="../assets/img/client-logos/mike-albert.png" alt="Mike Albert Logo" class="logo--lg">
    <img src="../assets/img/client-logos/navistone.png" alt="Navistone Logo" class="logo--sm">
    <img src="../assets/img/client-logos/ocm.svg" alt="Ocean Connect Marine Logo" class="logo--sm">
    <img src="../assets/img/client-logos/poa.png" alt="POA Logo">
    <img src="../assets/img/client-logos/ssa.png" alt="Second Story Auction Logo" class="logo--sm">
    <img src="../assets/img/client-logos/terrace-metrics.svg" alt="Terrace Metrics Logo">
    <img src="../assets/img/client-logos/trophy-awards.png" alt="Trophy Awards Logo" class="logo--sm">
  </div>
</section>

{% capture cta-copy %}{% include  /cta/cta-start-a-project.md %}{% endcapture %}
{% include call-to-action.html cta-copy=cta-copy %}
