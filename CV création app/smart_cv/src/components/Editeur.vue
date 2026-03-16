<template>
  <div class="editor-container">

    <nav class="action-bar">
      <button @click="$emit('back')" class="btn-back">
        <i class="fas fa-arrow-left"></i> Dashboard
      </button>
      <div class="completion-bar">
        <div class="completion-track">
          <div class="completion-fill" :style="{ width: completionPercent + '%' }"></div>
        </div>
        <span class="completion-label">{{ completionPercent }}% complété</span>
      </div>
      <div class="share-container">
        <button @click="showShareMenu = !showShareMenu" class="btn-share">
          <i class="fas fa-share-alt"></i> Partager
        </button>
        <div v-if="showShareMenu" class="share-dropdown">
          <button @click="shareTo('whatsapp')" class="share-item wa"><i class="fab fa-whatsapp"></i> WhatsApp</button>
          <button @click="shareTo('telegram')" class="share-item tg"><i class="fab fa-telegram-plane"></i> Telegram</button>
          <div class="divider"></div>
          <button @click="downloadImage" class="share-item"><i class="fas fa-file-image"></i> Image (.png)</button>
          <button @click="downloadPDF" class="share-item"><i class="fas fa-file-pdf"></i> Format PDF</button>
        </div>
      </div>
    </nav>

    <div class="editor-main">

      <button class="toggle-panel-btn" @click="isPanelOpen = !isPanelOpen" :class="{ 'is-closed': !isPanelOpen }">
        <span>{{ isPanelOpen ? '❮' : '❯' }}</span>
      </button>

      <!-- ══ PANNEAU GAUCHE ══ -->
      <aside class="editor-panel" :class="{ collapsed: !isPanelOpen }">
        <div class="panel-inner">

          <!-- APPARENCE -->
          <div class="section-block">
            <button class="section-toggle" @click="toggle('apparence')">
              <span><i class="fas fa-palette"></i> Apparence</span>
              <i class="fas" :class="open.apparence ? 'fa-chevron-up' : 'fa-chevron-down'"></i>
            </button>
            <div v-show="open.apparence" class="section-body">
              <label class="upload-photo-btn">
                <i class="fas fa-camera"></i>
                {{ cv.profile.photo ? 'Changer la photo' : 'Ajouter une photo' }}
                <input type="file" @change="handlePhotoUpload" accept="image/*" />
              </label>
              <button v-if="cv.profile.photo" @click="cv.profile.photo = ''" class="btn-remove small">Supprimer la photo</button>
              <p class="field-label mt-2">Thème de couleur</p>
              <div class="color-presets">
                <button v-for="preset in colorPresets" :key="preset.value" class="color-swatch"
                  :style="{ background: preset.value }" :class="{ active: cv.themeColor === preset.value }"
                  :title="preset.name" @click="cv.themeColor = preset.value"></button>
              </div>
              <div class="color-picker-row">
                <label class="field-label">Couleur personnalisée</label>
                <div class="picker-wrap">
                  <input type="color" v-model="cv.themeColor" class="free-color-input" />
                  <span class="color-hex">{{ cv.themeColor }}</span>
                </div>
              </div>
            </div>
          </div>

          <!-- PROFIL -->
          <div class="section-block">
            <button class="section-toggle" @click="toggle('profil')">
              <span><i class="fas fa-user"></i> Profil & Contact</span>
              <i class="fas" :class="open.profil ? 'fa-chevron-up' : 'fa-chevron-down'"></i>
            </button>
            <div v-show="open.profil" class="section-body">
              <label class="field-label">Nom complet</label>
              <input v-model="cv.profile.name" placeholder="Ex : Marie Dupont" />
              <label class="field-label">Poste / Titre</label>
              <input v-model="cv.profile.job" placeholder="Ex : Développeuse Frontend" />
              <label class="field-label">À propos</label>
              <textarea v-model="cv.profile.about" rows="3" placeholder="Courte description..."></textarea>
              <label class="field-label">Adresse</label>
              <input v-model="cv.profile.address" placeholder="Ex : Paris, France" />
              <label class="field-label">Email</label>
              <input v-model="cv.profile.email" placeholder="exemple@mail.com" />
              <label class="field-label">Téléphone</label>
              <input v-model="cv.profile.phone" placeholder="+33 6 00 00 00 00" />
            </div>
          </div>

          <!-- RÉSEAUX SOCIAUX -->
          <div class="section-block">
            <button class="section-toggle" @click="toggle('reseaux')">
              <span><i class="fas fa-link"></i> Réseaux sociaux</span>
              <i class="fas" :class="open.reseaux ? 'fa-chevron-up' : 'fa-chevron-down'"></i>
            </button>
            <div v-show="open.reseaux" class="section-body">
              <label class="field-label"><i class="fab fa-linkedin"></i> LinkedIn</label>
              <input v-model="cv.socials.linkedin" placeholder="linkedin.com/in/votre-profil" />
              <label class="field-label"><i class="fab fa-github"></i> GitHub</label>
              <input v-model="cv.socials.github" placeholder="github.com/votre-pseudo" />
              <label class="field-label"><i class="fas fa-globe"></i> Portfolio / Site</label>
              <input v-model="cv.socials.website" placeholder="monportfolio.com" />
              <label class="field-label"><i class="fab fa-twitter"></i> Twitter / X</label>
              <input v-model="cv.socials.twitter" placeholder="@monpseudo" />
            </div>
          </div>

          <!-- EXPÉRIENCES -->
          <div class="section-block">
            <button class="section-toggle" @click="toggle('exp')">
              <span><i class="fas fa-briefcase"></i> Expériences</span>
              <i class="fas" :class="open.exp ? 'fa-chevron-up' : 'fa-chevron-down'"></i>
            </button>
            <div v-show="open.exp" class="section-body">
              <div v-for="(exp, i) in cv.experiences" :key="exp.id" class="item-card">
                <div class="item-card-header">
                  <span class="item-card-title">{{ exp.company || 'Nouvelle expérience' }}</span>
                  <button @click="cv.experiences.splice(i, 1)" class="btn-icon-remove"><i class="fas fa-trash"></i></button>
                </div>
                <label class="field-label">Période</label>
                <input v-model="exp.date" placeholder="Ex : 2022 – 2024" />
                <label class="field-label">Entreprise</label>
                <input v-model="exp.company" placeholder="Nom de l'entreprise" />
                <label class="field-label">Poste</label>
                <input v-model="exp.title" placeholder="Titre du poste" />
                <label class="field-label">Missions</label>
                <textarea v-model="exp.description" rows="3" placeholder="Décrivez vos missions clés..."></textarea>
              </div>
              <button @click="addExperience" class="btn-add-section">
                <i class="fas fa-plus"></i> Ajouter une expérience
              </button>
            </div>
          </div>

          <!-- FORMATION -->
          <div class="section-block">
            <button class="section-toggle" @click="toggle('edu')">
              <span><i class="fas fa-graduation-cap"></i> Formation</span>
              <i class="fas" :class="open.edu ? 'fa-chevron-up' : 'fa-chevron-down'"></i>
            </button>
            <div v-show="open.edu" class="section-body">
              <div v-for="(edu, i) in cv.education" :key="edu.id" class="item-card">
                <div class="item-card-header">
                  <span class="item-card-title">{{ edu.school || 'Nouvelle formation' }}</span>
                  <button @click="cv.education.splice(i, 1)" class="btn-icon-remove"><i class="fas fa-trash"></i></button>
                </div>
                <label class="field-label">Année</label>
                <input v-model="edu.year" placeholder="Ex : 2020 – 2023" />
                <label class="field-label">Établissement</label>
                <input v-model="edu.school" placeholder="Nom de l'école / université" />
                <label class="field-label">Diplôme</label>
                <input v-model="edu.title" placeholder="Ex : Master Informatique" />
              </div>
              <button @click="addEducation" class="btn-add-section">
                <i class="fas fa-plus"></i> Ajouter une formation
              </button>
            </div>
          </div>

          <!-- COMPÉTENCES -->
          <div class="section-block">
            <button class="section-toggle" @click="toggle('skills')">
              <span><i class="fas fa-tools"></i> Compétences</span>
              <i class="fas" :class="open.skills ? 'fa-chevron-up' : 'fa-chevron-down'"></i>
            </button>
            <div v-show="open.skills" class="section-body">
              <div v-for="(skill, i) in cv.skills" :key="i" class="skill-row">
                <input v-model="skill.name" placeholder="Ex : Vue.js" class="skill-name-input" />
                <select v-model="skill.level" class="skill-level-select">
                  <option value="Débutant">Débutant</option>
                  <option value="Intermédiaire">Intermédiaire</option>
                  <option value="Avancé">Avancé</option>
                  <option value="Expert">Expert</option>
                </select>
                <button @click="cv.skills.splice(i, 1)" class="btn-icon-remove"><i class="fas fa-times"></i></button>
              </div>
              <button @click="cv.skills.push({ name: '', level: 'Intermédiaire' })" class="btn-add-section">
                <i class="fas fa-plus"></i> Ajouter une compétence
              </button>
            </div>
          </div>

          <!-- LANGUES -->
          <div class="section-block">
            <button class="section-toggle" @click="toggle('langues')">
              <span><i class="fas fa-language"></i> Langues</span>
              <i class="fas" :class="open.langues ? 'fa-chevron-up' : 'fa-chevron-down'"></i>
            </button>
            <div v-show="open.langues" class="section-body">
              <div v-for="(lang, i) in cv.languages" :key="i" class="skill-row">
                <input v-model="lang.name" placeholder="Ex : Anglais" class="skill-name-input" />
                <select v-model="lang.level" class="skill-level-select">
                  <option value="Notions">Notions</option>
                  <option value="Intermédiaire">Intermédiaire</option>
                  <option value="Courant">Courant</option>
                  <option value="Bilingue">Bilingue</option>
                  <option value="Natif">Natif</option>
                </select>
                <button @click="cv.languages.splice(i, 1)" class="btn-icon-remove"><i class="fas fa-times"></i></button>
              </div>
              <button @click="cv.languages.push({ name: '', level: 'Courant' })" class="btn-add-section">
                <i class="fas fa-plus"></i> Ajouter une langue
              </button>
            </div>
          </div>

          <!-- CENTRES D'INTÉRÊT -->
          <div class="section-block">
            <button class="section-toggle" @click="toggle('interests')">
              <span><i class="fas fa-heart"></i> Centres d'intérêt</span>
              <i class="fas" :class="open.interests ? 'fa-chevron-up' : 'fa-chevron-down'"></i>
            </button>
            <div v-show="open.interests" class="section-body">
              <div v-for="(item, i) in cv.interests" :key="i" class="skill-row">
                <input v-model="cv.interests[i]" placeholder="Ex : Photographie" class="skill-name-input full" />
                <button @click="cv.interests.splice(i, 1)" class="btn-icon-remove"><i class="fas fa-times"></i></button>
              </div>
              <button @click="cv.interests.push('')" class="btn-add-section">
                <i class="fas fa-plus"></i> Ajouter un intérêt
              </button>
            </div>
          </div>

        </div><!-- /panel-inner -->
      </aside>

      <!-- ══ PRÉVISUALISATION ══ -->
      <div class="cv-preview">
        <div class="cv-page-wrapper" id="cv-to-print">
          <component :is="currentTemplateComponent" :cv="cv" />
        </div>
      </div>

    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import html2pdf from 'html2pdf.js'
import html2canvas from 'html2canvas'

import TemplateCV1 from '@/components/TemplateCV1.vue'
import TemplateCV2 from '@/components/TemplateCV2.vue'
import TemplateCV3 from '@/components/TemplateCV3.vue'
import TemplateCV4 from '@/components/TemplateCV4.vue'
import TemplateCV5 from '@/components/TemplateCV5.vue'
import TemplateCV6 from '@/components/TemplateCV6.vue'

const props = defineProps({
  user:       { type: Object, default: null },
  templateId: { type: String, required: true }
})
const emit = defineEmits(['back'])

const isPanelOpen   = ref(true)
const showShareMenu = ref(false)

const open = ref({
  apparence: true, profil: true, reseaux: false,
  exp: false, edu: false, skills: false, langues: false, interests: false,
})
const toggle = key => { open.value[key] = !open.value[key] }

const colorPresets = [
  { name: 'Émeraude',   value: '#10b981' },
  { name: 'Océan',      value: '#0ea5e9' },
  { name: 'Nuit',       value: '#6366f1' },
  { name: 'Corail',     value: '#f43f5e' },
  { name: 'Ambre',      value: '#f59e0b' },
  { name: 'Ardoise',    value: '#475569' },
  { name: 'Prune',      value: '#a855f7' },
  { name: 'Terracotta', value: '#ea580c' },
]

const baseData = () => ({
  themeColor: '#10b981',
  profile: {
    name: 'Rodney Odimba', job: 'Développeur Fullstack',
    about: 'Développeur Vue.js passionné par les interfaces modernes et les architectures scalables.',
    email: 'rodney@email.com', phone: '+33 6 00 00 00 00', address: 'Paris, France', photo: '',
  },
  socials: { linkedin: '', github: '', website: '', twitter: '' },
  experiences: [
    { id: 1, date: '2023 – Présent', company: 'Acme Corp',       title: 'Développeur Frontend',   description: "Développement de composants Vue 3, intégration d'API REST, optimisation des performances." },
    { id: 2, date: '2021 – 2023',   company: 'StartupXYZ',      title: 'Développeur Fullstack',  description: "Conception d'une application SaaS en Vue.js et Node.js, gestion de base de données PostgreSQL." },
    { id: 3, date: '2020 – 2021',   company: 'Agence Digitale',  title: 'Développeur Web Junior', description: 'Intégration de maquettes et développement de sites vitrines sous WordPress et React.' },
  ],
  education: [
    { id: 1, year: '2020 – 2023', school: 'Université Paris Saclay', title: 'Master Informatique' },
    { id: 2, year: '2017 – 2020', school: 'IUT de Paris',            title: 'Licence Pro Développement Web' },
  ],
  skills: [
    { name: 'Vue.js', level: 'Expert' }, { name: 'JavaScript', level: 'Expert' },
    { name: 'Python',  level: 'Avancé' }, { name: 'TailwindCSS', level: 'Avancé' },
  ],
  languages: [{ name: 'Français', level: 'Natif' }, { name: 'Anglais', level: 'Courant' }],
  interests: ['Photographie', 'Randonnée', 'Open Source'],
})

const cv = ref(baseData())
watch(() => props.templateId, () => { cv.value = baseData() }, { immediate: true })

// ── IDs IDENTIQUES AU DASHBOARD ──
const templateMap = { TemplateCV1, TemplateCV2, TemplateCV3, TemplateCV4, TemplateCV5, TemplateCV6 }
const currentTemplateComponent = computed(() => templateMap[props.templateId] ?? TemplateCV1)

const completionPercent = computed(() => {
  const checks = [
    cv.value.profile.name, cv.value.profile.job, cv.value.profile.about,
    cv.value.profile.email, cv.value.profile.phone, cv.value.profile.address,
    cv.value.profile.photo,
    cv.value.socials.linkedin || cv.value.socials.github || cv.value.socials.website,
    cv.value.experiences.length > 0, cv.value.education.length > 0,
    cv.value.skills.length > 0, cv.value.languages.length > 0, cv.value.interests.length > 0,
  ]
  return Math.round((checks.filter(Boolean).length / checks.length) * 100)
})

const addExperience = () => cv.value.experiences.push({ id: Date.now(), date: '', company: '', title: '', description: '' })
const addEducation  = () => cv.value.education.push({ id: Date.now(), year: '', school: '', title: '' })

const handlePhotoUpload = e => {
  const file = e.target.files[0]; if (!file) return
  const reader = new FileReader()
  reader.onload = ev => { cv.value.profile.photo = ev.target.result }
  reader.readAsDataURL(file)
}

const shareTo = platform => console.log('share', platform)

const downloadPDF = () => {
  html2pdf().set({ margin: 0, filename: 'cv.pdf', html2canvas: { scale: 2 }, jsPDF: { unit: 'mm', format: 'a4' } })
    .from(document.getElementById('cv-to-print')).save()
}
const downloadImage = async () => {
  const canvas = await html2canvas(document.getElementById('cv-to-print'), { scale: 2 })
  const a = document.createElement('a'); a.download = 'cv.png'; a.href = canvas.toDataURL(); a.click()
}
</script>

<style scoped>
/* ════════ LAYOUT ════════ */
.editor-container {
  display: flex; flex-direction: column;
  height: 100vh; background: #111318;
  color: #e2e8f0; overflow: hidden;
  font-family: 'Segoe UI', sans-serif;
}
.editor-main { display: flex; flex: 1; overflow: hidden; position: relative; }

/* ════════ ACTION BAR ════════ */
.action-bar {
  display: flex; align-items: center; gap: 16px;
  padding: 12px 20px; background: #1a1d24;
  border-bottom: 1px solid #2a2d35;
  box-shadow: 0 2px 16px rgba(0,0,0,.4); z-index: 200;
}
.btn-back {
  background: linear-gradient(135deg,#10b981,#059669); color: white; border: none;
  padding: 10px 18px; border-radius: 10px; font-weight: 600; font-size: 13px;
  cursor: pointer; display: flex; align-items: center; gap: 7px;
  transition: all .25s; box-shadow: 0 3px 12px rgba(16,185,129,.35); white-space: nowrap;
}
.btn-back:hover { transform: translateY(-1px); box-shadow: 0 6px 20px rgba(16,185,129,.5); }

.completion-bar { flex: 1; display: flex; align-items: center; gap: 10px; max-width: 380px; }
.completion-track { flex: 1; height: 6px; background: #2a2d35; border-radius: 99px; overflow: hidden; }
.completion-fill { height: 100%; background: linear-gradient(90deg,#10b981,#34d399); border-radius: 99px; transition: width .5s cubic-bezier(.4,0,.2,1); }
.completion-label { font-size: 12px; color: #64748b; white-space: nowrap; }

.share-container { position: relative; }
.btn-share {
  background: linear-gradient(135deg,#34d399,#10b981); color: white; border: none;
  padding: 10px 20px; border-radius: 10px; font-weight: 600; font-size: 13px;
  cursor: pointer; display: flex; align-items: center; gap: 7px;
  transition: all .25s; box-shadow: 0 3px 12px rgba(52,211,153,.35);
}
.btn-share:hover { transform: translateY(-1px); box-shadow: 0 6px 20px rgba(52,211,153,.5); }
.share-dropdown {
  position: absolute; top: calc(100% + 8px); right: 0;
  background: #1e2028; border: 1px solid #2a2d35; border-radius: 14px;
  padding: 10px; min-width: 210px; box-shadow: 0 20px 40px rgba(0,0,0,.5); z-index: 300;
}
.share-item {
  width: 100%; background: transparent; border: none; padding: 10px 14px;
  border-radius: 8px; font-size: 13px; font-weight: 500; cursor: pointer;
  display: flex; align-items: center; gap: 10px; color: #e2e8f0; transition: all .2s; margin-bottom: 3px;
}
.share-item:hover { background: rgba(255,255,255,.07); transform: translateX(3px); }
.share-item.wa { background: linear-gradient(135deg,#25D366,#128C7E); }
.share-item.tg { background: linear-gradient(135deg,#0088cc,#229ED9); }
.divider { height: 1px; background: #2a2d35; margin: 6px 0; }

/*  TOGGLE BTN  */
.toggle-panel-btn {
  position: absolute; left: 340px; top: 50%; transform: translateY(-50%);
  background: #10b981; border: none; width: 24px; height: 56px;
  border-radius: 0 10px 10px 0; cursor: pointer; color: white;
  z-index: 100; transition: left .4s cubic-bezier(.4,0,.2,1); font-size: 11px;
}
.toggle-panel-btn.is-closed { left: 0; }

/*  PANEL — SCROLLBAR  */
.editor-panel {
  width: 340px;
  background: #1a1d24;
  border-right: 1px solid #2a2d35;
  overflow-y: auto;           /* ← scroll activé sur ce conteneur */
  flex-shrink: 0;
  height: 100%;
  transition: width .4s cubic-bezier(.4,0,.2,1), opacity .3s;
  /* ↓ scrollbar stylée sur le MÊME élément que overflow-y */
  scrollbar-width: thin;
  scrollbar-color: #10b981 #1a1d24;
}
.editor-panel::-webkit-scrollbar { width: 6px; }
.editor-panel::-webkit-scrollbar-track { background: #1a1d24; }
.editor-panel::-webkit-scrollbar-thumb { background: #10b981; border-radius: 99px; }

.editor-panel.collapsed { width: 0; opacity: 0; overflow: hidden; }

/* panel-inner : pas de overflow, pas de height fixe — il grandit librement */
.panel-inner {
  width: 340px;
  padding: 16px;
  display: flex;
  flex-direction: column;
  gap: 6px;
}

/* ════════ SECTION BLOCKS ════════ */
.section-block { background: #1e2028; border: 1px solid #2a2d35; border-radius: 12px; overflow: hidden; }
.section-toggle {
  width: 100%; background: transparent; border: none; padding: 13px 16px;
  color: #e2e8f0; display: flex; align-items: center; justify-content: space-between;
  cursor: pointer; font-size: 13px; font-weight: 600; letter-spacing: .03em; transition: background .2s;
}
.section-toggle span { display: flex; align-items: center; gap: 8px; }
.section-toggle span i { color: #10b981; font-size: 13px; }
.section-toggle:hover { background: rgba(255,255,255,.04); }
.section-toggle .fa-chevron-up, .section-toggle .fa-chevron-down { color: #475569; font-size: 11px; }
.section-body { padding: 0 16px 14px; display: flex; flex-direction: column; gap: 6px; }

/* ════════ FIELDS ════════ */
.field-label {
  font-size: 11px; font-weight: 600; color: #64748b;
  text-transform: uppercase; letter-spacing: .06em;
  margin-bottom: 2px; margin-top: 8px; display: block;
}
.field-label:first-child { margin-top: 0; }
.mt-2 { margin-top: 10px !important; }

.section-body input, .section-body textarea, .section-body select {
  width: 100%; background: #111318; border: 1px solid #2a2d35;
  border-radius: 8px; color: #e2e8f0; padding: 9px 11px;
  font-size: 13px; transition: border-color .2s, box-shadow .2s; outline: none; resize: vertical;
}
.section-body input:focus, .section-body textarea:focus, .section-body select:focus {
  border-color: #10b981; box-shadow: 0 0 0 3px rgba(16,185,129,.15);
}
.section-body select option { background: #1a1d24; }

/* ════════ PHOTO ════════ */
.upload-photo-btn {
  display: flex; align-items: center; justify-content: center; gap: 8px;
  background: rgba(16,185,129,.1); border: 1px dashed #10b981;
  border-radius: 8px; padding: 10px; font-size: 13px; font-weight: 600;
  color: #10b981; cursor: pointer; transition: background .2s;
}
.upload-photo-btn:hover { background: rgba(16,185,129,.18); }
.upload-photo-btn input { display: none; }

/* ════════ COLORS ════════ */
.color-presets { display: flex; flex-wrap: wrap; gap: 8px; margin: 4px 0; }
.color-swatch { width: 28px; height: 28px; border-radius: 50%; border: 3px solid transparent; cursor: pointer; transition: transform .2s, border-color .2s; outline: none; }
.color-swatch:hover { transform: scale(1.15); }
.color-swatch.active { border-color: white; transform: scale(1.15); }
.color-picker-row { display: flex; flex-direction: column; gap: 4px; }
.picker-wrap { display: flex; align-items: center; gap: 10px; }
.free-color-input { width: 44px !important; height: 36px !important; padding: 2px !important; border-radius: 8px !important; cursor: pointer; background: #111318 !important; }
.color-hex { font-size: 12px; color: #64748b; font-family: monospace; }

/* ════════ ITEM CARDS ════════ */
.item-card {
  background: #111318; border: 1px solid #2a2d35; border-left: 3px solid #10b981;
  border-radius: 8px; padding: 12px; margin-bottom: 8px; display: flex; flex-direction: column; gap: 4px;
}
.item-card-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 4px; }
.item-card-title { font-size: 12px; font-weight: 700; color: #94a3b8; white-space: nowrap; overflow: hidden; text-overflow: ellipsis; max-width: 200px; }

/* ════════ SKILL ROWS ════════ */
.skill-row { display: flex; gap: 6px; align-items: center; margin-bottom: 6px; }
.skill-name-input { flex: 1; }
.skill-level-select { width: 115px !important; flex-shrink: 0; }

/* ════════ BUTTONS ════════ */
.btn-add-section {
  width: 100%; background: transparent; border: 1px dashed #2a2d35;
  border-radius: 8px; color: #64748b; padding: 9px; font-size: 12px; font-weight: 600;
  cursor: pointer; transition: all .2s; display: flex; align-items: center; justify-content: center; gap: 6px;
}
.btn-add-section:hover { border-color: #10b981; color: #10b981; background: rgba(16,185,129,.06); }
.btn-remove { background: rgba(239,68,68,.1); border: 1px solid rgba(239,68,68,.3); border-radius: 6px; color: #f87171; padding: 7px 12px; font-size: 12px; cursor: pointer; transition: .2s; }
.btn-remove:hover { background: rgba(239,68,68,.2); }
.btn-remove.small { font-size: 11px; padding: 5px 10px; }
.btn-icon-remove { background: transparent; border: none; color: #475569; cursor: pointer; padding: 4px 6px; border-radius: 5px; font-size: 12px; transition: color .2s, background .2s; flex-shrink: 0; }
.btn-icon-remove:hover { color: #f87171; background: rgba(239,68,68,.1); }

/* ════════ PREVIEW ════════ */
.cv-preview {
  flex: 1; background: #252830;
  display: flex; justify-content: center; align-items: flex-start;
  padding: 40px; overflow-y: auto;
}
.cv-page-wrapper {
  width: 210mm; background: white;
  box-shadow: 0 24px 60px rgba(0,0,0,.6);
  transform: scale(0.75); transform-origin: top center;
  margin-bottom: -100px;
}
</style>