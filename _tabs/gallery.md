---
layout: gallery-tab
title: Gallery
icon: fas fa-images
order: 3
---

<style>
/* ── Gallery filter bar ─────────────────────────────────────────── */
.gallery-filter {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.5rem 0.75rem;
  border: 1.5px solid var(--link-color);
  border-radius: 8px;
  margin-bottom: 1.75rem;
  flex-wrap: wrap;
}
.gallery-filter-label {
  font-size: 0.78rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 0.06em;
  color: var(--link-color);
  margin-right: 0.25rem;
  white-space: nowrap;
}
.gf-btn {
  padding: 0.3rem 0.9rem;
  border: 1.5px solid var(--link-color);
  border-radius: 20px;
  background: transparent;
  color: var(--text-muted-color);
  font-size: 0.85rem;
  cursor: pointer;
  transition: background 0.15s, color 0.15s;
  font-family: inherit;
}
.gf-btn:hover {
  background: color-mix(in srgb, var(--link-color) 15%, transparent);
  color: var(--link-color);
}
.gf-btn.active {
  background: var(--link-color);
  color: #fff;
  font-weight: 600;
}

/* ── Gallery grid ───────────────────────────────────────────────── */
.gallery-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(180px, 1fr));
  gap: 1rem;
}

/* ── Gallery card ───────────────────────────────────────────────── */
.gallery-card {
  border: 1px solid var(--main-border-color);
  border-radius: 8px;
  overflow: hidden;
  transition: box-shadow 0.2s, transform 0.2s;
  cursor: pointer;
  background: var(--card-bg, transparent);
}
.gallery-card:hover {
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.15);
  transform: translateY(-2px);
}

/* Card header: 날짜 + 장소 */
.gallery-card-header {
  display: flex;
  font-size: 0.72rem;
  color: var(--text-muted-color);
  border-bottom: 1px solid var(--main-border-color);
}
.gallery-card-date,
.gallery-card-location {
  padding: 0.28rem 0.5rem;
  flex: 1;
  min-width: 0;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  display: flex;
  align-items: center;
  gap: 0.3rem;
}
.gallery-card-date { border-right: 1px solid var(--main-border-color); }
.gallery-card-date.no-sep { border-right: none; }  /* location 없을 때 구분선 제거 */
.gallery-card-location { justify-content: flex-end; }
.gallery-card-header i {
  font-size: 0.68rem;
  opacity: 0.55;
  flex-shrink: 0;
}

/* Twemoji 이미지 크기 정규화 */
img.emoji { height: 1em; width: 1em; vertical-align: -0.1em; }

/* Thumbnail — background-image 방식으로 Chirpy popup 래핑 회피 */
.gallery-card-thumb {
  position: relative;
  width: 100%;
  aspect-ratio: 3 / 4;
  overflow: hidden;
  background-color: #ffffff;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
}

/* Multi-image badge */
.gallery-card-count {
  position: absolute;
  top: 0.4rem;
  right: 0.45rem;
  background: rgba(0, 0, 0, 0.55);
  color: #fff;
  font-size: 0.68rem;
  font-weight: 600;
  padding: 0.15rem 0.45rem;
  border-radius: 10px;
  pointer-events: none;
}

/* Title bar */
.gallery-card-title {
  padding: 0.4rem 0.6rem;
  font-size: 0.83rem;
  font-weight: 500;
  color: var(--heading-color);
  text-align: center;
  border-top: 1px solid var(--main-border-color);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

/* ── Empty state ────────────────────────────────────────────────── */
.gallery-empty {
  text-align: center;
  padding: 4rem 0;
  color: var(--text-muted-color);
  opacity: 0.6;
}
.gallery-empty i { font-size: 3rem; display: block; margin-bottom: 0.75rem; }
.gallery-empty p { font-size: 1rem; margin: 0; }

/* ── Modal overlay ──────────────────────────────────────────────── */
.gallery-modal-overlay {
  display: none;
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(0, 0, 0, 0.87);
  z-index: 2147483647;
  align-items: center;
  justify-content: center;
  padding: 1.5rem;
  box-sizing: border-box;
}
.gallery-modal-overlay.gallery-open {
  display: flex;
}
.gallery-modal-inner {
  display: flex;
  flex-direction: row;
  width: 92vw;
  max-width: 960px;
  height: 80vh;
  max-height: 680px;
  flex: 0 0 auto;
  border-radius: 4px;
  overflow: hidden;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
}

/* Image panel */
.gallery-modal-img-panel {
  width: 60%;
  flex-shrink: 0;
  height: 100%;
  background: #000;
  position: relative;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal-dyn-img {
  display: block;
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
}
.modal-dyn-placeholder {
  position: absolute;
  inset: 0;
  background: #fff;
}

/* Prev / Next arrows */
.modal-arrow {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  background: rgba(255, 255, 255, 0.18);
  border: none;
  color: #fff;
  font-size: 1.2rem;
  width: 2.2rem;
  height: 2.2rem;
  border-radius: 50%;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.15s;
  z-index: 2;
}
.modal-arrow:hover { background: rgba(255, 255, 255, 0.35); }
.modal-arrow-prev { left: 0.6rem; }
.modal-arrow-next { right: 0.6rem; }

/* Image counter dots */
.modal-dots {
  position: absolute;
  bottom: 0.6rem;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 0.35rem;
  z-index: 2;
}
.modal-dot {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.45);
  transition: background 0.15s;
}
.modal-dot.active { background: #fff; }

/* Info panel — flex column containing global + local sections */
.gallery-modal-info-panel {
  width: 40%;
  flex-shrink: 0;
  height: 100%;
  background: var(--main-bg);
  border-left: 1px solid var(--main-border-color);
  display: flex;
  flex-direction: column;
  overflow: hidden;
  box-sizing: border-box;
}

/* Global section (card-level) */
.gallery-modal-global {
  flex: 1;
  min-height: 0;
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

/* Local section (per-image) — hidden by default */
.gallery-modal-local {
  flex: 1;
  min-height: 0;
  display: none;   /* JS shows this when image has local data */
  flex-direction: column;
  overflow: hidden;
  border-top: 2px solid var(--main-border-color);
}
.gallery-modal-local.has-local {
  display: flex;
}
/* Small "Local" label at top of local section */
.gallery-modal-local-label {
  font-size: 0.62rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.07em;
  color: var(--link-color);
  padding: 0.4rem 1.25rem 0;
  flex-shrink: 0;
}

.gallery-modal-header {
  padding: 1rem 1.25rem;
  border-bottom: 1px solid var(--main-border-color);
  flex-shrink: 0;
}
/* Local header has no title, less top padding */
.gallery-modal-local .gallery-modal-header {
  padding-top: 0.5rem;
}
.gallery-modal-title {
  font-size: 1rem;
  font-weight: 700;
  color: var(--heading-color);
  margin: 0 0 0.6rem;
  line-height: 1.4;
}
.gallery-modal-meta {
  display: flex;
  align-items: center;
  gap: 1.25rem;
  font-size: 0.78rem;
  color: var(--text-muted-color);
}
.gallery-modal-meta i {
  margin-right: 0.3rem;
  opacity: 0.6;
  font-size: 0.72rem;
}
.gallery-modal-body {
  padding: 1.1rem 1.25rem;
  font-size: 0.9rem;
  color: var(--text-color);
  line-height: 1.8;
  overflow-y: auto;
  flex: 1;
  white-space: pre-wrap;
  word-break: break-word;
}

/* Close button */
.gallery-modal-close {
  position: absolute;
  top: 0.6rem;
  right: 0.8rem;
  background: none;
  border: none;
  color: #fff;
  font-size: 1.9rem;
  line-height: 1;
  cursor: pointer;
  padding: 0.2rem 0.4rem;
  z-index: 10000;
  opacity: 0.8;
  transition: opacity 0.15s;
}
.gallery-modal-close:hover { opacity: 1; }

@media (max-width: 640px) {
  .gallery-modal-overlay { padding: 0; }
  .gallery-modal-inner {
    flex-direction: column;
    width: 100vw;
    height: 100vh;
    border-radius: 0;
  }
  .gallery-modal-img-panel { width: 100%; height: 55%; }
  .gallery-modal-info-panel {
    width: 100%;
    height: 45%;
    border-left: none;
    border-top: 1px solid var(--main-border-color);
  }
}
</style>

<!-- Filter bar -->
<div class="gallery-filter">
  <span class="gallery-filter-label">Filter</span>
  <button class="gf-btn active" data-filter="all">All</button>
  {% for cat in site.data.gallery.categories %}
  <button class="gf-btn" data-filter="{{ cat }}">{{ cat }}</button>
  {% endfor %}
</div>

<!-- Photo grid -->
{% if site.data.gallery.items.size > 0 %}
{% assign sorted_gallery = site.data.gallery.items | sort: "date" | reverse %}
<div class="gallery-grid" id="gallery-grid">
  {% for item in sorted_gallery %}
  {% assign imgs = item.images | default: "" %}
  {% assign first_img = "" %}
  {% if imgs != "" %}
    {% assign _fi = imgs[0] %}
    {% if _fi.url %}{% assign first_img = _fi.url | strip %}
    {% else %}{% assign first_img = _fi | default: "" | strip %}{% endif %}
  {% endif %}
  <div class="gallery-card" data-index="{{ forloop.index0 }}" data-category="{{ item.category }}" data-date="{{ item.date | default: '' }}">
    {% assign has_loc = false %}
    {% if item.location and item.location != "" %}{% assign has_loc = true %}{% endif %}
    <div class="gallery-card-header">
      {% if item.date and item.date != "" %}
      <span class="gallery-card-date{% unless has_loc %} no-sep{% endunless %}">
        <i class="fas fa-calendar"></i>{{ item.date }}
      </span>
      {% endif %}
      {% if has_loc %}
      <span class="gallery-card-location">
        <i class="fas fa-location-dot"></i>{{ item.location }}
      </span>
      {% endif %}
    </div>
    <div class="gallery-card-thumb"{% if first_img != "" %} style="background-image:url('{{ first_img | relative_url }}');"{% endif %}>
      {% if imgs != "" and imgs.size > 1 %}
        <span class="gallery-card-count">1 / {{ imgs.size }}</span>
      {% endif %}
    </div>
    <div class="gallery-card-title">{{ item.title }}</div>
  </div>
  {% endfor %}
</div>

<!-- 날짜 필터 결과 없음 메시지 -->
<div id="gallery-no-date-msg" class="gallery-empty" style="display:none;">
  <i class="fas fa-calendar-times"></i>
  <p id="gallery-no-date-text"></p>
</div>

<!-- Modal (JS will move this to document.body to escape Chirpy transforms) -->
<div class="gallery-modal-overlay" id="gallery-modal">
  <button class="gallery-modal-close" id="modal-close" aria-label="Close">&#x2715;</button>
  <div class="gallery-modal-inner">
    <div class="gallery-modal-img-panel" id="modal-img-panel">
      <button class="modal-arrow modal-arrow-prev" id="modal-prev">&#8249;</button>
      <button class="modal-arrow modal-arrow-next" id="modal-next">&#8250;</button>
      <div class="modal-dots" id="modal-dots"></div>
    </div>
    <div class="gallery-modal-info-panel">
      <!-- ── Global: card-level info ── -->
      <div class="gallery-modal-global">
        <div class="gallery-modal-header">
          <div class="gallery-modal-title" id="modal-title"></div>
          <div class="gallery-modal-meta">
            <span id="modal-date-wrap"><i class="fas fa-calendar"></i><span id="modal-date"></span></span>
            <span id="modal-loc-wrap"><i class="fas fa-location-dot"></i><span id="modal-location"></span></span>
          </div>
        </div>
        <div class="gallery-modal-body" id="modal-body"></div>
      </div>
      <!-- ── Local: per-image info (JS toggles .has-local) ── -->
      <div class="gallery-modal-local" id="modal-local">
        <div class="gallery-modal-header">
          <div class="gallery-modal-meta">
            <span id="modal-img-date-wrap"><i class="fas fa-calendar"></i><span id="modal-img-date"></span></span>
            <span id="modal-img-loc-wrap"><i class="fas fa-location-dot"></i><span id="modal-img-location"></span></span>
          </div>
        </div>
        <div class="gallery-modal-body" id="modal-img-body"></div>
      </div>
    </div>
  </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/twemoji@14/dist/twemoji.min.js" crossorigin="anonymous"></script>
<script>
/* helper: Twemoji로 이모지 → SVG 이미지 변환 (flag emoji 포함) */
function twparse(el) {
  if (typeof twemoji !== 'undefined') {
    twemoji.parse(el, { folder: 'svg', ext: '.svg', base: 'https://cdn.jsdelivr.net/gh/twitter/twemoji@14.0.2/assets/' });
  }
}

var galleryData = [
  {% assign sorted_gallery = site.data.gallery.items | sort: "date" | reverse %}
  {% for item in sorted_gallery %}
  (function(){
    var imgs = [];
    {% if item.images %}
      {% for img in item.images %}
        {% comment %} 이미지가 문자열(URL만)이면 img.url=nil → default로 img 자체 사용 {% endcomment %}
        {% assign img_url = img.url | default: img %}
        {% assign img_url_s = img_url | strip %}
        {% if img_url_s != "" %}
        imgs.push({
          url:         {{ img_url_s | relative_url | jsonify }},
          date:        {{ img.date        | default: "" | jsonify }},
          location:    {{ img.location    | default: "" | jsonify }},
          description: {{ img.description | default: "" | jsonify }}
        });
        {% endif %}
      {% endfor %}
    {% endif %}
    return {
      title:       {{ item.title       | default: "" | jsonify }},
      date:        {{ item.date        | default: "" | jsonify }},
      location:    {{ item.location    | default: "" | jsonify }},
      description: {{ item.description | default: "" | jsonify }},
      images:      imgs
    };
  })(){% unless forloop.last %},{% endunless %}
  {% endfor %}
];
window.galleryData = galleryData;

(function () {
  if (window._galleryModalReady) return;
  window._galleryModalReady = true;

  var overlay   = document.getElementById('gallery-modal');
  var closeBtn  = document.getElementById('modal-close');
  var imgPanel  = document.getElementById('modal-img-panel');
  var modalTitle      = document.getElementById('modal-title');
  var modalDate       = document.getElementById('modal-date');
  var modalDateWrap   = document.getElementById('modal-date-wrap');
  var modalLoc        = document.getElementById('modal-location');
  var modalLocWrap    = document.getElementById('modal-loc-wrap');
  var modalBody       = document.getElementById('modal-body');
  /* local section elements */
  var modalLocal      = document.getElementById('modal-local');
  var modalImgDate    = document.getElementById('modal-img-date');
  var modalImgDateWrap= document.getElementById('modal-img-date-wrap');
  var modalImgLoc     = document.getElementById('modal-img-location');
  var modalImgLocWrap = document.getElementById('modal-img-loc-wrap');
  var modalImgBody    = document.getElementById('modal-img-body');
  var modalPrev = document.getElementById('modal-prev');
  var modalNext = document.getElementById('modal-next');
  var modalDots = document.getElementById('modal-dots');

  document.body.appendChild(overlay);

  var curImages = [];
  var curIdx    = 0;
  var isOpen    = false;

  function clearPanel() {
    var old = imgPanel.querySelector('.modal-dyn-img, .modal-dyn-placeholder');
    while (old) { imgPanel.removeChild(old); old = imgPanel.querySelector('.modal-dyn-img, .modal-dyn-placeholder'); }
  }

  function renderLocal(imgObj) {
    var hasDate = imgObj.date && imgObj.date !== '';
    var hasLoc  = imgObj.location && imgObj.location !== '';
    var hasDesc = imgObj.description && imgObj.description !== '';
    if (!hasDate && !hasLoc && !hasDesc) {
      modalLocal.classList.remove('has-local');
      return;
    }
    modalLocal.classList.add('has-local');
    if (hasDate) { modalImgDate.textContent = imgObj.date; modalImgDateWrap.style.display = ''; }
    else { modalImgDateWrap.style.display = 'none'; }
    if (hasLoc)  { modalImgLoc.textContent  = imgObj.location; modalImgLocWrap.style.display = ''; }
    else { modalImgLocWrap.style.display = 'none'; }
    modalImgBody.textContent = imgObj.description || '';
  }

  function renderImage(idx) {
    curIdx = idx;
    clearPanel();
    var imgObj = curImages[idx] || {};
    var src = (imgObj.url || '').trim();
    if (src) {
      var img = document.createElement('img');
      img.className = 'modal-dyn-img';
      img.src = src;
      imgPanel.insertBefore(img, imgPanel.firstChild);
    } else {
      var ph = document.createElement('div');
      ph.className = 'modal-dyn-placeholder';
      imgPanel.insertBefore(ph, imgPanel.firstChild);
    }
    var n = curImages.length;
    modalPrev.style.display = (n > 1 && idx > 0)     ? '' : 'none';
    modalNext.style.display = (n > 1 && idx < n - 1) ? '' : 'none';
    modalDots.querySelectorAll('.modal-dot').forEach(function (d, i) {
      d.classList.toggle('active', i === idx);
    });
    renderLocal(imgObj);
  }

  function buildDots(n) {
    modalDots.innerHTML = '';
    if (n <= 1) return;
    for (var i = 0; i < n; i++) {
      var d = document.createElement('span');
      d.className = 'modal-dot';
      modalDots.appendChild(d);
    }
  }

  function closeModal() {
    if (!isOpen) return;
    isOpen = false;
    overlay.classList.remove('gallery-open');
    document.body.style.overflow = '';
    clearPanel();
    modalLocal.classList.remove('has-local');
    curImages = [];
  }

  document.querySelectorAll('.gallery-card').forEach(function (card) {
    card.addEventListener('click', function (e) {
      e.stopPropagation();
      if (isOpen) return;
      var idx  = parseInt(card.getAttribute('data-index'), 10);
      var data = galleryData[idx];
      if (!data) return;
      curImages = data.images || [];
      modalTitle.textContent = data.title;
      twparse(modalTitle);
      modalBody.textContent  = data.description;
      if (data.date) {
        modalDate.textContent = data.date;
        modalDateWrap.style.display = '';
      } else {
        modalDate.textContent = '';
        modalDateWrap.style.display = 'none';
      }
      if (data.location) {
        modalLoc.textContent = data.location;
        modalLocWrap.style.display = '';
      } else {
        modalLoc.textContent = '';
        modalLocWrap.style.display = 'none';
      }
      buildDots(curImages.length);
      renderImage(0);
      isOpen = true;
      overlay.classList.add('gallery-open');
      document.body.style.overflow = 'hidden';
    });
  });

  modalPrev.addEventListener('click', function (e) { e.stopPropagation(); if (curIdx > 0) renderImage(curIdx - 1); });
  modalNext.addEventListener('click', function (e) { e.stopPropagation(); if (curIdx < curImages.length - 1) renderImage(curIdx + 1); });

  closeBtn.addEventListener('click', function (e) { e.stopPropagation(); closeModal(); });
  overlay.addEventListener('click', function (e) { if (e.target === overlay) closeModal(); });
  document.addEventListener('keydown', function (e) {
    if (!overlay.classList.contains('gallery-open')) return;
    if (e.key === 'Escape')     closeModal();
    if (e.key === 'ArrowLeft')  { if (curIdx > 0) renderImage(curIdx - 1); }
    if (e.key === 'ArrowRight') { if (curIdx < curImages.length - 1) renderImage(curIdx + 1); }
  });

  /* 카드 제목 이모지 렌더링 */
  document.querySelectorAll('.gallery-card-title').forEach(function (el) { twparse(el); });

  /* ── 공통 필터 상태 (카테고리 + 날짜) ── */
  window._galleryFilter = { cat: 'all', date: null };
  window._galleryApplyFilter = function() {
    var cat = window._galleryFilter.cat;
    var df  = window._galleryFilter.date; /* null | {type:'day'|'month', val:string} */
    var visible = 0;
    document.querySelectorAll('.gallery-card').forEach(function(c) {
      var catOk  = (cat === 'all' || c.getAttribute('data-category') === cat);
      var dateOk = true;
      if (df) {
        var cd = c.getAttribute('data-date') || '';
        if (df.type === 'day') {
          dateOk = cd === df.val;
        } else {
          /* month: 'YYYY.MM' 또는 'YYYY.MM.DD' 모두 허용 */
          dateOk = cd === df.val || cd.indexOf(df.val + '.') === 0;
        }
      }
      var show = catOk && dateOk;
      c.style.display = show ? '' : 'none';
      if (show) visible++;
    });
    /* 날짜 필터 결과 없음 메시지 */
    var noMsg = document.getElementById('gallery-no-date-msg');
    var noTxt = document.getElementById('gallery-no-date-text');
    if (noMsg && noTxt) {
      if (df && visible === 0) {
        var label = '';
        if (df.type === 'month') {
          var mo = parseInt(df.val.split('.')[1], 10);
          label = mo + '월에 올린 카드가 없습니다.';
        } else {
          label = df.val + '에 올린 카드가 없습니다.';
        }
        noTxt.textContent = label;
        noMsg.style.display = '';
      } else {
        noMsg.style.display = 'none';
      }
    }
  };

  /* ── Filter bar ── */
  var filterBtns = document.querySelectorAll('.gf-btn');
  filterBtns.forEach(function (btn) {
    btn.addEventListener('click', function () {
      filterBtns.forEach(function (b) { b.classList.remove('active'); });
      btn.classList.add('active');
      window._galleryFilter.cat = btn.getAttribute('data-filter');
      window._galleryApplyFilter();
    });
  });
})();
</script>

{% else %}
<div class="gallery-empty">
  <i class="fas fa-images"></i>
  <p>사진이 아직 없어요. <code>_data/gallery.yml</code>에 항목을 추가해보세요.</p>
</div>
{% endif %}
