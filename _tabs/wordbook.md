---
layout: page
title: Word Book
permalink: /wordbook/
---

<div id="flashcard-container">
  <div id="card" onclick="flipCard()">
    <div id="card-content">준비 중...</div>
  </div>
  <div class="controls">
    <button onclick="prevCard()">이전</button>
    <button onclick="nextCard()">다음</button>
    <button onclick="shuffleCards()">섞기</button>
  </div>
  <p id="card-index"></p>
</div>

<script>
  // 데이터가 없을 경우를 대비해 빈 배열 '[]'을 기본값으로 설정합니듸.
  const words = {{ site.data.words | jsonify | default: '[]' }};
  let currentIndex = 0;
  let isFront = true;

  function updateCard() {
    // 데이터가 아예 없을 경우에 대한 예외 처리입니듸.
    if (!words || words.length === 0) {
      document.getElementById('card-content').innerText = "_data/words.yml 파일을 확인해 주십니듸.";
      return;
    }

    const cardContent = document.getElementById('card-content');
    const indexText = document.getElementById('card-index');
    
    if (isFront) {
      cardContent.innerText = words[currentIndex].word;
    } else {
      cardContent.innerText = words[currentIndex].definition;
    }
    indexText.innerText = `${currentIndex + 1} / ${words.length}`;
  }

  function flipCard() {
    if (words.length > 0) {
      isFront = !isFront;
      updateCard();
    }
  }

  function nextCard() {
    if (currentIndex < words.length - 1) {
      currentIndex++;
      isFront = true;
      updateCard();
    }
  }

  function prevCard() {
    if (currentIndex > 0) {
      currentIndex--;
      isFront = true;
      updateCard();
    }
  }

  function shuffleCards() {
    if (words.length > 0) {
      words.sort(() => Math.random() - 0.5);
      currentIndex = 0;
      isFront = true;
      updateCard();
    }
  }

  window.onload = updateCard;
</script>
