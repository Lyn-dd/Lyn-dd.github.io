---
layout: page
title: PHOTO CLEANER
icon: fas fa-fw fa-images
order: 6
---

{% raw %}
<div id="pc-app" class="pc-app">
  <section id="pc-intro" class="pc-panel">
    <h2>PHOTO CLEANER</h2>
    <p class="pc-muted">이미지를 브라우저에서 분류한 후, 삭제 예정 파일을 휴지통으로 옮깁니다.</p>

    <div id="pc-drop-zone" class="pc-drop-zone" tabindex="0" role="button" aria-label="이미지 파일 또는 폴더를 끌어다 놓기">
      <strong>이미지 폴더나 이미지 파일들을 여기에 드래그 앤 드롭하세요.</strong>
      <span>또는 아래 버튼으로 직접 고를 수 있습니다.</span>
    </div>

    <div class="pc-actions">
      <button type="button" id="pc-pick-folder" class="pc-btn pc-btn-primary">폴더 선택</button>
      <button type="button" id="pc-pick-files" class="pc-btn">파일 여러 개 선택</button>
    </div>

    <p class="pc-note">권장 방식은 <strong>폴더 선택</strong>. 그래야 하위 폴더 경로까지 삭제 스크립트에 정확히 들어갑니다.</p>

    <input id="pc-folder-input" type="file" accept="image/*" webkitdirectory directory multiple hidden>
    <input id="pc-file-input" type="file" accept="image/*" multiple hidden>
  </section>

  <section id="pc-workspace" class="pc-panel" hidden>
    <div class="pc-topbar">
      <div>
        <strong id="pc-status">준비 중...</strong>
        <div id="pc-file-name" class="pc-muted"></div>
      </div>
      <button type="button" id="pc-reset" class="pc-btn pc-btn-small">처음부터</button>
    </div>

    <div class="pc-progress" aria-label="진행률">
      <div id="pc-progress-bar" class="pc-progress-bar"></div>
    </div>

    <div id="pc-card" class="pc-card" aria-live="polite">
      <div class="pc-swipe-label pc-swipe-left">유지</div>
      <div class="pc-swipe-label pc-swipe-right">삭제</div>
      <img id="pc-image" class="no-lazyload" data-no-lightgallery="true" alt="현재 이미지" draggable="false">
      <div id="pc-fallback" class="pc-fallback" hidden>
        <strong>미리보기를 표시할 수 없는 이미지 형식입니다.</strong>
        <span>분류는 계속 가능합니다.</span>
      </div>
    </div>

    <div class="pc-controls" aria-label="분류 조작">
      <button type="button" id="pc-keep" class="pc-btn pc-btn-keep">← 유지</button>
      <button type="button" id="pc-undo" class="pc-btn">되돌리기</button>
      <button type="button" id="pc-delete" class="pc-btn pc-btn-delete">삭제 →</button>
    </div>

    <p class="pc-note">마우스/터치로 왼쪽 스와이프하면 유지, 오른쪽 스와이프하면 삭제 예정입니다. 키보드는 ← 유지, → 삭제, Z 또는 Backspace 되돌리기.</p>
  </section>

  <section id="pc-done" class="pc-panel" hidden>
    <h2>분류 완료!</h2>
    <p id="pc-summary" class="pc-summary"></p>
    <div class="pc-actions">
      <button type="button" id="pc-download-bat" class="pc-btn pc-btn-primary">Windows 휴지통 BAT 다운로드</button>
      <button type="button" id="pc-download-list" class="pc-btn">삭제 목록 TXT 다운로드</button>
      <button type="button" id="pc-restart" class="pc-btn">다시 분류</button>
    </div>
    <p class="pc-note">다운로드한 BAT를 실행하면 폴더 선택창이 뜹니다. 방금 이미지를 불러온 원본 폴더를 선택하면, 삭제 예정 파일만 휴지통으로 이동합니다.</p>
  </section>
</div>

<style>
  #pc-app, #pc-app * { box-sizing: border-box; }
  #pc-app [hidden] { display: none !important; }
  #pc-app {
    --pc-accent: #4f8cff;
    --pc-keep: #2ea44f;
    --pc-delete: #d73a49;
    --pc-border: rgba(127, 127, 127, 0.28);
    --pc-soft: rgba(127, 127, 127, 0.10);
    --pc-strong: rgba(127, 127, 127, 0.18);
    max-width: 980px;
    margin: 0 auto;
  }
  #pc-app .pc-panel {
    border: 1px solid var(--pc-border);
    border-radius: 18px;
    padding: 24px;
    background: var(--pc-soft);
  }
  #pc-app .pc-muted,
  #pc-app .pc-note {
    opacity: 0.78;
  }
  #pc-app .pc-note {
    margin: 14px 0 0;
    font-size: 0.95rem;
  }
  #pc-app .pc-drop-zone {
    display: grid;
    gap: 8px;
    place-items: center;
    min-height: 180px;
    margin: 18px 0;
    padding: 28px;
    border: 3px dashed var(--pc-border);
    border-radius: 18px;
    text-align: center;
    cursor: pointer;
    transition: transform 160ms ease, border-color 160ms ease, background 160ms ease;
    user-select: none;
  }
  #pc-app .pc-drop-zone:hover,
  #pc-app .pc-drop-zone:focus,
  #pc-app .pc-drop-zone.pc-dragging {
    border-color: var(--pc-accent);
    background: rgba(79, 140, 255, 0.12);
    transform: translateY(-1px);
    outline: none;
  }
  #pc-app .pc-actions,
  #pc-app .pc-controls {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    justify-content: center;
    align-items: center;
  }
  #pc-app .pc-btn {
    border: 1px solid var(--pc-border);
    border-radius: 999px;
    padding: 10px 18px;
    background: var(--pc-strong);
    color: inherit;
    cursor: pointer;
    font: inherit;
    line-height: 1.2;
    transition: transform 120ms ease, filter 120ms ease, background 120ms ease;
  }
  #pc-app .pc-btn:hover {
    transform: translateY(-1px);
    filter: brightness(1.05);
  }
  #pc-app .pc-btn:active {
    transform: translateY(0);
  }
  #pc-app .pc-btn-primary {
    background: var(--pc-accent);
    color: white;
    border-color: transparent;
  }
  #pc-app .pc-btn-keep {
    background: var(--pc-keep);
    color: white;
    border-color: transparent;
  }
  #pc-app .pc-btn-delete {
    background: var(--pc-delete);
    color: white;
    border-color: transparent;
  }
  #pc-app .pc-btn-small {
    padding: 7px 12px;
    font-size: 0.9rem;
  }
  #pc-app .pc-topbar {
    display: flex;
    gap: 12px;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 12px;
  }
  #pc-app .pc-progress {
    width: 100%;
    height: 10px;
    overflow: hidden;
    border-radius: 999px;
    background: var(--pc-strong);
    margin-bottom: 14px;
  }
  #pc-app .pc-progress-bar {
    width: 0%;
    height: 100%;
    background: var(--pc-accent);
    transition: width 180ms ease;
  }
  #pc-app .pc-card {
    position: relative;
    display: grid;
    place-items: center;
    min-height: min(68vh, 650px);
    overflow: hidden;
    border-radius: 18px;
    border: 1px solid var(--pc-border);
    background:
      linear-gradient(45deg, rgba(127,127,127,0.10) 25%, transparent 25%),
      linear-gradient(-45deg, rgba(127,127,127,0.10) 25%, transparent 25%),
      linear-gradient(45deg, transparent 75%, rgba(127,127,127,0.10) 75%),
      linear-gradient(-45deg, transparent 75%, rgba(127,127,127,0.10) 75%);
    background-size: 24px 24px;
    background-position: 0 0, 0 12px, 12px -12px, -12px 0;
    touch-action: pan-y;
    user-select: none;
    will-change: transform;
  }
  #pc-app .pc-card img {
    display: block;
    max-width: 100% !important;
    max-height: min(68vh, 650px) !important;
    width: auto !important;
    height: auto !important;
    margin: 0 auto !important;
    object-fit: contain;
    border-radius: 12px;
    box-shadow: 0 18px 48px rgba(0, 0, 0, 0.30);
    pointer-events: none;
  }
  #pc-app .pc-fallback {
    display: grid;
    gap: 8px;
    place-items: center;
    padding: 28px;
    text-align: center;
  }
  #pc-app .pc-swipe-label {
    position: absolute;
    z-index: 2;
    top: 20px;
    padding: 8px 14px;
    border-radius: 999px;
    color: white;
    font-weight: 800;
    letter-spacing: 0.06em;
    opacity: 0;
    transform: scale(0.94);
    transition: opacity 100ms ease, transform 100ms ease;
    pointer-events: none;
  }
  #pc-app .pc-swipe-left {
    left: 20px;
    background: var(--pc-keep);
  }
  #pc-app .pc-swipe-right {
    right: 20px;
    background: var(--pc-delete);
  }
  #pc-app .pc-card.pc-show-keep .pc-swipe-left,
  #pc-app .pc-card.pc-show-delete .pc-swipe-right {
    opacity: 1;
    transform: scale(1);
  }
  #pc-app .pc-controls {
    margin-top: 14px;
  }
  #pc-app .pc-summary {
    font-size: 1.1rem;
    font-weight: 700;
  }
  @media (max-width: 640px) {
    #pc-app .pc-panel { padding: 16px; }
    #pc-app .pc-card { min-height: 58vh; }
    #pc-app .pc-card img { max-height: 58vh !important; }
    #pc-app .pc-topbar { flex-direction: column; }
    #pc-app .pc-actions, #pc-app .pc-controls { align-items: stretch; }
    #pc-app .pc-btn { width: 100%; }
  }
</style>

<script>
(() => {
  'use strict';

  const IMAGE_EXT_RE = /\.(avif|bmp|gif|heic|heif|jpe?g|png|svg|tiff?|webp)$/i;
  const collator = new Intl.Collator('ko-KR', { numeric: true, sensitivity: 'base' });

  const el = {
    intro: document.getElementById('pc-intro'),
    dropZone: document.getElementById('pc-drop-zone'),
    pickFolder: document.getElementById('pc-pick-folder'),
    pickFiles: document.getElementById('pc-pick-files'),
    folderInput: document.getElementById('pc-folder-input'),
    fileInput: document.getElementById('pc-file-input'),
    workspace: document.getElementById('pc-workspace'),
    done: document.getElementById('pc-done'),
    status: document.getElementById('pc-status'),
    fileName: document.getElementById('pc-file-name'),
    progressBar: document.getElementById('pc-progress-bar'),
    card: document.getElementById('pc-card'),
    image: document.getElementById('pc-image'),
    fallback: document.getElementById('pc-fallback'),
    keep: document.getElementById('pc-keep'),
    delete: document.getElementById('pc-delete'),
    undo: document.getElementById('pc-undo'),
    reset: document.getElementById('pc-reset'),
    summary: document.getElementById('pc-summary'),
    downloadBat: document.getElementById('pc-download-bat'),
    downloadList: document.getElementById('pc-download-list'),
    restart: document.getElementById('pc-restart')
  };

  const state = {
    items: [],
    index: 0,
    decisions: [],
    rootHint: '',
    activeUrl: null,
    preload: null,
    gesture: null,
    animating: false
  };

  function isImageFile(file) {
    return Boolean(file && ((file.type && file.type.startsWith('image/')) || IMAGE_EXT_RE.test(file.name || '')));
  }

  function normalizePath(path) {
    const raw = String(path || '').replace(/\\+/g, '/').replace(/^[a-zA-Z]:\//, '').replace(/^\/+/, '');
    const parts = raw.split('/').filter((part) => part && part !== '.' && part !== '..');
    return parts.join('/') || 'unknown-image';
  }

  function stripFirstSegment(path) {
    const parts = normalizePath(path).split('/');
    return parts.length > 1 ? parts.slice(1).join('/') : parts[0];
  }

  function makeRecord(file, relativePath) {
    return {
      file,
      name: file.name || 'image',
      size: file.size || 0,
      type: file.type || '',
      relativePath: normalizePath(relativePath || file.webkitRelativePath || file.name || 'image')
    };
  }

  function finalizeRecords(records) {
    return records
      .filter((record) => record && isImageFile(record.file))
      .sort((a, b) => collator.compare(a.relativePath, b.relativePath));
  }

  function getDeletedItems() {
    return state.decisions.filter((entry) => entry.action === 'delete').map((entry) => entry.item);
  }

  function getKeepCount() {
    return state.decisions.filter((entry) => entry.action === 'keep').length;
  }

  function formatBytes(bytes) {
    if (!Number.isFinite(bytes) || bytes <= 0) return '0 B';
    const units = ['B', 'KB', 'MB', 'GB'];
    let value = bytes;
    let unitIndex = 0;
    while (value >= 1024 && unitIndex < units.length - 1) {
      value /= 1024;
      unitIndex += 1;
    }
    return `${value.toFixed(value >= 10 || unitIndex === 0 ? 0 : 1)} ${units[unitIndex]}`;
  }

  function clearObjectUrls() {
    if (state.activeUrl) URL.revokeObjectURL(state.activeUrl);
    if (state.preload && state.preload.url) URL.revokeObjectURL(state.preload.url);
    state.activeUrl = null;
    state.preload = null;
  }

  function takePreloadedUrl(index) {
    if (state.preload && state.preload.index === index) {
      const url = state.preload.url;
      state.preload = null;
      return url;
    }
    return URL.createObjectURL(state.items[index].file);
  }

  function preloadNext(index) {
    if (state.preload && state.preload.url) {
      URL.revokeObjectURL(state.preload.url);
      state.preload = null;
    }
    if (index < 0 || index >= state.items.length) return;
    const url = URL.createObjectURL(state.items[index].file);
    const img = new Image();
    img.src = url;
    state.preload = { index, url, img };
  }

  function showSection(section) {
    el.intro.hidden = section !== 'intro';
    el.workspace.hidden = section !== 'workspace';
    el.done.hidden = section !== 'done';
  }

  function startSession(records, rootHint) {
    const items = finalizeRecords(records);
    if (!items.length) {
      alert('이미지 파일을 찾지 못했습니다. JPG, PNG, WEBP 같은 이미지가 들어 있는 폴더를 골라주세요.');
      return;
    }

    clearObjectUrls();
    state.items = items;
    state.index = 0;
    state.decisions = [];
    state.rootHint = rootHint || '';
    showSection('workspace');
    renderCurrent();
  }

  function renderCurrent() {
    state.animating = false;
    resetCardTransform();

    const total = state.items.length;
    const done = state.decisions.length;
    const deleted = getDeletedItems().length;

    if (state.index >= total) {
      clearObjectUrls();
      showSection('done');
      el.summary.textContent = `총 ${total}개 중 유지 ${getKeepCount()}개, 삭제 예정 ${deleted}개입니다.`;
      return;
    }

    const item = state.items[state.index];
    if (state.activeUrl) URL.revokeObjectURL(state.activeUrl);
    state.activeUrl = takePreloadedUrl(state.index);

    el.image.hidden = false;
    el.fallback.hidden = true;
    el.image.onload = () => {
      el.image.hidden = false;
      el.fallback.hidden = true;
    };
    el.image.onerror = () => {
      el.image.hidden = true;
      el.fallback.hidden = false;
    };
    el.image.src = state.activeUrl;
    el.image.alt = item.name;

    el.status.textContent = `${state.index + 1} / ${total} 분류 중 · 삭제 예정 ${deleted}개입니다.`;
    el.fileName.textContent = `${item.relativePath} · ${formatBytes(item.size)}`;
    el.progressBar.style.width = `${Math.round((done / total) * 100)}%`;
    el.undo.disabled = state.decisions.length === 0;

    preloadNext(state.index + 1);
  }

  function decide(action) {
    if (state.index >= state.items.length) return;
    const item = state.items[state.index];
    state.decisions.push({ action, item });
    state.index += 1;
    renderCurrent();
  }

  function undo() {
    if (!state.decisions.length) return;
    state.decisions.pop();
    state.index = Math.max(0, state.index - 1);
    renderCurrent();
  }

  function resetAll() {
    clearObjectUrls();
    state.items = [];
    state.index = 0;
    state.decisions = [];
    state.rootHint = '';
    state.animating = false;
    el.folderInput.value = '';
    el.fileInput.value = '';
    showSection('intro');
  }

  function resetCardTransform() {
    el.card.style.transition = '';
    el.card.style.transform = '';
    el.card.classList.remove('pc-show-keep', 'pc-show-delete');
  }

  function animateDecision(action) {
    if (state.animating || state.index >= state.items.length) return;
    state.animating = true;
    const dir = action === 'delete' ? 1 : -1;
    el.card.style.transition = 'transform 140ms ease, opacity 140ms ease';
    el.card.style.transform = `translateX(${dir * 120}%) rotate(${dir * 8}deg)`;
    setTimeout(() => decide(action), 110);
  }

  function recordsFromFolderInput(fileList) {
    const files = Array.from(fileList || []).filter(isImageFile);
    if (!files.length) return [];
    return files.map((file) => {
      const rel = file.webkitRelativePath ? stripFirstSegment(file.webkitRelativePath) : file.name;
      return makeRecord(file, rel);
    });
  }

  function recordsFromFileInput(fileList) {
    return Array.from(fileList || []).filter(isImageFile).map((file) => makeRecord(file, file.name));
  }

  async function recordsFromDataTransfer(dataTransfer) {
    const items = Array.from(dataTransfer.items || []);
    const entries = items
      .map((item) => (typeof item.webkitGetAsEntry === 'function' ? item.webkitGetAsEntry() : null))
      .filter(Boolean);

    if (entries.length) {
      const records = [];
      for (const entry of entries) {
        await traverseEntry(entry, '', records);
      }

      if (entries.length === 1 && entries[0].isDirectory) {
        const top = `${entries[0].name}/`;
        records.forEach((record) => {
          if (record.relativePath.startsWith(top)) {
            record.relativePath = normalizePath(record.relativePath.slice(top.length));
          }
        });
      }

      return records;
    }

    return recordsFromFileInput(dataTransfer.files);
  }

  function readEntries(directoryEntry) {
    const reader = directoryEntry.createReader();
    const result = [];

    return new Promise((resolve) => {
      const readBatch = () => {
        reader.readEntries((batch) => {
          if (!batch.length) {
            resolve(result);
            return;
          }
          result.push(...batch);
          readBatch();
        }, () => resolve(result));
      };
      readBatch();
    });
  }

  async function traverseEntry(entry, currentPath, output) {
    if (entry.isFile) {
      await new Promise((resolve) => {
        entry.file((file) => {
          if (isImageFile(file)) {
            const rel = currentPath ? `${currentPath}/${file.name}` : file.name;
            output.push(makeRecord(file, rel));
          }
          resolve();
        }, () => resolve());
      });
      return;
    }

    if (entry.isDirectory) {
      const nextPath = currentPath ? `${currentPath}/${entry.name}` : entry.name;
      const children = await readEntries(entry);
      for (const child of children) {
        await traverseEntry(child, nextPath, output);
      }
    }
  }

  function downloadBlob(filename, content, type) {
    const blob = new Blob([content], { type: type || 'text/plain;charset=utf-8' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = filename;
    document.body.appendChild(a);
    a.click();
    a.remove();
    setTimeout(() => URL.revokeObjectURL(url), 500);
  }

  function toBase64Utf8(text) {
    const bytes = new TextEncoder().encode(text);
    let binary = '';
    for (let i = 0; i < bytes.length; i += 1) {
      binary += String.fromCharCode(bytes[i]);
    }
    return btoa(binary);
  }

  function makeWindowsBat() {
    const deleted = getDeletedItems();
    const payload = {
      version: 1,
      createdAt: new Date().toISOString(),
      rootHint: state.rootHint,
      files: deleted.map((item) => item.relativePath)
    };

    const base64 = toBase64Utf8(JSON.stringify(payload));
    const chunks = base64.match(/.{1,76}/g) || [];
    const echoLines = chunks.map((chunk) => `echo ${chunk}`).join('\r\n');
    const psCommand = "$ErrorActionPreference='Stop'; Add-Type -AssemblyName System.Windows.Forms; Add-Type -AssemblyName Microsoft.VisualBasic; $b64=(Get-Content -Raw -LiteralPath $env:B64FILE) -replace '\\s',''; $json=[Text.Encoding]::UTF8.GetString([Convert]::FromBase64String($b64)); $data=$json | ConvertFrom-Json; $dlg=New-Object System.Windows.Forms.FolderBrowserDialog; $dlg.Description='Select the original image folder used in PHOTO CLEANER'; $dlg.ShowNewFolderButton=$false; if($dlg.ShowDialog() -ne [System.Windows.Forms.DialogResult]::OK){ Write-Host 'Canceled.'; exit 0 }; $root=$dlg.SelectedPath; $deleted=0; $missing=0; foreach($rel in @($data.files)){ $child=($rel -replace '/', [IO.Path]::DirectorySeparatorChar); $path=Join-Path $root $child; if(Test-Path -LiteralPath $path){ [Microsoft.VisualBasic.FileIO.FileSystem]::DeleteFile($path, 'OnlyErrorDialogs', 'SendToRecycleBin'); Write-Host ('TRASH: ' + $path); $deleted++ } else { Write-Host ('MISSING: ' + $path); $missing++ } }; Write-Host ''; Write-Host ('DONE. trashed=' + $deleted + ', missing=' + $missing);";

    return [
      '@echo off',
      'chcp 65001 >nul',
      'setlocal EnableExtensions DisableDelayedExpansion',
      'title PHOTO CLEANER - Move selected files to Recycle Bin',
      'echo PHOTO CLEANER - selected files will be moved to Windows Recycle Bin.',
      'echo When the folder picker opens, select the original image folder.',
      'echo.',
      'set "B64FILE=%TEMP%\\photo-cleaner-%RANDOM%-%RANDOM%.b64"',
      '> "%B64FILE%" (',
      echoLines,
      ')',
      `powershell -NoProfile -ExecutionPolicy Bypass -Command "${psCommand}"`,
      'set "PC_EXIT=%ERRORLEVEL%"',
      'del "%B64FILE%" >nul 2>nul',
      'echo.',
      'if not "%PC_EXIT%"=="0" echo An error occurred. Check the messages above.',
      'pause',
      'exit /b %PC_EXIT%'
    ].join('\r\n');
  }

  function downloadBat() {
    const deleted = getDeletedItems();
    if (!deleted.length) {
      alert('휴지통으로 보낼 파일이 없습니다.');
      return;
    }
    downloadBlob('photo-cleaner-trash.bat', makeWindowsBat(), 'application/octet-stream;charset=utf-8');
  }

  function downloadDeleteList() {
    const deleted = getDeletedItems();
    if (!deleted.length) {
      alert('삭제 예정 목록이 비어 있습니다.');
      return;
    }
    const lines = [
      '# PHOTO CLEANER 삭제 예정 목록입니다.',
      `# 생성 시각: ${new Date().toLocaleString()}`,
      `# 총 ${deleted.length}개입니다.`,
      '',
      ...deleted.map((item) => item.relativePath)
    ];
    downloadBlob('photo-cleaner-delete-list.txt', lines.join('\n'), 'text/plain;charset=utf-8');
  }

  el.pickFolder.addEventListener('click', () => el.folderInput.click());
  el.pickFiles.addEventListener('click', () => el.fileInput.click());
  el.dropZone.addEventListener('click', () => el.folderInput.click());
  el.dropZone.addEventListener('keydown', (event) => {
    if (event.key === 'Enter' || event.key === ' ') {
      event.preventDefault();
      el.folderInput.click();
    }
  });

  el.folderInput.addEventListener('change', () => {
    const files = Array.from(el.folderInput.files || []);
    const firstPath = files[0] && files[0].webkitRelativePath ? files[0].webkitRelativePath : '';
    const rootHint = firstPath.includes('/') ? firstPath.split('/')[0] : '선택한 폴더';
    startSession(recordsFromFolderInput(files), rootHint);
  });

  el.fileInput.addEventListener('change', () => {
    startSession(recordsFromFileInput(el.fileInput.files), '선택한 파일들이 들어 있는 폴더');
  });

  ['dragenter', 'dragover'].forEach((type) => {
    el.dropZone.addEventListener(type, (event) => {
      event.preventDefault();
      event.stopPropagation();
      el.dropZone.classList.add('pc-dragging');
    });
  });

  ['dragleave', 'drop'].forEach((type) => {
    el.dropZone.addEventListener(type, (event) => {
      event.preventDefault();
      event.stopPropagation();
      el.dropZone.classList.remove('pc-dragging');
    });
  });

  el.dropZone.addEventListener('drop', async (event) => {
    const records = await recordsFromDataTransfer(event.dataTransfer);
    startSession(records, '드롭한 폴더 또는 파일들이 들어 있는 폴더');
  });

  el.keep.addEventListener('click', () => animateDecision('keep'));
  el.delete.addEventListener('click', () => animateDecision('delete'));
  el.undo.addEventListener('click', undo);
  el.reset.addEventListener('click', resetAll);
  el.restart.addEventListener('click', resetAll);
  el.downloadBat.addEventListener('click', downloadBat);
  el.downloadList.addEventListener('click', downloadDeleteList);

  el.card.addEventListener('pointerdown', (event) => {
    if (state.index >= state.items.length) return;
    state.gesture = {
      pointerId: event.pointerId,
      startX: event.clientX,
      startY: event.clientY,
      dx: 0,
      dy: 0
    };
    el.card.setPointerCapture(event.pointerId);
    el.card.style.transition = 'none';
  });

  el.card.addEventListener('pointermove', (event) => {
    if (!state.gesture || state.gesture.pointerId !== event.pointerId) return;
    const dx = event.clientX - state.gesture.startX;
    const dy = event.clientY - state.gesture.startY;
    state.gesture.dx = dx;
    state.gesture.dy = dy;

    if (Math.abs(dx) < 8 && Math.abs(dy) > Math.abs(dx)) return;

    const rotate = Math.max(-9, Math.min(9, dx / 24));
    el.card.style.transform = `translateX(${dx}px) rotate(${rotate}deg)`;
    el.card.classList.toggle('pc-show-delete', dx > 45);
    el.card.classList.toggle('pc-show-keep', dx < -45);
  });

  function finishGesture(event) {
    if (!state.gesture || state.gesture.pointerId !== event.pointerId) return;
    const dx = state.gesture.dx;
    const threshold = Math.min(150, Math.max(72, el.card.clientWidth * 0.16));
    state.gesture = null;

    if (Math.abs(dx) >= threshold) {
      animateDecision(dx > 0 ? 'delete' : 'keep');
      return;
    }

    el.card.style.transition = 'transform 140ms ease';
    resetCardTransform();
  }

  el.card.addEventListener('pointerup', finishGesture);
  el.card.addEventListener('pointercancel', finishGesture);
  el.card.addEventListener('lostpointercapture', (event) => {
    if (state.gesture && state.gesture.pointerId === event.pointerId) {
      state.gesture = null;
      resetCardTransform();
    }
  });

  document.addEventListener('keydown', (event) => {
    if (el.workspace.hidden) return;
    if (event.key === 'ArrowLeft') {
      event.preventDefault();
      animateDecision('keep');
    } else if (event.key === 'ArrowRight') {
      event.preventDefault();
      animateDecision('delete');
    } else if (event.key === 'Backspace' || event.key.toLowerCase() === 'z') {
      event.preventDefault();
      undo();
    }
  });

  showSection('intro');
})();
</script>
{% endraw %}
