<template>
  <div class="dashboard-wrapper">
    <div class="profile-bar d-flex align-items-center justify-content-end p-3 container">
      <div v-if="user" class="user-info d-flex align-items-center gap-3">
        <span class="text-white fw-bold">{{ user.displayName }}</span>
        <img
          :src="user.photoURL"
          referrerpolicy="no-referrer"
          class="profile-pic"
          alt="Profil"
        >
      </div>
    </div>

    <div class="container py-4">
      <header class="text-center mb-5 text-white">
        <h1 class="display-5 fw-light">Ravi de vous voir !</h1>
        <p class="opacity-75">Quel modèle souhaitez-vous utiliser aujourd'hui ?</p>
      </header>

      <div class="row g-4">
        <div
          v-for="temp in templates"
          :key="temp.id"
          class="col-md-6 col-lg-4"
        >
          <div class="glass-card-template" @click="emit('select-template', temp.id)">
            <div class="preview-area">
              <!-- Miniature unique par template -->
              <div class="cv-mockup" :class="'mockup-' + temp.id">
                <div class="mockup-inner" v-html="temp.mockup"></div>
              </div>
            </div>
            <div class="card-info text-center">
              <h3 class="h5 mb-1 text-white">{{ temp.name }}</h3>
              <p class="small text-white-50">{{ temp.description }}</p>
              <button class="btn-select mt-2">Éditer ce modèle</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({ user: { type: Object, default: null } })
const emit = defineEmits(['select-template'])

const templates = [
  {
    id: 'TemplateCV1',
    name: 'Modèle 1',
    description: 'Design épuré et moderne',
    mockup: `
      <div class="mk mk1">
        <div class="mk-left">
          <div class="mk-avatar"></div>
          <div class="mk-line w60 mt8"></div>
          <div class="mk-line w80 mt4"></div>
          <div class="mk-sep mt8"></div>
          <div class="mk-label mt8"></div>
          <div class="mk-line w70 mt4"></div>
          <div class="mk-line w50 mt4"></div>
          <div class="mk-sep mt8"></div>
          <div class="mk-label mt8"></div>
          <div class="mk-bar mt4"><div class="mk-fill" style="width:90%"></div></div>
          <div class="mk-bar mt4"><div class="mk-fill" style="width:70%"></div></div>
        </div>
        <div class="mk-right">
          <div class="mk-name"></div>
          <div class="mk-title mt4"></div>
          <div class="mk-sep2 mt8"></div>
          <div class="mk-label mt8"></div>
          <div class="mk-line w90 mt4"></div>
          <div class="mk-line w70 mt4"></div>
          <div class="mk-entry mt8">
            <div class="mk-dot"></div>
            <div>
              <div class="mk-line w80 mt0"></div>
              <div class="mk-line w60 mt4"></div>
            </div>
          </div>
          <div class="mk-entry mt4">
            <div class="mk-dot"></div>
            <div>
              <div class="mk-line w70 mt0"></div>
              <div class="mk-line w50 mt4"></div>
            </div>
          </div>
        </div>
      </div>`
  },
  {
    id: 'TemplateCV2',
    name: 'Modèle 2',
    description: 'Header coloré pleine largeur',
    mockup: `
      <div class="mk mk2">
        <div class="mk2-header">
          <div class="mk2-avatar"></div>
          <div class="mk2-info">
            <div class="mk-name-w"></div>
            <div class="mk-line w50 mt4" style="background:rgba(255,255,255,0.5)"></div>
          </div>
          <div class="mk2-contact">
            <div class="mk-line w80 mt0" style="background:rgba(255,255,255,0.5)"></div>
            <div class="mk-line w60 mt4" style="background:rgba(255,255,255,0.4)"></div>
          </div>
        </div>
        <div class="mk2-body">
          <div class="mk2-main">
            <div class="mk-label mt0"></div>
            <div class="mk-entry mt6">
              <div class="mk-dot themed"></div>
              <div><div class="mk-line w80 mt0"></div><div class="mk-line w60 mt4"></div></div>
            </div>
            <div class="mk-entry mt4">
              <div class="mk-dot themed"></div>
              <div><div class="mk-line w70 mt0"></div><div class="mk-line w50 mt4"></div></div>
            </div>
          </div>
          <div class="mk2-side">
            <div class="mk-label mt0"></div>
            <div class="mk-line w90 mt6"></div>
            <div class="mk-bar mt4"><div class="mk-fill" style="width:85%"></div></div>
            <div class="mk-line w80 mt4"></div>
            <div class="mk-bar mt4"><div class="mk-fill" style="width:60%"></div></div>
          </div>
        </div>
      </div>`
  },
  {
    id: 'TemplateCV3',
    name: 'Modèle 3',
    description: 'Élégance & typographie serif',
    mockup: `
      <div class="mk mk3">
        <div class="mk3-topband"></div>
        <div class="mk3-header">
          <div>
            <div class="mk3-name"></div>
            <div class="mk-line w40 mt4" style="background:var(--acc)"></div>
          </div>
          <div class="mk3-avatar"></div>
        </div>
        <div class="mk3-deco"><div class="mk3-line"></div><div class="mk3-diamond"></div><div class="mk3-line"></div></div>
        <div class="mk3-body">
          <div class="mk3-left">
            <div class="mk-label"></div>
            <div class="mk-entry mt6">
              <div><div class="mk-line w30 mt0" style="opacity:.5"></div><div class="mk-line w80 mt3"></div><div class="mk-line w60 mt3" style="background:var(--acc)"></div></div>
            </div>
            <div class="mk-entry mt6">
              <div><div class="mk-line w30 mt0" style="opacity:.5"></div><div class="mk-line w70 mt3"></div><div class="mk-line w50 mt3" style="background:var(--acc)"></div></div>
            </div>
          </div>
          <div class="mk3-right">
            <div class="mk-label"></div>
            <div class="mk-line w90 mt6"></div>
            <div class="mk-line w70 mt4"></div>
            <div class="mk-label mt8"></div>
            <div class="mk-bar mt4"><div class="mk-fill" style="width:90%"></div></div>
            <div class="mk-bar mt4"><div class="mk-fill" style="width:65%"></div></div>
          </div>
        </div>
        <div class="mk3-botband"></div>
      </div>`
  },
  {
    id: 'TemplateCV4',
    name: 'Modèle 4',
    description: 'Sidebar colorée à droite',
    mockup: `
      <div class="mk mk4">
        <div class="mk4-main">
          <div class="mk4-header">
            <div class="mk-avatar sm"></div>
            <div>
              <div class="mk-name-sm"></div>
              <div class="mk-line w40 mt4" style="background:var(--acc)"></div>
            </div>
          </div>
          <div class="mk4-accent"></div>
          <div class="mk-label mt8"></div>
          <div class="mk-entry mt6">
            <div class="mk-datecol"><div class="mk-line w90 mt0" style="opacity:.5"></div></div>
            <div><div class="mk-line w80 mt0"></div><div class="mk-line w60 mt3" style="background:var(--acc)"></div></div>
          </div>
          <div class="mk-entry mt4">
            <div class="mk-datecol"><div class="mk-line w90 mt0" style="opacity:.5"></div></div>
            <div><div class="mk-line w70 mt0"></div><div class="mk-line w50 mt3" style="background:var(--acc)"></div></div>
          </div>
        </div>
        <div class="mk4-side">
          <div class="mk4-s-avatar"></div>
          <div class="mk-label mt8" style="background:rgba(255,255,255,.4)"></div>
          <div class="mk-line w90 mt6" style="background:rgba(255,255,255,.5)"></div>
          <div class="mk-line w70 mt4" style="background:rgba(255,255,255,.4)"></div>
          <div class="mk-label mt8" style="background:rgba(255,255,255,.4)"></div>
          <div class="mk-bar mt4" style="background:rgba(255,255,255,.2)"><div class="mk-fill" style="width:85%;background:white"></div></div>
          <div class="mk-bar mt4" style="background:rgba(255,255,255,.2)"><div class="mk-fill" style="width:60%;background:white"></div></div>
        </div>
      </div>`
  },
  {
    id: 'TemplateCV5',
    name: 'Modèle 5',
    description: 'Dark mode · Style tech',
    mockup: `
      <div class="mk mk5">
        <div class="mk5-header">
          <div class="mk5-glow"></div>
          <div>
            <div class="mk5-tag"></div>
            <div class="mk5-name mt4"></div>
          </div>
          <div class="mk5-avatar"></div>
        </div>
        <div class="mk5-strip">
          <div class="mk-line w30 mt0" style="background:rgba(255,255,255,.25)"></div>
          <div class="mk-line w25 mt0 ml8" style="background:rgba(255,255,255,.2)"></div>
        </div>
        <div class="mk5-body">
          <div class="mk5-main">
            <div class="mk-label mt0" style="background:#30363d"></div>
            <div class="mk5-card mt6">
              <div class="mk-line w80 mt0" style="background:#c9d1d9"></div>
              <div class="mk-line w60 mt4" style="background:var(--acc)"></div>
            </div>
            <div class="mk5-card mt4">
              <div class="mk-line w70 mt0" style="background:#c9d1d9"></div>
              <div class="mk-line w50 mt4" style="background:var(--acc)"></div>
            </div>
          </div>
          <div class="mk5-side">
            <div class="mk-label mt0" style="background:#30363d"></div>
            <div class="mk5-chip mt6"></div>
            <div class="mk5-chip mt4"></div>
            <div class="mk5-chip mt4"></div>
          </div>
        </div>
      </div>`
  },
  {
    id: 'TemplateCV6',
    name: 'Modèle 6',
    description: 'Éditorial · Style magazine',
    mockup: `
      <div class="mk mk6">
        <div class="mk6-header">
          <div>
            <div class="mk6-overtitle"></div>
            <div class="mk6-name mt4"></div>
            <div class="mk6-stripe mt6"></div>
          </div>
          <div class="mk6-avatar"></div>
        </div>
        <div class="mk6-contact"></div>
        <div class="mk6-about"></div>
        <div class="mk6-body">
          <div class="mk6-col">
            <div class="mk6-col-title"></div>
            <div class="mk-line w30 mt6" style="opacity:.4"></div>
            <div class="mk-line w80 mt3"></div>
            <div class="mk-line w60 mt3" style="background:var(--acc)"></div>
            <div class="mk-line w30 mt6" style="opacity:.4"></div>
            <div class="mk-line w70 mt3"></div>
          </div>
          <div class="mk6-col">
            <div class="mk6-col-title"></div>
            <div class="mk-line w30 mt6" style="opacity:.4"></div>
            <div class="mk-line w80 mt3"></div>
            <div class="mk-label mt8"></div>
            <div class="mk-bar mt4"><div class="mk-fill" style="width:90%"></div></div>
            <div class="mk-bar mt4"><div class="mk-fill" style="width:65%"></div></div>
          </div>
          <div class="mk6-col">
            <div class="mk6-col-title"></div>
            <div class="mk6-skill-line mt6"></div>
            <div class="mk6-skill-line mt4"></div>
            <div class="mk6-skill-line mt4"></div>
          </div>
        </div>
      </div>`
  },
]
</script>

<style scoped>
/* ════════════ WRAPPER ════════════ */
.dashboard-wrapper {
  min-height: 100vh;
  background: linear-gradient(rgba(15, 23, 42, 0.8), rgba(15, 23, 42, 0.8)),
              url('https://images.unsplash.com/photo-1470770841072-f978cf4d019e?q=80&w=2070&auto=format&fit=crop');
  background-size: cover;
  background-position: center;
  background-attachment: fixed;
}

/* ════════════ CARD ════════════ */
.glass-card-template {
  background: rgba(255,255,255,0.05);
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 20px;
  padding: 20px;
  cursor: pointer;
  height: 100%;
  transition: all 0.4s cubic-bezier(0.175,0.885,0.32,1.275);
}
.glass-card-template:hover {
  transform: translateY(-10px);
  background: rgba(255,255,255,0.1);
  border-color: rgba(56,180,105,0.5);
  box-shadow: 0 15px 35px rgba(0,0,0,0.4);
}

.preview-area {
  background: rgba(0,0,0,0.15);
  border-radius: 12px;
  height: 200px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
  overflow: hidden;
  padding: 10px;
}

.cv-mockup {
  width: 100%;
  height: 100%;
  border-radius: 6px;
  overflow: hidden;
}
.mockup-inner { width: 100%; height: 100%; }

/* ════════════ SHARED ATOMS ════════════ */
:deep(.mk) { width: 100%; height: 100%; background: white; border-radius: 6px; overflow: hidden; font-size: 0; }
:deep(.mk-line) { height: 4px; border-radius: 99px; background: #e2e8f0; }
:deep(.mk-label) { height: 5px; width: 45%; border-radius: 3px; background: #cbd5e1; }
:deep(.mk-sep) { height: 1px; background: #e2e8f0; }
:deep(.mk-sep2) { height: 2px; background: var(--acc); }
:deep(.mk-bar) { height: 3px; background: #e2e8f0; border-radius: 99px; }
:deep(.mk-fill) { height: 100%; background: var(--acc); border-radius: 99px; }
:deep(.mk-dot) { width: 7px; height: 7px; border-radius: 50%; background: #e2e8f0; flex-shrink: 0; margin-top: 2px; }
:deep(.mk-dot.themed) { background: var(--acc); }
:deep(.mk-avatar) { width: 26px; height: 26px; border-radius: 50%; background: #e2e8f0; flex-shrink: 0; }
:deep(.mk-avatar.sm) { width: 22px; height: 22px; }
:deep(.mk-name) { height: 8px; width: 70%; border-radius: 3px; background: #1a1a1a; }
:deep(.mk-name-w) { height: 7px; width: 60%; border-radius: 3px; background: rgba(255,255,255,.9); }
:deep(.mk-name-sm) { height: 6px; width: 55%; border-radius: 3px; background: #1a1a1a; }
:deep(.mk-entry) { display: flex; gap: 5px; align-items: flex-start; }
:deep(.mk-datecol) { min-width: 28px; }
:deep(.w30) { width: 30%; } :deep(.w40) { width: 40%; } :deep(.w50) { width: 50%; }
:deep(.w60) { width: 60%; } :deep(.w70) { width: 70%; } :deep(.w80) { width: 80%; }
:deep(.w90) { width: 90%; } :deep(.w100) { width: 100%; }
:deep(.mt0) { margin-top: 0; } :deep(.mt3) { margin-top: 3px; } :deep(.mt4) { margin-top: 4px; }
:deep(.mt6) { margin-top: 6px; } :deep(.mt8) { margin-top: 8px; }
:deep(.ml8) { margin-left: 8px; }

/* ════════════ TEMPLATE 1 — sidebar dark gauche ════════════ */
:deep(.mk1) { --acc: #10b981; display: flex; }
:deep(.mk1 .mk-left) { width: 35%; background: #1a1a1a; padding: 10px 8px; display: flex; flex-direction: column; gap: 0; }
:deep(.mk1 .mk-right) { flex: 1; padding: 10px 8px; display: flex; flex-direction: column; gap: 0; }
:deep(.mk1 .mk-avatar) { width: 40px; height: 40px; border-radius: 6px; background: #333; border: 2px solid var(--acc); }
:deep(.mk1 .mk-label) { background: var(--acc); width: 50%; }
:deep(.mk1 .mk-sep) { background: rgba(255,255,255,.1); }
:deep(.mk1 .mk-line) { background: #444; }
:deep(.mk1 .mk-fill) { background: var(--acc); }
:deep(.mk1 .mk-bar) { background: rgba(255,255,255,.1); }

/* ════════════ TEMPLATE 2 — header coloré ════════════ */
:deep(.mk2) { --acc: #0ea5e9; display: flex; flex-direction: column; background: #fafaf8; }
:deep(.mk2-header) { background: var(--acc); padding: 8px 10px; display: flex; align-items: center; gap: 8px; }
:deep(.mk2 .mk-avatar) { width: 28px; height: 28px; border-radius: 6px; background: rgba(255,255,255,.3); border: 2px solid rgba(255,255,255,.4); }
:deep(.mk2-info) { flex: 1; }
:deep(.mk2-body) { flex: 1; display: flex; gap: 0; padding: 8px 0; }
:deep(.mk2-main) { flex: 1; padding: 0 8px; border-right: 1px solid #e8e8e4; }
:deep(.mk2-side) { width: 80px; padding: 0 8px; }
:deep(.mk2 .mk-label) { background: #1c1c1c; }
:deep(.mk2 .mk-dot.themed) { background: var(--acc); }
:deep(.mk2 .mk-fill) { background: var(--acc); }

/* ════════════ TEMPLATE 3 — crème & serif ════════════ */
:deep(.mk3) { --acc: #10b981; background: #fdf8f2; display: flex; flex-direction: column; }
:deep(.mk3-topband) { height: 4px; background: var(--acc); }
:deep(.mk3-botband) { height: 4px; background: var(--acc); margin-top: auto; }
:deep(.mk3-header) { display: flex; justify-content: space-between; align-items: center; padding: 8px 12px; }
:deep(.mk3-name) { height: 10px; width: 65%; border-radius: 2px; background: #1a110a; }
:deep(.mk3-avatar) { width: 28px; height: 28px; border-radius: 50%; background: #e8e0d5; border: 2px solid var(--acc); }
:deep(.mk3-deco) { display: flex; align-items: center; gap: 4px; padding: 0 12px; margin-bottom: 4px; }
:deep(.mk3-line) { flex: 1; height: 1px; background: var(--acc); opacity: .4; }
:deep(.mk3-diamond) { width: 5px; height: 5px; background: var(--acc); transform: rotate(45deg); }
:deep(.mk3-body) { flex: 1; display: flex; gap: 0; padding: 0 0 4px; }
:deep(.mk3-left) { flex: 3; padding: 0 8px 0 12px; border-right: 1px solid #e2d9ce; }
:deep(.mk3-right) { flex: 2; padding: 0 12px 0 8px; }
:deep(.mk3 .mk-label) { background: var(--acc); width: 50%; }
:deep(.mk3 .mk-fill) { background: var(--acc); }
:deep(.mk3 .mk-bar) { background: #e8e0d5; }
:deep(.mk3 .mk-line) { background: #c8bdb0; }

/* ════════════ TEMPLATE 4 — sidebar colorée droite ════════════ */
:deep(.mk4) { --acc: #6366f1; display: flex; }
:deep(.mk4-main) { flex: 1; padding: 8px 10px; display: flex; flex-direction: column; }
:deep(.mk4-header) { display: flex; align-items: center; gap: 6px; background: #f9fafb; padding: 6px; border-radius: 4px; margin-bottom: 0; }
:deep(.mk4-accent) { height: 2px; background: var(--acc); margin: 4px 0; }
:deep(.mk4-side) { width: 55px; background: var(--acc); padding: 8px 6px; display: flex; flex-direction: column; }
:deep(.mk4-s-avatar) { width: 26px; height: 26px; border-radius: 50%; background: rgba(255,255,255,.3); border: 2px solid rgba(255,255,255,.5); align-self: center; }
:deep(.mk4 .mk-fill) { background: var(--acc); }

/* ════════════ TEMPLATE 5 — dark tech ════════════ */
:deep(.mk5) { --acc: #10b981; background: #0d1117; display: flex; flex-direction: column; }
:deep(.mk5-header) { background: #161b22; padding: 8px 10px; display: flex; align-items: center; justify-content: space-between; gap: 8px; position: relative; border-bottom: 1px solid #30363d; }
:deep(.mk5-glow) { position: absolute; top: -10px; right: -10px; width: 60px; height: 60px; border-radius: 50%; background: radial-gradient(circle, rgba(16,185,129,.15), transparent 70%); }
:deep(.mk5-tag) { height: 5px; width: 50%; border-radius: 2px; background: rgba(16,185,129,.4); border: 1px solid rgba(16,185,129,.3); }
:deep(.mk5-name) { height: 8px; width: 65%; border-radius: 2px; background: #e6edf3; }
:deep(.mk5-avatar) { width: 24px; height: 24px; border-radius: 6px; background: #21262d; border: 1px solid rgba(16,185,129,.4); }
:deep(.mk5-strip) { background: #161b22; padding: 4px 10px; display: flex; border-bottom: 1px solid #21262d; }
:deep(.mk5-body) { flex: 1; display: flex; gap: 0; padding: 6px 0; }
:deep(.mk5-main) { flex: 1; padding: 0 8px; border-right: 1px solid #21262d; }
:deep(.mk5-side) { width: 70px; padding: 0 8px; }
:deep(.mk5-card) { background: #161b22; border: 1px solid #30363d; border-left: 2px solid var(--acc); border-radius: 4px; padding: 5px 6px; }
:deep(.mk5-chip) { height: 12px; border-radius: 4px; background: #161b22; border: 1px solid #30363d; width: 100%; }
:deep(.mk5 .mk-label) { background: #30363d; }
:deep(.mk5 .mk-fill) { background: var(--acc); }
:deep(.mk5 .mk-bar) { background: #21262d; }

/* ════════════ TEMPLATE 6 — magazine éditorial ════════════ */
:deep(.mk6) { --acc: #10b981; background: white; display: flex; flex-direction: column; }
:deep(.mk6-header) { padding: 8px 10px 4px; display: flex; justify-content: space-between; align-items: flex-end; border-top: 2px solid #1a1a1a; }
:deep(.mk6-overtitle) { height: 4px; width: 55%; border-radius: 2px; background: #9ca3af; }
:deep(.mk6-name) { height: 12px; width: 75%; border-radius: 1px; background: #0a0a0a; }
:deep(.mk6-stripe) { height: 3px; width: 100%; background: var(--acc); }
:deep(.mk6-avatar) { width: 24px; height: 24px; border-radius: 3px; background: #f3f4f6; border: 1px solid var(--acc); }
:deep(.mk6-contact) { height: 14px; background: #f9fafb; border-top: 1px solid #e5e7eb; border-bottom: 1px solid #e5e7eb; margin-bottom: 2px; }
:deep(.mk6-about) { height: 8px; background: rgba(16,185,129,.06); border-bottom: 1px solid rgba(16,185,129,.15); margin-bottom: 2px; }
:deep(.mk6-body) { flex: 1; display: grid; grid-template-columns: 1fr 1fr 1fr; border-top: 2px solid #1a1a1a; padding: 6px 0; }
:deep(.mk6-col) { padding: 0 8px; border-right: 1px solid #e5e7eb; display: flex; flex-direction: column; }
:deep(.mk6-col:last-child) { border-right: none; }
:deep(.mk6-col-title) { height: 5px; width: 60%; border-radius: 1px; background: #0a0a0a; border-bottom: 2px solid var(--acc); padding-bottom: 2px; }
:deep(.mk6-skill-line) { height: 3px; background: #f3f4f6; border-radius: 99px; }
:deep(.mk6-skill-line::after) { content: ''; display: block; height: 100%; width: 70%; background: var(--acc); border-radius: 99px; }
:deep(.mk6 .mk-fill) { background: var(--acc); }
:deep(.mk6 .mk-bar) { background: #e5e7eb; }
:deep(.mk6 .mk-label) { background: #9ca3af; }

/* ════════════ BOTTOM UI ════════════ */
.btn-select {
  width: 100%;
  background: transparent;
  border: 1px solid rgba(255,255,255,0.3);
  color: white;
  border-radius: 50px;
  padding: 8px;
  font-size: .9rem;
  transition: .3s;
  cursor: pointer;
}
.glass-card-template:hover .btn-select {
  background: #38b469;
  border-color: #38b469;
}
.profile-pic {
  width: 45px; height: 45px;
  border-radius: 50%;
  object-fit: cover;
  border: 2px solid #38b469;
}
</style>