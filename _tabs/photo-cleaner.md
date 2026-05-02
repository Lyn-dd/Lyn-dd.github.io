---
layout: page
title: PHOTO CLEANER
icon: fas fa-fw fa-images
order: 6
---

<div class="photo-cleaner-frame-wrap">
  <iframe
    class="photo-cleaner-frame"
    src="/photo-cleaner-app.html"
    title="PHOTO CLEANER"
    loading="eager"
    allow="clipboard-write"
  ></iframe>
</div>

<style>
  .photo-cleaner-frame-wrap {
    width: 100%;
    min-height: 860px;
  }
  .photo-cleaner-frame {
    width: 100%;
    min-height: 860px;
    border: 0;
    border-radius: 18px;
    background: transparent;
  }
  @media (max-width: 680px) {
    .photo-cleaner-frame-wrap,
    .photo-cleaner-frame {
      min-height: 940px;
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
