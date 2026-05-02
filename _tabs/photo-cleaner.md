---
layout: page
title: PHOTO CLEANER
icon: fas fa-fw fa-images
order: 6
---

<div class="photo-cleaner-frame-wrap">
  <iframe
    id="photo-cleaner-frame"
    class="photo-cleaner-frame"
    src="{{ '/photo-cleaner-app.html' | relative_url }}"
    title="PHOTO CLEANER"
    loading="eager"
    data-proofer-ignore
  ></iframe>
</div>

<p class="photo-cleaner-fallback">
  화면이 뜨지 않으면
  <a
    href="{{ '/photo-cleaner-app.html' | relative_url }}"
    target="_blank"
    rel="noopener"
    data-proofer-ignore
  >PHOTO CLEANER 단독 페이지</a>를 열어주세요.
</p>

<style>
  .photo-cleaner-frame-wrap {
    width: 100%;
    min-height: 920px;
  }

  .photo-cleaner-frame {
    display: block;
    width: 100%;
    height: 920px;
    border: 0;
    border-radius: 18px;
    background: transparent;
  }

  .photo-cleaner-fallback {
    margin-top: 0.75rem;
    font-size: 0.95rem;
    opacity: 0.75;
  }

  @media (max-width: 680px) {
    .photo-cleaner-frame,
    .photo-cleaner-frame-wrap {
      min-height: 980px;
      height: 980px;
    }
  }
</style>

<script>
  window.addEventListener('message', function (event) {
    if (event.origin !== window.location.origin) return;
    if (!event.data || event.data.type !== 'photo-cleaner-height') return;

    var frame = document.getElementById('photo-cleaner-frame');
    if (!frame) return;

    var height = Math.max(920, Number(event.data.height) || 0);
    frame.style.height = height + 'px';
  });
</script>
