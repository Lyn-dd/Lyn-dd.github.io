---
layout: page
title: Diary
icon: fas fa-fw fa-book
order: 7
---

<div class="diary-frame-wrap">
  <iframe
    id="diary-frame"
    class="diary-frame"
    src="{{ '/Diary.html' | relative_url }}"
    title="DIARY"
    loading="eager"
    data-proofer-ignore
  ></iframe>
</div>

<p class="diary-fallback">
  화면이 뜨지 않으면
  <a
    href="{{ '/Diary.html' | relative_url }}"
    target="_blank"
    rel="noopener"
    data-proofer-ignore
  >DIARY 단독 페이지</a>를 열어주세요.
</p>

<style>
.diary-frame-wrap {
  width: 100%;
  min-height: 920px;
}

.diary-frame {
  display: block;
  width: 100%;
  height: 920px;
  border: none;
}
</style>
