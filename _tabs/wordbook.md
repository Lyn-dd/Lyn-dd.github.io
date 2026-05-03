---
layout: page
title: WORD BOOK
icon: fas fa-fw fa-book
order: 7
permalink: /wordbook/
---

<div class="wordbook-frame-wrap">
  <iframe
    id="wordbook-frame"
    class="wordbook-frame"
    src="{{ '/wordbook-app.html' | relative_url }}"
    title="WORD BOOK"
    loading="eager"
    data-proofer-ignore
  ></iframe>
</div>

<p class="wordbook-fallback">
  화면이 뜨지 않으면
  <a
    href="{{ '/wordbook-app.html' | relative_url }}"
    target="_blank"
    rel="noopener"
    data-proofer-ignore
  >WORD BOOK 단독 페이지</a>를 열어주세요.
</p>

<style>
  .wordbook-frame-wrap {
    width: 100%;
    min-height: 760px;
  }

  .wordbook-frame {
    display: block;
    width: 100%;
    height: 760px;
    border: 0;
    border-radius: 18px;
    background: transparent;
  }

  .wordbook-fallback {
    margin-top: 0.75rem;
    font-size: 0.95rem;
    opacity: 0.75;
  }

  @media (max-width: 680px) {
    .wordbook-frame,
    .wordbook-frame-wrap {
      min-height: 820px;
      height: 820px;
    }
  }
</style>

<script>
  window.addEventListener('message', function (event) {
    if (event.origin !== window.location.origin) return;
    if (!event.data || event.data.type !== 'wordbook-height') return;

    var frame = document.getElementById('wordbook-frame');
    if (!frame) return;

    var height = Math.max(760, Number(event.data.height) || 0);
    frame.style.height = height + 'px';
  });

  document.addEventListener('keydown', function (event) {
    var activeTag = document.activeElement && document.activeElement.tagName
      ? document.activeElement.tagName.toLowerCase()
      : '';

    if (['input', 'textarea', 'select'].indexOf(activeTag) !== -1) return;

    if (event.key !== 'ArrowLeft' && event.key !== 'ArrowRight') return;

    var frame = document.getElementById('wordbook-frame');
    if (!frame || !frame.contentWindow) return;

    event.preventDefault();

    frame.contentWindow.postMessage(
      {
        type: 'wordbook-key',
        key: event.key
      },
      window.location.origin
    );
  });
</script>
