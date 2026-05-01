---
layout: page
title: PHOTO CLEANER
icon: fas fa-fw fa-images
order: 6
---

{% raw %}
<style>
    /* 테마 스타일과 충돌을 피하기 위해 더 구체적인 선택자 사용 및 !important 추가 */
    body .post-content #drop-zone { border: 3px dashed #bbb; border-radius: 10px; padding: 50px; text-align: center; cursor: pointer; margin-bottom: 20px; transition: 0.3s; color: #888; }
    body .post-content #drop-zone:hover { background-color: rgba(0, 102, 204, 0.1); border-color: #0066cc; color: #fff; }
    body .post-content #workspace { display: none; text-align: center; }
    /* 이미지가 클릭되지 않게 하고(pointer-events: none) 테마의 이미지 스타일을 우회 */
    body .post-content #image-container img { max-width: 80% !important; max-height: 500px !important; border-radius: 8px !important; box-shadow: 0 4px 8px rgba(0,0,0,0.3) !important; pointer-events: none !important; margin: 0 auto; display: block; }
    body .post-content .controls { margin-top: 20px; font-size: 1.1rem; color: #fff; }
    body .post-content .btn { padding: 10px 20px; font-size: 1rem; cursor: pointer; background-color: #0066cc; color: white; border: none; border-radius: 5px; margin-top: 15px; }
    body .post-content .btn:hover { background-color: #0055aa; }
</style>

<div id="drop-zone">이곳에 정리할 이미지 파일들을 드래그 앤 드롭해주세요.</div>

<div id="workspace">
    <div id="image-container"></div>
    <div class="controls">
        <p>⌨️ <strong>왼쪽(←)</strong>: 유지 &nbsp;|&nbsp; <strong>오른쪽(→)</strong>: 삭제(휴지통)</p>
        <p id="status-text"></p>
    </div>
    <button id="download-btn" class="btn" style="display:none;">배치(.bat) 파일 다운로드</button>
</div>

<script>
    let files = [];
    let currentIndex = 0;
    let deleteList = [];

    const dropZone = document.getElementById('drop-zone');
    const workspace = document.getElementById('workspace');
    const imageContainer = document.getElementById('image-container');
    const downloadBtn = document.getElementById('download-btn');
    const statusText = document.getElementById('status-text');

    dropZone.addEventListener('dragover', (e) => { e.preventDefault(); dropZone.style.borderColor = '#0066cc'; });
    dropZone.addEventListener('dragleave', (e) => { e.preventDefault(); dropZone.style.borderColor = '#bbb'; });
    dropZone.addEventListener('drop', (e) => {
        e.preventDefault();
        files = Array.from(e.dataTransfer.files).filter(f => f.type.startsWith('image/'));
        if (files.length > 0) {
            dropZone.style.display = 'none';
            workspace.style.display = 'block';
            showImage(currentIndex);
        } else {
            alert('이미지 파일만 인식됩니다.');
        }
    });

    function showImage(index) {
        if (index >= files.length) {
            imageContainer.innerHTML = '<h3 style="color: #fff;">분류 완료! 아래 버튼을 눌러 스크립트를 다운로드하세요.</h3>';
            statusText.innerText = `총 ${files.length}개 중 ${deleteList.length}개 삭제 예정`;
            statusText.style.color = '#fff';
            downloadBtn.style.display = 'inline-block';
            return;
        }
        const file = files[index];
        const url = URL.createObjectURL(file);
        // pointer-events: none과 data-no-lightgallery, class="no-lazyload" 속성 추가
        imageContainer.innerHTML = `<img src="${url}" alt="${file.name}" style="pointer-events: none;" data-no-lightgallery="true" class="no-lazyload">`;
        statusText.innerText = `현재 진행: ${index + 1} / ${files.length} (${file.name})`;
        statusText.style.color = '#fff';
    }

    document.addEventListener('keydown', (e) => {
        if (currentIndex >= files.length) return;
        if (e.key === 'ArrowLeft') {
            currentIndex++;
            showImage(currentIndex);
        } else if (e.key === 'ArrowRight') {
            deleteList.push(files[currentIndex].name);
            currentIndex++;
            showImage(currentIndex);
        }
    });

    downloadBtn.addEventListener('click', () => {
        if (deleteList.length === 0) {
            alert('휴지통으로 보낼 파일이 없습니다.');
            return;
        }
        const fileNames = deleteList.map(name => `"${name}"`).join(" ");
        const batContent = `@echo off\nchcp 65001 >nul\nset files=${fileNames}\nfor %%f in (%files%) do (\n    powershell -Command "Add-Type -AssemblyName Microsoft.VisualBasic; [Microsoft.VisualBasic.FileIO.FileSystem]::DeleteFile('%%~f', 'OnlyErrorDialogs', 'SendToRecycleBin')"\n)\necho.\necho 선택한 파일들이 휴지통으로 이동되었습니다.\npause`;
        const blob = new Blob([batContent], { type: 'text/plain;charset=utf-8' });
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a');
        a.href = url;
        a.download = 'cleanup.bat';
        a.click();
    });
</script>
{% endraw %}
