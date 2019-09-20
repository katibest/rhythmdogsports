---
layout: default
permalink: /style-guide/
---
  <section class="section--light">
  <h1 class="u-center--xs u-push-bottom--lg">Gaslight Design</h1>
  {% capture style-guide-intro %}{% include  /snipets/style-guide-intro.md %}{% endcapture %}
  {% include image-pane.html image-url="../assets/img/jossie.jpg" pane-align="right" pane-copy=style-guide-intro %}
</section>

<section class="section--light">
  <div class="u-contained--max">
    <h2><span class="highlight">Typography</span></h2>
    <div class="grid grid--2 u-push-bottom--lg">
      <div id="headers">
        <h1>&lt;h1&gt;Heading 1</h1>
        <h2>&lt;h2&gt;Heading 2</h2>
        <h3>&lt;h3&gt;Heading 3</h3>
        <h4>&lt;h4&gt;Heading 4</h4>
        <h5>&lt;h5&gt;Heading 5</h5>
        <h6>&lt;h6&gt;Heading 6</h6>
        <p>
          &lt;p&gt;Paragraph Tag: Sure, we love to geek out over technology and design. But it's all for a bigger purpose - boosting your business. We build apps that radically improve productivity, profits, client relationships and more.&lt;/p&gt;
        </p>
        <p class="caption">
          &lt;p class&equals;&quot;captions&quot;&gt;Caption Class: Sure, we love to geek out over technology and design. &lt;/p&gt;
        </p>
        <p>
          <i>&lt;i&gt;italic copy&lt;/i&gt;</i><br><b>&lt;b&gt;bold copy&lt;/b&gt;</b><br><strong>&lt;strong&gt;strong copy&lt;/strong&gt;</strong>
        </p>
        <p>
          <a href="#">&lt;a href&equals;&quot; &quot;&gt;a link tag&lt;/a&gt;</a>
        </p>
      </div>
      <div id="body-copy">
        <h3>Highlight <span class="highlight">&lt;span class&equals;&quot;highlight&quot;&gt;</span></h3>
        <blockquote>&lt;blockquote&gt;blockquote&lt;/blockquote&gt;</blockquote>
        <blockquote class="quote__right">&lt;blockquote class&equals;&quot;quote__right&quot;&gt; blockquote &lt;/blockquote&gt;</blockquote>
        <p>Sure, we love to geek out over technology and design. But it's all for a bigger purpose - boosting your business. We build apps that radically improve productivity, profits, client relationships and more.
        Sure, we love to geek out over technology and design. But it's all for a bigger purpose - boosting your business.</p>
        <blockquote class="quote__left">&lt;blockquote class&equals;&quot;quote__left&quot;&gt; blockquote &lt;/blockquote&gt;</blockquote>
        <p>Sure, we love to geek out over technology and design. But it's all for a bigger purpose - boosting your business. We build apps that radically improve productivity, profits, client relationships and more.
        Sure, we love to geek out over technology and design. But it's all for a bigger purpose - boosting your business.</p>
      </div>
    </div>
    <h2><span class="highlight">Color Scheme</span></h2>
    <div class="grid grid--4">
    <div class="u-padded--sm">
      <div class="u-padded--lg section--light u-push-bottom--sm">
      </div>
      <p>
        <b>Hex:</b> #ffffff<br />
        <b>Sass:</b> $color-light<br />
        <b>Background Color:</b> .section--light<br />
        <b>Font Color:</b> .color--light<br />
        <b>Border Color:</b> .border-color--light
      </p>
    </div>
    <div class="u-padded--sm">
      <div class="u-padded--lg section--off-light u-push-bottom--sm">
      </div>
      <p>
        <b>Hex:</b> #f2f2f2<br />
        <b>Sass:</b> $color-off-light<br />
        <b>Background Color:</b> .section--off-light<br />
        <b>Font Color:</b> .color--off-light<br />
        <b>Border Color:</b> .border-color--off-light
      </p>
    </div>
      <div class="u-padded--sm">
        <div class="u-padded--lg section--dark u-push-bottom--sm">
        </div>
        <p>
          <b>Hex:</b> #252525<br />
          <b>Sass:</b> $color-dark<br />
          <b>Background Color:</b> .section--dark<br />
          <b>Font Color:</b> .color--dark<br />
          <b>Border Color:</b> .border-color--dark
        </p>
      </div>
      <div class="u-padded--sm">
        <div class="u-padded--lg section--off-dark u-push-bottom--sm">
        </div>
        <p>
          <b>Hex:</b> #343434<br />
          <b>Sass:</b> $color-off-dark<br />
          <b>Background Color:</b> .section--off-dark<br />
          <b>Font Color:</b> .color--off-dark<br />
          <b>Border Color:</b> .border-color--off-dark
        </p>
      </div>
      <div class="u-padded--sm">
        <div class="u-padded--lg section--splash-1 u-push-bottom--sm">
        </div>
        <p>
          <b>Hex:</b> #33BFE5<br />
          <b>Sass:</b> $color-splash-1<br />
          <b>Background Color:</b> .section--splash-1<br />
          <b>Font Color:</b> .color--splash-1<br />
          <b>Border Color:</b> .border-color--splash-1
        </p>
      </div>
      <div class="u-padded--sm">
        <div class="u-padded--lg section--splash-2 u-push-bottom--sm">
        </div>
        <p>
          <b>Hex:</b> #f03330<br />
          <b>Sass:</b> $color-splash-2<br />
          <b>Background Color:</b> .section--splash-2<br />
          <b>Font Color:</b> .color--splash-2<br />
          <b>Border Color:</b> .border-color--splash-2
        </p>
      </div>
      <div class="u-padded--sm">
        <div class="u-padded--lg section--splash-3 u-push-bottom--sm">
        </div>
        <p>
          <b>Hex:</b> #FFD503<br />
          <b>Sass:</b> $color-splash-3<br />
          <b>Background Color:</b> .section--splash-3<br />
          <b>Font Color:</b> .color--splash-3<br />
          <b>Border Color:</b> .border-color--splash-3
        </p>
      </div>
    </div>
  </div>
</section>
<section class="section--dark">
  <div class="u-contained--wide">
    <h2><span class="highlight">Grid Layout</span></h2>
    <h3>2 Column Grid</h3>
    <h4>&lt;div class&equals;&quot;grid grid--2&quot;&gt;</h4>
    <div class="grid grid--2 u-push-bottom--lg">
      <div class="u-padded--lg section--off-light">
      </div>
      <div class="u-padded--lg section--off-light">
      </div>
    </div>
    <h3>3 Column Grid</h3>
    <h4>&lt;div class&equals;&quot;grid grid--3&quot;&gt;</h4>
    <div class="grid grid--3 u-push-bottom--lg">
      <div class="u-padded--lg section--off-light">
      </div>
      <div class="u-padded--lg section--off-light">
      </div>
      <div class="u-padded--lg section--off-light">
      </div>
    </div>
    <h3>4 Column Grid</h3>
    <h4>&lt;div class&equals;&quot;grid grid--4&quot;&gt;</h4>
    <div class="grid grid--4 u-push-bottom--lg">
      <div class="u-padded--lg section--off-light">
      </div>
      <div class="u-padded--lg section--off-light">
      </div>
      <div class="u-padded--lg section--off-light">
      </div>
      <div class="u-padded--lg section--off-light">
      </div>
    </div>
    <h3>5 Column Grid</h3>
    <h4>&lt;div class&equals;&quot;grid grid--5&quot;&gt;</h4>
    <div class="grid grid--5 u-push-bottom--lg">
      <div class="u-padded--lg section--off-light">
      </div>
      <div class="u-padded--lg section--off-light">
      </div>
      <div class="u-padded--lg section--off-light">
      </div>
      <div class="u-padded--lg section--off-light">
      </div>
      <div class="u-padded--lg section--off-light">
      </div>
    </div>
    <h3>Grid Item Spans 2 Columns</h3>
    <div class="grid grid--4 u-push-bottom--lg">
      <div class="grid__child--span-2 u-padded--lg section--splash-1">
        <h4>&lt;div class&equals;&quot;grid__child--span-2&quot;&gt;</h4>
      </div>
      <div class="u-padded--lg section--off-light">
      </div>
      <div class="u-padded--lg section--off-light">
      </div>
    </div>
    <h3>Grid Item Spans 3 Columns</h3>
    <div class="grid grid--4 u-push-bottom--lg">
      <div class="grid__child--span-3 u-padded--lg section--splash-3">
        <h4>&lt;div class&equals;&quot;grid__child--span-3&quot;&gt;</h4>
      </div>
      <div class="u-padded--lg section--off-light">
      </div>
    </div>
  </div>
</section>
