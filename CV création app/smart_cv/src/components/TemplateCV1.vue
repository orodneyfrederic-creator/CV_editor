<template>
  <div class="cv-garcia" :style="{ '--theme': cv.themeColor || '#10b981' }">

    <!-- ══ COLONNE GAUCHE ══ -->
    <div class="left-column">

      <!-- Photo -->
      <div class="photo-wrapper">
        <img v-if="cv.profile.photo" :src="cv.profile.photo" class="photo-render" alt="Photo de profil" />
        <div v-else class="photo-placeholder">
          <i class="fas fa-user"></i>
        </div>
      </div>

      <!-- Contact -->
      <section class="block">
        <h2 class="block-title">Contact</h2>
        <ul class="contact-list">
          <li v-if="cv.profile.address">
            <span class="contact-icon">Adresse: </span> {{ cv.profile.address }}
          </li>
          <li v-if="cv.profile.email">
            <span class="contact-icon">Email: </span> {{ cv.profile.email }}
          </li>
          <li v-if="cv.profile.phone">
            <span class="contact-icon"> Téléphone: </span> {{ cv.profile.phone }}
          </li>
        </ul>
      </section>

      <!-- Réseaux sociaux -->
      <section class="block" v-if="cv.socials && (cv.socials.linkedin || cv.socials.github || cv.socials.website || cv.socials.twitter)">
        <h2 class="block-title">Réseaux</h2>
        <ul class="contact-list">
          <li v-if="cv.socials.linkedin">
            <span class="contact-icon">in</span> {{ cv.socials.linkedin }}
          </li>
          <li v-if="cv.socials.github">
            <span class="contact-icon">⌥</span> {{ cv.socials.github }}
          </li>
          <li v-if="cv.socials.website">
            <span class="contact-icon">🌐</span> {{ cv.socials.website }}
          </li>
          <li v-if="cv.socials.twitter">
            <span class="contact-icon">𝕏</span> {{ cv.socials.twitter }}
          </li>
        </ul>
      </section>

      <!-- Compétences -->
      <section class="block" v-if="cv.skills && cv.skills.length">
        <h2 class="block-title">Compétences</h2>
        <div v-for="skill in cv.skills" :key="skill.name" class="skill-item">
          <div class="skill-header">
            <span class="skill-name">{{ skill.name }}</span>
            <span class="skill-level-tag">{{ skill.level }}</span>
          </div>
          <div class="skill-bar-track">
            <div
              class="skill-bar-fill"
              :style="{ width: skillLevelToPercent(skill.level) + '%' }"
            ></div>
          </div>
        </div>
      </section>

      <!-- Langues -->
      <section class="block" v-if="cv.languages && cv.languages.length">
        <h2 class="block-title">Langues</h2>
        <div v-for="lang in cv.languages" :key="lang.name" class="skill-item">
          <div class="skill-header">
            <span class="skill-name">{{ lang.name }}</span>
            <span class="skill-level-tag">{{ lang.level }}</span>
          </div>
        </div>
      </section>

      <!-- Centres d'intérêt -->
      <section class="block" v-if="cv.interests && cv.interests.length">
        <h2 class="block-title">Centres d'intérêt</h2>
        <div class="interests-wrap">
          <span
            v-for="interest in cv.interests"
            :key="interest"
            class="interest-tag"
          >{{ interest }}</span>
        </div>
      </section>

    </div>

    <!-- ══ COLONNE DROITE ══ -->
    <div class="right-column">

      <!-- Header -->
      <header class="cv-header">
        <h1 class="name">{{ cv.profile.name }}</h1>
        <div class="role">{{ cv.profile.job }}</div>
      </header>

      <!-- Profil / À propos -->
      <section class="section" v-if="cv.profile.about">
        <h2 class="section-title">Profil</h2>
        <p class="about-text">{{ cv.profile.about }}</p>
      </section>

      <!-- Expériences -->
      <section class="section" v-if="cv.experiences && cv.experiences.length">
        <h2 class="section-title">Expériences</h2>
        <div v-for="exp in cv.experiences" :key="exp.id" class="exp-item">
          <div class="exp-date">{{ exp.date }}</div>
          <div class="exp-content">
            <div class="exp-job">{{ exp.title }}</div>
            <div class="exp-company">{{ exp.company }}</div>
            <p class="exp-desc">{{ exp.description }}</p>
          </div>
        </div>
      </section>

      <!-- Formation -->
      <section class="section" v-if="cv.education && cv.education.length">
        <h2 class="section-title">Formation</h2>
        <div v-for="edu in cv.education" :key="edu.id" class="exp-item">
          <div class="exp-date">{{ edu.year }}</div>
          <div class="exp-content">
            <div class="exp-job">{{ edu.title }}</div>
            <div class="exp-company">{{ edu.school }}</div>
          </div>
        </div>
      </section>

    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  cv: { type: Object, required: true }
})

const skillLevelToPercent = (level) => {
  const map = { 'Débutant': 25, 'Intermédiaire': 50, 'Avancé': 75, 'Expert': 100,
                'Notions': 20, 'Courant': 65, 'Bilingue': 85, 'Natif': 100 }
  return map[level] ?? 50
}
</script>

<style scoped>
/* ── Variable de thème injectée depuis :style ── */
.cv-garcia {
  --theme: #10b981; /* fallback, écrasé dynamiquement */

  width: 210mm;
  min-height: 297mm;
  display: grid;
  grid-template-columns: 32% 68%;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: white;
  text-align: left;
}

/* ════════════════════
   COLONNE GAUCHE
════════════════════ */
.left-column {
  background: #1a1a1a;
  color: white;
  padding: 28px 18px;
  display: flex;
  flex-direction: column;
  gap: 22px;
}

/* Photo */
.photo-wrapper {
  width: 100%;
  aspect-ratio: 1 / 1;
  background: #2a2a2a;
  border-radius: 8px;
  overflow: hidden;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 3px solid var(--theme);
}
.photo-render {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.photo-placeholder {
  font-size: 48px;
  color: #444;
}

/* Block générique */
.block { display: flex; flex-direction: column; gap: 8px; }

.block-title {
  font-size: 11px;
  font-weight: 800;
  letter-spacing: .12em;
  text-transform: uppercase;
  color: var(--theme);
  border-bottom: 1px solid rgba(255,255,255,0.08);
  padding-bottom: 5px;
  margin: 0;
}

/* Contact */
.contact-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 6px;
}
.contact-list li {
  font-size: 12px;
  color: #ccc;
  display: flex;
  align-items: flex-start;
  gap: 6px;
  word-break: break-all;
}
.contact-icon {
  font-size: 12px;
  flex-shrink: 0;
  margin-top: 1px;
  font-style: normal;
  font-weight: 700;
  color: var(--theme);
}

/* Compétences */
.skill-item { display: flex; flex-direction: column; gap: 3px; margin-bottom: 6px; }
.skill-header { display: flex; justify-content: space-between; align-items: center; }
.skill-name { font-size: 12px; color: #e2e8f0; font-weight: 600; }
.skill-level-tag { font-size: 10px; color: var(--theme); font-weight: 700; }
.skill-bar-track {
  height: 4px;
  background: rgba(255,255,255,0.1);
  border-radius: 99px;
  overflow: hidden;
}
.skill-bar-fill {
  height: 100%;
  background: var(--theme);
  border-radius: 99px;
  transition: width 0.5s ease;
}

/* Intérêts */
.interests-wrap { display: flex; flex-wrap: wrap; gap: 5px; }
.interest-tag {
  background: rgba(255,255,255,0.07);
  border: 1px solid rgba(255,255,255,0.12);
  border-radius: 99px;
  padding: 3px 10px;
  font-size: 11px;
  color: #ccc;
}

/* ════════════════════
   COLONNE DROITE
════════════════════ */
.right-column {
  background: white;
  padding: 36px 32px;
  color: #1a1a1a;
  display: flex;
  flex-direction: column;
  gap: 24px;
}

/* Header */
.cv-header { display: flex; flex-direction: column; gap: 4px; }
.name {
  font-size: 30px;
  font-weight: 800;
  margin: 0;
  color: #1a1a1a;
  line-height: 1.1;
}
.role {
  font-size: 14px;
  color: var(--theme);
  text-transform: uppercase;
  font-weight: 700;
  letter-spacing: .08em;
}

/* Sections */
.section { display: flex; flex-direction: column; gap: 12px; }
.section-title {
  font-size: 13px;
  font-weight: 800;
  text-transform: uppercase;
  letter-spacing: .1em;
  color: #1a1a1a;
  border-bottom: 2px solid var(--theme);
  padding-bottom: 4px;
  margin: 0;
}

/* About */
.about-text { font-size: 13px; color: #555; line-height: 1.65; margin: 0; }

/* Expériences / Formation */
.exp-item {
  display: flex;
  gap: 16px;
}
.exp-date {
  min-width: 90px;
  font-size: 11px;
  color: #999;
  padding-top: 3px;
  font-style: italic;
}
.exp-content { display: flex; flex-direction: column; gap: 2px; }
.exp-job { font-weight: 700; font-size: 14px; color: #1a1a1a; }
.exp-company { color: var(--theme); font-size: 13px; font-weight: 600; }
.exp-desc { font-size: 12px; color: #666; line-height: 1.55; margin: 4px 0 0; white-space: pre-line; }
</style>