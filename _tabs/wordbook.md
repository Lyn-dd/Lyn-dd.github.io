---
layout: page
title: Word Book
icon: fas fa-fw fa-book
order: 7
permalink: /wordbook/
---

<style>
.wordbook {
  max-width: 720px;
  margin: 2rem auto;
  text-align: center;
}

.wordbook-card {
  width: 100%;
  min-height: 240px;
  padding: 2rem;
  border: 1px solid var(--bs-border-color, #ddd);
  border-radius: 1rem;
  background: var(--card-bg, #fff);
  color: inherit;
  font-size: 1.6rem;
  font-weight: 700;
  cursor: pointer;
  box-shadow: 0 0.5rem 1.25rem rgba(0, 0, 0, 0.08);
}

.wordbook-card:hover {
  transform: translateY(-2px);
}

.wordbook-index {
  margin: 1rem 0;
  opacity: 0.75;
}

.wordbook-controls {
  display: flex;
  justify-content: center;
  gap: 0.75rem;
  flex-wrap: wrap;
}

.wordbook-controls button {
  padding: 0.5rem 1rem;
  border: 1px solid var(--bs-border-color, #ddd);
  border-radius: 0.5rem;
  background: transparent;
  color: inherit;
  cursor: pointer;
}
</style>

<div id="flashcard-container" class="wordbook">
  <button type="button" id="card" class="wordbook-card" aria-label="카드 뒤집기">
    <span id="card-content">준비 중...</span>
  </button>

  <p id="card-index" class="wordbook-index"></p>

  <div class="wordbook-controls">
    <button type="button" id="word-prev">이전</button>
    <button type="button" id="word-next">다음</button>
    <button type="button" id="word-shuffle">섞기</button>
  </div>
</div>

<script id="word-data" type="application/json">
{{ site.data.words | jsonify }}
</script>

<script>
(() => {
  function initWordBook() {
    const dataEl = document.getElementById('word-data');
    const card = document.getElementById('card');
    const cardContent = document.getElementById('card-content');
    const indexText = document.getElementById('card-index');
    const prevBtn = document.getElementById('word-prev');
    const nextBtn = document.getElementById('word-next');
    const shuffleBtn = document.getElementById('word-shuffle');

    if (!card || !cardContent || !indexText) {
      return;
    }

    let words = [];

    try {
      const parsed = JSON.parse(dataEl?.textContent || '[]');
      words = Array.isArray(parsed) ? parsed : [];
    } catch (error) {
      console.error('Word data parse error:', error);
    }

    let currentIndex = 0;
    let isFront = true;

    function showCard() {
      if (!words.length) {
        cardContent.textContent = '_data/words.yml 파일을 확인해 주십니듸.';
        indexText.textContent = '';
        return;
      }

      const item = words[currentIndex] || {};
      const front = item.word || '';
      const back = item.definition || item.meaning || '';

      cardContent.textContent = isFront ? front : back;
      indexText.textContent = `${currentIndex + 1} / ${words.length}`;
    }

    card.addEventListener('click', () => {
      if (!words.length) return;
      isFront = !isFront;
      showCard();
    });

    prevBtn?.addEventListener('click', () => {
      if (!words.length) return;
      currentIndex = (currentIndex - 1 + words.length) % words.length;
      isFront = true;
      showCard();
    });

    nextBtn?.addEventListener('click', () => {
      if (!words.length) return;
      currentIndex = (currentIndex + 1) % words.length;
      isFront = true;
      showCard();
    });

    shuffleBtn?.addEventListener('click', () => {
      if (!words.length) return;

      for (let i = words.length - 1; i > 0; i -= 1) {
        const j = Math.floor(Math.random() * (i + 1));
        [words[i], words[j]] = [words[j], words[i]];
      }

      currentIndex = 0;
      isFront = true;
      showCard();
    });

    showCard();
  }

  if (document.readyState === 'loading') {
    document.addEventListener('DOMContentLoaded', initWordBook, { once: true });
  } else {
    initWordBook();
  }
})();
</script>
