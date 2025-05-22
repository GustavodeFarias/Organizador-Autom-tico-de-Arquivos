<!DOCTYPE html>
<html lang="pt-BR" >
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Organizador Automático de Arquivos</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@700&display=swap');

    /* Reset */
    *, *::before, *::after {
      box-sizing: border-box;
    }
    body, html {
      margin: 0; padding: 0; height: 100%;
      background: #0a0a0f;
      overflow-x: hidden;
      font-family: 'Poppins', sans-serif;
      color: #EEE;
      user-select: none;
    }

    /* Background animated blobs */
    body::before, body::after {
      content: "";
      position: fixed;
      border-radius: 50%;
      opacity: 0.4;
      filter: blur(120px);
      mix-blend-mode: screen;
      animation-timing-function: ease-in-out;
      animation-iteration-count: infinite;
      animation-direction: alternate;
      z-index: 0;
      pointer-events: none;
    }
    body::before {
      width: 500px;
      height: 500px;
      left: -150px;
      top: -150px;
      background: linear-gradient(45deg,#6e8efb,#a777e3);
      animation-name: float1;
      animation-duration: 12000ms;
    }
    body::after {
      width: 600px;
      height: 600px;
      right: -200px;
      bottom: -200px;
      background: linear-gradient(45deg,#ff6f61,#f77b23);
      animation-name: float2;
      animation-duration: 14000ms;
    }
    @keyframes float1 {
      0% { transform: translate(0,0) rotate(0deg); }
      100% { transform: translate(30px,40px) rotate(15deg); }
    }
    @keyframes float2 {
      0% { transform: translate(0,0) rotate(0deg); }
      100% { transform: translate(-40px,-30px) rotate(-20deg); }
    }

    /* Container for all content */
    .container {
      position: relative;
      z-index: 10;
      max-width: 960px;
      margin: 0 auto;
      padding: 40px 20px 80px;
      display: flex;
      flex-direction: column;
      min-height: 100vh;
    }

    /* Header */
    header {
      text-align: center;
      margin-bottom: 48px;
      color: #cfcde1;
      text-shadow:
        0 0 3px #4a409d,
        0 0 6px #3f3180,
        0 0 9px #6e8efbc9;
      user-select: text;
    }
    header h1 {
      font-size: 3.8rem;
      letter-spacing: 0.12em;
      font-weight: 700;
      line-height: 1.1;
      filter: drop-shadow(0 0 5px #4a3db9bb);
      background: linear-gradient(90deg, #4a3db9, #6e7ee0, #4a3db9);
      background-size: 200% auto;
      background-clip: text;
      -webkit-background-clip: text;
      -webkit-text-fill-color: transparent;
      animation: shimmer 5s linear infinite;
      margin: 0;
    }
    @keyframes shimmer {
      0% {
        background-position: -200% 0;
      }
      100% {
        background-position: 200% 0;
      }
    }

    /* Instructions */
    .instructions {
      margin-bottom: 30px;
      font-weight: 600;
      font-size: 1.3rem;
      text-align: center;
      color: #d9d9f7;
      text-shadow: 0 0 5px #4a4a85bb;
      user-select: none;
    }

    /* Select files button */
    label.file-select-btn {
      display: block;
      cursor: pointer;
      background:
        linear-gradient(145deg, #7739f9, #972ff3);
      box-shadow:
        0 0 18px #7739f9,
        0 0 24px #972ff3;
      border-radius: 50px;
      padding: 18px 48px;
      color: white;
      font-weight: 700;
      font-size: 1.3rem;
      text-align: center;
      margin: 0 auto 46px;
      width: max-content;
      text-transform: uppercase;
      transition: background 0.4s ease, box-shadow 0.4s ease;
      user-select: none;
      filter: drop-shadow(0 0 12px #6e8efbCC);
      outline-offset: 6px;
      outline-color: transparent;
    }
    label.file-select-btn:hover,
    label.file-select-btn:focus-visible {
      background: linear-gradient(145deg, #972ff3, #9f1dff);
      box-shadow:
        0 0 28px #a050ff,
        0 0 40px #d535ff;
      outline-color: #d535ff;
      outline-style: solid;
      outline-width: 3px;
    }
    input[type="file"] {
      display: none;
    }

    /* Drop Zone */
    .drop-zone {
      border: 4px solid #7739f9;
      border-radius: 28px;
      padding: 60px 0;
      max-width: 480px;
      margin: 0 auto 60px;
      text-align: center;
      font-size: 1.4rem;
      font-weight: 700;
      color: #6c55bc;
      cursor: pointer;
      position: relative;
      box-shadow: 0 0 26px 3px #972ff3cc;
      transition:
        border-color 0.4s ease,
        box-shadow 0.4s ease,
        transform 0.4s ease;
      user-select: none;
    }
    .drop-zone:focus-visible {
      outline: 0;
      border-color: #d535ff;
      box-shadow: 0 0 40px 6px #d535ffcc;
      transform: scale(1.04);
      z-index: 20;
    }
    .drop-zone svg {
      width: 96px;
      height: 96px;
      margin-bottom: 18px;
      fill: #8c73e4;
      filter: drop-shadow(0 0 14px #8c73e4cc);
      animation: pulse 2.2s ease-in-out infinite;
    }
    .drop-zone.dragover {
      border-color: #d535ff;
      box-shadow: 0 0 38px 9px #d535ffcc;
      color: #ff3af7;
      transform: scale(1.1);
    }
    @keyframes pulse {
      0%, 100% {
        transform: scale(1);
        filter: drop-shadow(0 0 14px #8c73e4cc);
      }
      50% {
        transform: scale(1.1);
        filter: drop-shadow(0 0 28px #ff3af7cc);
      }
    }

    /* Organized section with glass */
    .organized {
      max-width: 900px;
      margin: 0 auto;
      user-select: none;
      min-height: 240px;
      position: relative;
      z-index: 10;
      padding-bottom: 120px;
    }

    /* Folder groups */
    .folder-group {
      background: rgba(50 15 80 / 0.45);
      border-radius: 22px;
      backdrop-filter: saturate(180%) blur(18px);
      border: 2px solid rgba(255 255 255 / 0.15);
      padding: 24px 36px 36px;
      margin-bottom: 48px;
      box-shadow:
        0 0 15px #7e6bff88,
        inset 0 0 50px #9e7bff88;
      transform-style: preserve-3d;
      perspective: 700px;
      transition: box-shadow 0.6s ease;
      cursor: default;
      overflow: visible;
    }
    .folder-group:hover {
      box-shadow:
        0 0 42px 8px #ad67ffcc,
        inset 0 0 72px #d63bffcc;
      transform: translateZ(8px) rotateX(3deg);
    }

    /* Folder titles */
    .folder-title {
      font-size: 2.3rem;
      color: #916fcecc;
      font-weight: 700;
      font-variant: small-caps;
      display: flex;
      align-items: center;
      gap: 26px;
      user-select: text;
      text-shadow:
        0 0 4px #7f6ec9bb,
        0 0 8px #a78efdcc;
    }
    .folder-title svg {
      width: 42px;
      height: 42px;
      filter: drop-shadow(0 0 6px #a570ffcc);
      flex-shrink: 0;
    }

    /* File list grid */
    .file-list {
      display: flex;
      flex-wrap: wrap;
      gap: 18px;
      margin-top: 18px;
    }

    /* File cards */
    .file-item {
      background: rgba(110 142 251 / 0.25);
      border-radius: 14px;
      padding: 16px 22px;
      min-width: 180px;
      flex: 1 1 auto;
      font-weight: 700;
      font-size: 1rem;
      color: #eee;
      display: flex;
      align-items: center;
      gap: 16px;
      user-select: all;
      cursor: default;
      box-shadow:
        0 0 10px #6e8efbbb,
        inset 0 2px 4px rgba(255 255 255 / 0.22);
      text-shadow:
        0 0 3px #4a64b1;
      position: relative;
      perspective: 1100px;
      transition:
        transform 0.25s ease, box-shadow 0.35s ease,
        background 0.35s ease, color 0.22s ease;
    }
    .file-item::before {
      content: "";
      position: absolute;
      top: 0; left: 0; right: 0; bottom: 0;
      border-radius: 14px;
      box-shadow: 0 0 20px #7f65e0aa;
      opacity: 0.4;
      pointer-events: none;
      transition: opacity 0.3s ease;
      z-index: -1;
    }
    .file-item:hover {
      background: #8452ffcc;
      color: white;
      box-shadow:
        0 0 28px #bb7eff,
        inset 0 0 20px #c36eff;
      transform: translateZ(42px) scale(1.03);
    }
    .file-item:hover::before {
      opacity: 0.8;
    }
    .file-icon {
      width: 32px;
      height: 32px;
      fill: #cbb2ff;
      filter: drop-shadow(0 0 5px #cbb2ffbb);
      flex-shrink: 0;
      transition: fill 0.35s ease;
    }
    .file-item:hover .file-icon {
      fill: #e0d2ff;
      filter: drop-shadow(0 0 15px #e0d2ffcc);
    }

    /* Download button */
    #downloadBtn {
      position: fixed;
      bottom: 34px;
      right: 32px;
      background: #ff53e4;
      border: none;
      padding: 18px 44px;
      border-radius: 48px;
      font-weight: 800;
      color: white;
      font-size: 1.3rem;
      cursor: pointer;
      box-shadow:
        0 0 35px #ff66e9,
        0 15px 40px #ff66e999;
      transition:
        background 0.3s ease,
        box-shadow 0.3s ease,
        transform 0.35s ease;
      user-select: none;
      z-index: 20;
      outline-offset: 6px;
      outline-color: transparent;
    }
    #downloadBtn:hover,
    #downloadBtn:focus-visible {
      background: #d529ba;
      box-shadow:
        0 0 45px #ff46cc,
        0 20px 50px #ff46ccbb;
      transform: scale(1.06);
      outline-color: #ff56db;
      outline-style: solid;
      outline-width: 4px;
    }

    #downloadBtn:disabled {
      background: #83396e77;
      box-shadow: none;
      cursor: not-allowed;
      transform: none;
      outline: none;
    }

    /* Responsive */
    @media (max-width: 720px) {
      header h1 {
        font-size: 2.9rem;
      }
      .instructions {
        font-size: 1.1rem;
      }
      .drop-zone {
        max-width: 100%;
        padding: 54px 30px;
        font-size: 1.2rem;
      }
      .drop-zone svg {
        width: 72px;
        height: 72px;
        margin-bottom: 14px;
      }
      .folder-title {
        font-size: 1.8rem;
        gap: 18px;
      }
      .file-item {
        min-width: 140px;
        font-size: 0.92rem;
        padding: 14px 18px;
      }
      #downloadBtn {
        right: 16px;
        bottom: 28px;
        padding: 14px 32px;
        font-size: 1.1rem;
      }
    }
  </style>
</head>
<body>
  <div class="container" role="main">
    <header>
      <h1 tabindex="0">Organizador Automático de Arquivos</h1>
    </header>

    <p class="instructions" aria-live="polite" aria-atomic="true">
      Arraste e solte seus arquivos aqui ou clique no botão para selecionar os arquivos que deseja organizar automaticamente por tipo.
    </p>

    <label for="fileSelector" class="file-select-btn" tabindex="0" role="button" aria-label="Selecionar arquivos para organizar">Selecionar Arquivos</label>
    <input type="file" id="fileSelector" multiple webkitdirectory directory aria-label="Input de seleção de arquivos" />

    <div id="dropZone" class="drop-zone" tabindex="0" role="region" aria-label="Área para arrastar e soltar arquivos">
      <svg xmlns="http://www.w3.org/2000/svg" aria-hidden="true" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 15v4a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2v-4"/><polyline points="7 10 12 15 17 10" /><line x1="12" y1="15" x2="12" y2="3"/></svg>
      Arraste os arquivos para cá
    </div>

    <section id="organizedFiles" class="organized" aria-live="polite" aria-atomic="true" tabindex="0"></section>

    <button id="downloadBtn" class="btn" style="display:none;" aria-label="Baixar pastas zipadas por tipo">Baixar Pastas Zipadas por Tipo</button>
  </div>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js"></script>
  <script>
    (() => {
      const fileSelector = document.getElementById('fileSelector');
      const dropZone = document.getElementById('dropZone');
      const organizedFilesContainer = document.getElementById('organizedFiles');
      const downloadBtn = document.getElementById('downloadBtn');

      const categories = {
        'PDFs': {
          exts: ['pdf'],
          icon: `<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M6 2h9l5 5v15a2 2 0 0 1-2 2h-12a2 2 0 0 1-2-2v-18a2 2 0 0 1 2-2zm7 7h4l-5-5v4a1 1 0 0 0 1 1zm-3 6v-1h1v-2h-3v1h1v2h-1v1h2z"/></svg>`
        },
        'Imagens': {
          exts: ['jpg','jpeg','png','gif','bmp','svg','webp','tiff'],
          icon: `<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M21 19v-14a2 2 0 0 0-2-2h-14a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2zm-14-4l3.5 4.5 2.5-3.01 3.5 4.5h-11z"/></svg>`
        },
        'Vídeos': {
          exts: ['mp4','avi','mkv','mov','wmv','flv','webm','mpeg'],
          icon: `<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M17 10.5v-5a2 2 0 0 0-2-2h-6a2 2 0 0 0-2 2v10a2 2 0 0 0 2 2h6a2 2 0 0 0 2-2v-5l4 4v-8l-4 4z"/></svg>`
        },
        'Planilhas': {
          exts: ['xls', 'xlsx', 'csv', 'ods'],
          icon: `<svg viewBox="0 0 24 24" aria-hidden="true"><path d="M19 2h-14a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-16a2 2 0 0 0-2-2zm-7 18h-3v-8h3v8zm1-10h-4v-2h4v2zm5 10h-3v-5h3v5zm0-7h-3v-2h3v2z"/></svg>`
        },
        'Outros': {
          exts: [],
          icon: `<svg viewBox="0 0 24 24" aria-hidden="true"><circle cx="12" cy="12" r="10" stroke="#6e8efb" stroke-width="2" fill="none"/><path d="M7 12h10M12 7v10" stroke="#6e8efb" stroke-width="2" stroke-linecap="round"/></svg>`
        }
      };

      function getCategory(filename) {
        const ext = filename.split('.').pop().toLowerCase();
        for (const [cat, data] of Object.entries(categories)) {
          if (data.exts.includes(ext)) {
            return cat;
          }
        }
        return 'Outros';
      }

      function renderOrganizedFiles(filesGrouped) {
        organizedFilesContainer.innerHTML = '';
        let anyFiles = false;

        for (const [category, files] of Object.entries(filesGrouped)) {
          if(files.length === 0) continue;
          anyFiles = true;
          const folderGroup = document.createElement('div');
          folderGroup.className = 'folder-group';

          const folderTitle = document.createElement('h2');
          folderTitle.className = 'folder-title';
          folderTitle.innerHTML = `${categories[category].icon} ${category} (${files.length})`;
          folderGroup.appendChild(folderTitle);

          const fileList = document.createElement('div');
          fileList.className = 'file-list';

          for (const file of files) {
            const fileItem = document.createElement('div');
            fileItem.className = 'file-item';
            fileItem.title = file.name;

            let fileIcon = '';
            switch(category) {
              case 'PDFs':
                fileIcon = `<svg class="file-icon" viewBox="0 0 24 24" aria-hidden="true"><path d="M6 2h9l5 5v15a2 2 0 0 1-2 2h-12a2 2 0 0 1-2-2v-18a2 2 0 0 1 2-2zm7 7h4l-5-5v4a1 1 0 0 0 1 1zm-3 6v-1h1v-2h-3v1h1v2h-1v1h2z"/></svg>`;
                break;
              case 'Imagens':
                fileIcon = `<svg class="file-icon" viewBox="0 0 24 24" aria-hidden="true"><path d="M21 19v-14a2 2 0 0 0-2-2h-14a2 2 0 0 0-2 2v14a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2zm-14-4l3.5 4.5 2.5-3.01 3.5 4.5h-11z"/></svg>`;
                break;
              case 'Vídeos':
                fileIcon = `<svg class="file-icon" viewBox="0 0 24 24" aria-hidden="true"><path d="M17 10.5v-5a2 2 0 0 0-2-2h-6a2 2 0 0 0-2 2v10a2 2 0 0 0 2 2h6a2 2 0 0 0 2-2v-5l4 4v-8l-4 4z"/></svg>`;
                break;
              case 'Planilhas':
                fileIcon = `<svg class="file-icon" viewBox="0 0 24 24" aria-hidden="true"><path d="M19 2h-14a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h14a2 2 0 0 0 2-2v-16a2 2 0 0 0-2-2zm-7 18h-3v-8h3v8zm1-10h-4v-2h4v2zm5 10h-3v-5h3v5zm0-7h-3v-2h3v2z"/></svg>`;
                break;
              default:
                fileIcon = `<svg class="file-icon" viewBox="0 0 24 24" aria-hidden="true"><circle cx="12" cy="12" r="10" stroke="#6e8efb" stroke-width="2" fill="none"/><path d="M7 12h10M12 7v10" stroke="#6e8efb" stroke-width="2" stroke-linecap="round"/></svg>`;
            }

            fileItem.innerHTML = `${fileIcon} ${file.name}`;
            fileList.appendChild(fileItem);
          }

          folderGroup.appendChild(fileList);
          organizedFilesContainer.appendChild(folderGroup);
        }

        if(!anyFiles) {
          organizedFilesContainer.innerHTML = '<p style="color:#bbb; font-style: italic; user-select:none;">Nenhum arquivo organizado ainda.</p>';
          downloadBtn.style.display = 'none';
        } else {
          downloadBtn.style.display = 'block';
        }
      }

      function organizeFiles(files) {
        const grouped = {};
        for (const cat of Object.keys(categories)) {
          grouped[cat] = [];
        }
        for (const file of files) {
          grouped[getCategory(file.name)].push(file);
        }
        return grouped;
      }

      function handleFiles(selectedFiles) {
        if (!selectedFiles.length) {
          organizedFilesContainer.innerHTML = '';
          downloadBtn.style.display = 'none';
          return;
        }
        const filesArr = Array.from(selectedFiles);
        const grouped = organizeFiles(filesArr);
        renderOrganizedFiles(grouped);
        currentGroupedFiles = grouped;
      }

      dropZone.addEventListener('dragover', e => {
        e.preventDefault();
        dropZone.classList.add('dragover');
      });

      dropZone.addEventListener('dragleave', e => {
        e.preventDefault();
        dropZone.classList.remove('dragover');
      });

      dropZone.addEventListener('drop', e => {
        e.preventDefault();
        dropZone.classList.remove('dragover');
        if (e.dataTransfer.files.length > 0) {
          handleFiles(e.dataTransfer.files);
        }
      });

      fileSelector.addEventListener('change', e => {
        handleFiles(e.target.files);
      });

      let currentGroupedFiles = {};

      downloadBtn.addEventListener('click', async () => {
        if (Object.keys(currentGroupedFiles).length === 0) return;
        downloadBtn.disabled = true;
        downloadBtn.textContent = 'Preparando download...';

        const zip = new JSZip();

        const toArrayBuffer = file => new Response(file).arrayBuffer();

        for (const [category, files] of Object.entries(currentGroupedFiles)) {
          if (files.length === 0) continue;
          const folder = zip.folder(category);
          for (const file of files) {
            try {
              const arrayBuffer = await toArrayBuffer(file);
              folder.file(file.name, arrayBuffer);
            } catch(err) {
              console.error('Erro ao adicionar arquivo ao zip:', file.name, err);
            }
          }
        }

        try {
          const content = await zip.generateAsync({type:"blob"});
          const url = URL.createObjectURL(content);
          const a = document.createElement('a');
          a.href = url;
          a.download = 'arquivos_organizados.zip';
          document.body.appendChild(a);
          a.click();
          a.remove();
          URL.revokeObjectURL(url);
        } catch(err) {
          alert('Erro ao gerar arquivo zip.');
        }
        downloadBtn.disabled = false;
        downloadBtn.textContent = 'Baixar Pastas Zipadas por Tipo';
      });

      dropZone.addEventListener('keydown', e => {
        if (e.key === 'Enter' || e.key === ' ') {
          e.preventDefault();
          fileSelector.click();
        }
      });

      setInterval(() => {
        if (Object.keys(currentGroupedFiles).length) {
          renderOrganizedFiles(currentGroupedFiles);
        }
      }, 60000);

    })();
  </script>
</body>
</html>

