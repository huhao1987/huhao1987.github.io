---
layout: default
title: Hao Android Lab
---

<section class="showcase-shell">
  <header class="topbar">
    <div class="brand-copy">
      <p class="eyebrow" id="txtEyebrow">Curated by huhao1987</p>
      <h1 id="txtTitle">Android Project Market</h1>
    </div>
    <div class="topbar-actions">
      <label class="lang-wrap" for="langSelect">
        <span id="txtLangLabel">Language</span>
        <select id="langSelect" class="lang-select">
          <option value="en">English</option>
          <option value="zh">中文</option>
        </select>
      </label>
      <a class="btn btn-solid" id="txtGithubBtn" href="https://github.com/huhao1987" target="_blank" rel="noopener noreferrer">Visit GitHub</a>
    </div>
  </header>

  <section class="hero-band">
    <div class="hero-chip" id="txtFeatureChip">Featured Collection</div>
    <p id="syncNote">Sync: initializing...</p>
    <p id="projectCount">8 projects</p>
  </section>

  <div class="market-layout">
    <aside class="catalog-panel" aria-label="project list">
      <div class="catalog-tools">
        <input id="repoSearch" type="search" placeholder="Search project name or keyword" />
        <div class="filters">
          <button class="filter is-active" data-filter="all" type="button" id="txtFilterAll">All</button>
          <button class="filter" data-filter="released" type="button" id="txtFilterReleased">Released</button>
          <button class="filter" data-filter="developing" type="button" id="txtFilterDeveloping">Developing</button>
        </div>
      </div>
      <ul id="appGrid" class="catalog-grid"></ul>
    </aside>

    <section class="detail-panel" id="detailPanel" aria-label="project details">
      <div class="detail-hero">
        <img id="detailHero" src="" alt="project hero" loading="lazy" />
        <div class="detail-gradient"></div>
        <div class="detail-hero-info">
          <div class="detail-icon" id="detailIcon">NX</div>
          <div>
            <p class="status" id="detailStatus">Released</p>
            <h2 id="detailName">NXloaderRB</h2>
            <p id="detailSubtitle">Nintendo Switch payload loader on Android</p>
          </div>
        </div>
      </div>

      <div class="detail-body">
        <div class="metric-row">
          <div class="metric-card">
            <span id="txtStars">Stars</span>
            <strong id="detailStars">-</strong>
          </div>
          <div class="metric-card">
            <span id="txtLanguage">Language</span>
            <strong id="detailLang">-</strong>
          </div>
        </div>

        <p class="detail-summary" id="detailSummary"></p>

        <div class="detail-actions">
          <a id="detailUrl" href="#" target="_blank" rel="noopener noreferrer" class="btn btn-solid">Open Repository</a>
          <button id="openSheet" type="button" class="btn btn-ghost">Full Preview</button>
        </div>

        <section class="detail-section">
          <h3 id="txtHighlights">Highlights</h3>
          <ul id="detailHighlights" class="highlights"></ul>
        </section>

        <section class="detail-section">
          <h3 id="txtShots">Shots</h3>
          <div id="previewStrip" class="shot-grid"></div>
        </section>
      </div>
    </section>
  </div>
</section>

<div class="sheet" id="sheet" aria-hidden="true">
  <div class="sheet-mask" id="sheetMask"></div>
  <section class="sheet-panel" role="dialog" aria-modal="true" aria-label="project preview detail">
    <button class="sheet-close" id="sheetClose" type="button" aria-label="Close">×</button>
    <div class="sheet-head">
      <div class="sheet-icon" id="sheetIcon">NX</div>
      <div>
        <p class="sheet-status" id="sheetStatus">Released</p>
        <h4 id="sheetName">NXloaderRB</h4>
      </div>
    </div>
    <div id="sheetGallery" class="sheet-gallery"></div>
  </section>
</div>

<script>
  const repoPreview = (repo) => `https://opengraph.githubassets.com/1/${repo}`;

  const i18n = {
    en: {
      title: "Android Project Market",
      github: "Visit GitHub",
      feature: "Featured Collection",
      languageLabel: "Language",
      searchPlaceholder: "Search project name or keyword",
      filterAll: "All",
      filterReleased: "Released",
      filterDeveloping: "Developing",
      stars: "Stars",
      language: "Language",
      openRepo: "Open Repository",
      repoSoon: "Repository Coming Soon",
      fullPreview: "Full Preview",
      highlights: "Highlights",
      shots: "Shots",
      statusReleased: "Released",
      statusDeveloping: "Developing",
      noMatch: "No matching projects",
      projectsSuffix: "projects",
      syncInit: "Sync: initializing...",
      syncLoading: "Sync: loading latest stars from GitHub...",
      syncDone: "Sync:",
      sheetStatus: "Released"
    },
    zh: {
      title: "安卓项目市场",
      github: "访问 GitHub",
      feature: "精选项目集",
      languageLabel: "语言",
      searchPlaceholder: "搜索项目名或关键词",
      filterAll: "全部",
      filterReleased: "已发布",
      filterDeveloping: "开发中",
      stars: "星标",
      language: "语言",
      openRepo: "打开仓库",
      repoSoon: "仓库待公开",
      fullPreview: "全屏预览",
      highlights: "亮点",
      shots: "截图",
      statusReleased: "已发布",
      statusDeveloping: "开发中",
      noMatch: "没有匹配项目",
      projectsSuffix: "个项目",
      syncInit: "同步状态：初始化中...",
      syncLoading: "同步状态：正在从 GitHub 获取最新星数...",
      syncDone: "同步状态：",
      sheetStatus: "已发布"
    }
  };

  const iconSvg = (label, c1, c2) => {
    const safe = label.slice(0, 2).toUpperCase();
    const svg = `<svg xmlns='http://www.w3.org/2000/svg' width='120' height='120' viewBox='0 0 120 120'>
      <defs><linearGradient id='g' x1='0' y1='0' x2='1' y2='1'><stop offset='0%' stop-color='${c1}'/><stop offset='100%' stop-color='${c2}'/></linearGradient></defs>
      <rect rx='30' ry='30' width='120' height='120' fill='url(#g)'/>
      <text x='60' y='73' text-anchor='middle' font-size='38' font-family='Arial, sans-serif' font-weight='700' fill='white'>${safe}</text>
    </svg>`;
    return `data:image/svg+xml;utf8,${encodeURIComponent(svg)}`;
  };

  const placeholderShot = (title, c1, c2) => {
    const svg = `<svg xmlns='http://www.w3.org/2000/svg' width='1280' height='720' viewBox='0 0 1280 720'>
      <defs><linearGradient id='b' x1='0' y1='0' x2='1' y2='1'><stop offset='0%' stop-color='${c1}'/><stop offset='100%' stop-color='${c2}'/></linearGradient></defs>
      <rect width='1280' height='720' fill='url(#b)'/>
      <circle cx='1110' cy='140' r='180' fill='rgba(255,255,255,0.14)'/>
      <circle cx='170' cy='610' r='240' fill='rgba(255,255,255,0.12)'/>
      <text x='88' y='360' font-size='64' font-family='Arial, sans-serif' font-weight='700' fill='white'>${title}</text>
      <text x='88' y='420' font-size='34' font-family='Arial, sans-serif' fill='rgba(255,255,255,0.9)'>Project Preview</text>
    </svg>`;
    return `data:image/svg+xml;utf8,${encodeURIComponent(svg)}`;
  };

  const projects = [
    {
      id: "nxloaderrb",
      name: "NXloaderRB",
      status: "released",
      subtitle: { en: "Nintendo Switch payload loader on Android", zh: "Android 端 Nintendo Switch payload 注入工具" },
      summary: { en: "A rebuilt version of NXloader for a more stable Android payload injection workflow.", zh: "NXloaderRB 的重构版本，提供更稳定的 Android payload 注入流程。" },
      highlights: { en: ["Optimized mobile injection flow", "Actively maintained", "Practical utility focus"], zh: ["移动端注入流程优化", "持续活跃更新", "面向实用工具场景"] },
      stars: 103,
      language: "Kotlin",
      url: "https://github.com/huhao1987/NXloaderRB",
      repo: "huhao1987/NXloaderRB",
      icon: iconSvg("NX", "#0ea5e9", "#2563eb"),
      shots: [{ src: repoPreview("huhao1987/NXloaderRB"), caption: "Repository social preview" }]
    },
    {
      id: "mgba-android",
      name: "mGBA_Android",
      status: "released",
      subtitle: { en: "mGBA Android port", zh: "mGBA 的 Android 移植版" },
      summary: { en: "Android port of mGBA focused on performance and compatibility.", zh: "mGBA 模拟器 Android 端移植项目，聚焦性能和兼容性。" },
      highlights: { en: ["Handheld emulation direction", "Native Android experience", "Homepage flagship project"], zh: ["掌机模拟器方向", "Android 原生体验", "主页重点项目"] },
      stars: 48,
      language: "Kotlin",
      url: "https://github.com/huhao1987/mGBA_Android",
      repo: "huhao1987/mGBA_Android",
      icon: iconSvg("MG", "#06b6d4", "#16a34a"),
      shots: [{ src: repoPreview("huhao1987/mGBA_Android"), caption: "Repository social preview" }]
    },
    {
      id: "rmmv-deployment",
      name: "RMMV-android-deployment",
      status: "released",
      subtitle: { en: "RPG Maker MV Android deployment library", zh: "RPG Maker MV 的 Android 部署库" },
      summary: { en: "A Kotlin library helping RPG Maker MV creators ship Android builds faster.", zh: "帮助 RPG Maker MV 作者更快速构建 Android 版本的 Kotlin 库。" },
      highlights: { en: ["Lower deployment barrier", "Reusable library capability", "Great for dev ecosystem showcase"], zh: ["降低部署门槛", "库化能力", "适合开发者生态展示"] },
      stars: 10,
      language: "Kotlin",
      url: "https://github.com/huhao1987/RMMV-android-deployment",
      repo: "huhao1987/RMMV-android-deployment",
      icon: iconSvg("RM", "#0ea5e9", "#f97316"),
      shots: [{ src: repoPreview("huhao1987/RMMV-android-deployment"), caption: "Repository social preview" }]
    },
    {
      id: "citra-cheat-public",
      name: "citra_cheat_database_public",
      status: "released",
      subtitle: { en: "Public cheat database for Citra workflows", zh: "面向 Citra 流程的公开作弊数据库" },
      summary: { en: "A public repository for Citra-related cheat data sharing and maintenance.", zh: "Citra 相关公开数据库仓库，便于规则共享与维护。" },
      highlights: { en: ["Data maintenance focused", "Toolchain support", "Public sharing orientation"], zh: ["数据维护型项目", "工具链支撑", "公开分享导向"] },
      stars: 3,
      language: "Data",
      url: "https://github.com/huhao1987/citra_cheat_database_public",
      repo: "huhao1987/citra_cheat_database_public",
      icon: iconSvg("CT", "#ef4444", "#f97316"),
      shots: [{ src: repoPreview("huhao1987/citra_cheat_database_public"), caption: "Repository social preview" }]
    },
    {
      id: "usrcheatreader",
      name: "usrcheatreader",
      status: "released",
      subtitle: { en: "Read usrcheat.dat on Android", zh: "在 Android 上读取 usrcheat.dat" },
      summary: { en: "Android utility for reading NDS cheat file `usrcheat.dat`.", zh: "用于读取 NDS cheat 文件 `usrcheat.dat` 的 Android 工具。" },
      highlights: { en: ["NDS file parsing", "Works with emulator workflows", "Lightweight practical tool"], zh: ["NDS 文件解析", "与模拟器方向协同", "轻量实用工具"] },
      stars: 1,
      language: "Kotlin",
      url: "https://github.com/huhao1987/usrcheatreader",
      repo: "huhao1987/usrcheatreader",
      icon: iconSvg("UR", "#ec4899", "#f97316"),
      shots: [{ src: repoPreview("huhao1987/usrcheatreader"), caption: "Repository social preview" }]
    },
    {
      id: "usrcheat-android",
      name: "usrcheat_android",
      status: "released",
      subtitle: { en: "Android library for usrcheat.dat", zh: "usrcheat.dat 的 Android 解析库" },
      summary: { en: "A reusable Android library for parsing `usrcheat.dat`.", zh: "Android 平台解析 `usrcheat.dat` 的基础库，可被其他项目集成。" },
      highlights: { en: ["Low-level capability", "Reusable library", "Upstream/downstream with usrcheatreader"], zh: ["偏底层能力", "可复用库", "与 usrcheatreader 上下游配套"] },
      stars: 0,
      language: "Kotlin",
      url: "https://github.com/huhao1987/usrcheat_android",
      repo: "huhao1987/usrcheat_android",
      icon: iconSvg("UA", "#06b6d4", "#6366f1"),
      shots: [{ src: repoPreview("huhao1987/usrcheat_android"), caption: "Repository social preview" }]
    },
    {
      id: "hhrmxp-android",
      name: "HHRmxp-android",
      status: "developing",
      subtitle: { en: "RMXP-Z based Android runtime", zh: "基于 RMXP-Z 的 Android 运行时" },
      summary: { en: "Built on rmxp-z with major refactors, targeting RPG Maker XP runtime on Android.", zh: "基于 rmxp-z 并重构了大量内容，目标支持 RPG Maker XP 在 Android 运行。" },
      highlights: { en: ["Current stage: prototype", "Core direction: runtime compatibility", "Long-term key project"], zh: ["当前阶段：prototype", "核心方向：运行时兼容", "长期重点项目"] },
      stars: null,
      language: "Kotlin / C++",
      url: null,
      repo: null,
      icon: iconSvg("HX", "#8b5cf6", "#ec4899"),
      shots: [{ src: placeholderShot("HHRmxp-android", "#1f2937", "#7c3aed"), caption: "Prototype visual" }]
    },
    {
      id: "hhgbaemulator",
      name: "HHGbaEmulator",
      status: "developing",
      subtitle: { en: "New Android GBA/GB/GBC emulator", zh: "全新 Android GBA/GB/GBC 模拟器" },
      summary: { en: "A new Android emulator project for GBA / GB / GBC.", zh: "全新 Android 模拟器项目，覆盖 GBA / GB / GBC。" },
      highlights: { en: ["Current stage: early development", "Multi-system support", "Planned as next-generation core emulator"], zh: ["当前阶段：early development", "多机种支持", "计划作为新一代主力模拟器"] },
      stars: null,
      language: "Kotlin / Native",
      url: null,
      repo: null,
      icon: iconSvg("HG", "#14b8a6", "#3b82f6"),
      shots: [{ src: placeholderShot("HHGbaEmulator", "#0f172a", "#0ea5e9"), caption: "Prototype visual" }]
    }
  ];

  const appGrid = document.getElementById("appGrid");
  const repoSearch = document.getElementById("repoSearch");
  const filters = document.querySelectorAll(".filter");
  const projectCount = document.getElementById("projectCount");
  const syncNote = document.getElementById("syncNote");

  const sheet = document.getElementById("sheet");
  const sheetMask = document.getElementById("sheetMask");
  const sheetClose = document.getElementById("sheetClose");
  const openSheetBtn = document.getElementById("openSheet");

  let currentFilter = "all";
  let activeId = projects[0].id;
  let currentLang = "en";

  function tr(key) {
    return i18n[currentLang][key] || key;
  }

  function statusLabel(status) {
    return status === "released" ? tr("statusReleased") : tr("statusDeveloping");
  }

  function localizeStaticText() {
    document.getElementById("txtTitle").textContent = tr("title");
    document.getElementById("txtGithubBtn").textContent = tr("github");
    document.getElementById("txtLangLabel").textContent = tr("languageLabel");
    document.getElementById("txtFeatureChip").textContent = tr("feature");
    document.getElementById("repoSearch").placeholder = tr("searchPlaceholder");
    document.getElementById("txtFilterAll").textContent = tr("filterAll");
    document.getElementById("txtFilterReleased").textContent = tr("filterReleased");
    document.getElementById("txtFilterDeveloping").textContent = tr("filterDeveloping");
    document.getElementById("txtStars").textContent = tr("stars");
    document.getElementById("txtLanguage").textContent = tr("language");
    document.getElementById("openSheet").textContent = tr("fullPreview");
    document.getElementById("txtHighlights").textContent = tr("highlights");
    document.getElementById("txtShots").textContent = tr("shots");
    document.getElementById("langSelect").value = currentLang;
  }

  function fitFilter(project) {
    return currentFilter === "all" ? true : project.status === currentFilter;
  }

  function fitKeyword(project, keyword) {
    if (!keyword) return true;
    const text = `${project.name} ${project.subtitle[currentLang]} ${project.summary[currentLang]}`.toLowerCase();
    return text.includes(keyword.toLowerCase());
  }

  function renderGrid() {
    const keyword = repoSearch.value.trim();
    const list = projects.filter((project) => fitFilter(project) && fitKeyword(project, keyword));

    if (!list.length) {
      appGrid.innerHTML = `<li class="empty">${tr("noMatch")}</li>`;
      return;
    }

    if (!list.some((project) => project.id === activeId)) {
      activeId = list[0].id;
    }

    appGrid.innerHTML = list
      .map((project) => {
        const stars = project.stars === null ? "N/A" : project.stars;
        return `
          <li>
            <button type="button" class="app-card ${project.id === activeId ? "is-active" : ""}" data-id="${project.id}">
              <img src="${project.icon}" alt="${project.name} icon" loading="lazy" />
              <div class="app-card-body">
                <strong>${project.name}</strong>
                <p>${project.subtitle[currentLang]}</p>
                <span>${statusLabel(project.status)} · ⭐ ${stars}</span>
              </div>
            </button>
          </li>`;
      })
      .join("");

    document.querySelectorAll(".app-card").forEach((button) => {
      button.addEventListener("click", () => {
        activeId = button.dataset.id;
        renderGrid();
        renderDetail();
      });
    });
  }

  function renderPreviewStrip(project) {
    const strip = document.getElementById("previewStrip");
    strip.innerHTML = project.shots
      .map(
        (shot) => `
          <button type="button" class="shot" data-src="${shot.src}" data-caption="${shot.caption}">
            <img src="${shot.src}" alt="${project.name} preview" loading="lazy" />
            <span>${shot.caption}</span>
          </button>`
      )
      .join("");

    strip.querySelectorAll(".shot").forEach((button) => {
      button.addEventListener("click", () => openSheet(button.dataset.src, button.dataset.caption));
    });
  }

  function renderDetail() {
    const project = projects.find((item) => item.id === activeId);
    if (!project) return;

    const iconCode = project.name.replace(/[^A-Za-z]/g, "").slice(0, 2).toUpperCase();
    document.getElementById("detailPanel").dataset.status = project.status;
    document.getElementById("detailIcon").style.backgroundImage = `url('${project.icon}')`;
    document.getElementById("detailIcon").textContent = iconCode;
    document.getElementById("detailStatus").textContent = statusLabel(project.status);
    document.getElementById("detailName").textContent = project.name;
    document.getElementById("detailSubtitle").textContent = project.subtitle[currentLang];
    document.getElementById("detailSummary").textContent = project.summary[currentLang];
    document.getElementById("detailStars").textContent = project.stars === null ? "N/A" : project.stars;
    document.getElementById("detailLang").textContent = project.language;

    const hero = document.getElementById("detailHero");
    hero.src = project.shots[0] ? project.shots[0].src : project.icon;
    hero.alt = `${project.name} hero`;

    const repoLink = document.getElementById("detailUrl");
    if (project.url) {
      repoLink.href = project.url;
      repoLink.textContent = tr("openRepo");
      repoLink.classList.remove("is-disabled");
    } else {
      repoLink.removeAttribute("href");
      repoLink.textContent = tr("repoSoon");
      repoLink.classList.add("is-disabled");
    }

    document.getElementById("detailHighlights").innerHTML = project.highlights[currentLang]
      .map((item) => `<li>${item}</li>`)
      .join("");

    renderPreviewStrip(project);

    document.getElementById("sheetIcon").style.backgroundImage = `url('${project.icon}')`;
    document.getElementById("sheetIcon").textContent = iconCode;
    document.getElementById("sheetStatus").textContent = statusLabel(project.status);
    document.getElementById("sheetName").textContent = project.name;
  }

  function openSheet(src, caption) {
    const panel = document.getElementById("sheetGallery");
    panel.innerHTML = `
      <figure>
        <img src="${src}" alt="preview" />
        <figcaption>${caption}</figcaption>
      </figure>`;
    sheet.classList.add("is-open");
    sheet.setAttribute("aria-hidden", "false");
  }

  function closeSheet() {
    sheet.classList.remove("is-open");
    sheet.setAttribute("aria-hidden", "true");
  }

  function updateProjectCount() {
    const suffix = tr("projectsSuffix");
    projectCount.textContent = currentLang === "en" ? `${projects.length} ${suffix}` : `${projects.length}${suffix}`;
  }

  async function refreshGitHubData() {
    const targets = projects.filter((project) => project.repo);
    syncNote.textContent = tr("syncLoading");

    await Promise.all(
      targets.map(async (project) => {
        try {
          const response = await fetch(`https://api.github.com/repos/${project.repo}`);
          if (!response.ok) return;
          const data = await response.json();
          project.stars = data.stargazers_count;
          if (data.language) project.language = data.language;
        } catch (error) {
          console.warn("sync failed", project.name, error);
        }
      })
    );

    syncNote.textContent = `${tr("syncDone")} ${new Date().toLocaleString()}`;
    renderGrid();
    renderDetail();
  }

  function setLanguage(lang) {
    currentLang = lang;
    localizeStaticText();
    updateProjectCount();
    if (!syncNote.textContent || syncNote.textContent.includes("initializing") || syncNote.textContent.includes("初始化")) {
      syncNote.textContent = tr("syncInit");
    }
    renderGrid();
    renderDetail();
  }

  document.getElementById("langSelect").addEventListener("change", (event) => {
    setLanguage(event.target.value);
  });
  openSheetBtn.addEventListener("click", () => {
    const project = projects.find((item) => item.id === activeId);
    if (project && project.shots[0]) {
      openSheet(project.shots[0].src, project.shots[0].caption);
    }
  });

  sheetMask.addEventListener("click", closeSheet);
  sheetClose.addEventListener("click", closeSheet);

  document.addEventListener("keydown", (event) => {
    if (event.key === "Escape") closeSheet();
  });

  filters.forEach((button) => {
    button.addEventListener("click", () => {
      filters.forEach((item) => item.classList.remove("is-active"));
      button.classList.add("is-active");
      currentFilter = button.dataset.filter;
      renderGrid();
      renderDetail();
    });
  });

  repoSearch.addEventListener("input", () => {
    renderGrid();
    renderDetail();
  });

  setLanguage("en");
  refreshGitHubData();
</script>
