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
  // Jekyll이 YAML 데이터를 JS 배열로 변환해줍니듸.
  const words = {{ site.data.words | jsonify }};
  let currentIndex = 0;
  let isFront = true;

  function updateCard() {
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
    isFront = !isFront;
    updateCard();
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
    words.sort(() => Math.random() - 0.5);
    currentIndex = 0;
    isFront = true;
    updateCard();
  }

  // 초기 로드
  window.onload = updateCard;
</script>
