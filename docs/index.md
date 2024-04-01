---
layout: home
title: Orillusion
titleTemplate: Некст-ген WebGPU движок

hero:
  name: Orillusion
  image:
    light: /en/images/logo_black.png
    dark: /en/images/logo_white.png
  text: Next Generation WebGPU Engine
  tagline: Простой но мощный для Web3D разработчиков
  actions:
    - theme: brand
      text: Начало
      link: /guide/
    - theme: alt
      text: Примеры
      link: /example/base/AddRemove
    - theme: alt
      text: GitHub
      link: https://github.com/Orillusion/orillusion

features:
  - title: Доступный
    details: Освежающе простой 3D-движок, управляемый данными, встроенный в JavaScript. Бесплатный и открытый исходный код навсегда!
  - title: Универсальный
    details: Постепенно расширяемая структура ECS, которая масштабируется между библиотекой и полнофункциональным продуктом.
  - title: Производительный
    details: 
      Чистый веб-кроссплатформенный рантайм.<br>
      Быстрый WebGPU рендер.<br>
      Минимальные усилия по оптимизации.

---

<div class="heroDemos">
  <div class="container">
    <Demo src="/examples/pbr.ts" :code="false" :codepen="false" :fullscreen="false" :height="450" style="margin:0"></Demo>
    <Demo src="/examples/pbr2.ts" :code="false" :codepen="false" :fullscreen="false" :height="450" style="margin:0"></Demo>
  </div>
</div>
<Logo :homeHero="true"></Logo>
