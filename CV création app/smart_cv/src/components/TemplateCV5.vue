<template>
  <div class="cv-pulse" :style="{ '--theme': cv.themeColor || '#10b981' }">

    <!-- ══ HEADER ASYMÉTRIQUE ══ -->
    <header class="cv-header">
      <div class="header-bg"></div>
      <div class="header-content">
        <div class="hc-left">
          <div class="name-tag">{{ cv.profile.job }}</div>
          <h1 class="name">{{ cv.profile.name }}</h1>
          <p class="about" v-if="cv.profile.about">{{ cv.profile.about }}</p>
        </div>
        <div class="hc-right">
          <div class="photo-hex">
            <img v-if="cv.profile.photo" :src="cv.profile.photo" class="photo" alt="Photo" />
            <div v-else class="photo-ph"><i class="fas fa-user"></i></div>
          </div>
        </div>
      </div>
      <div class="contact-strip">
        <span v-if="cv.profile.email" class="cs-item"><i class="fas fa-envelope"></i> {{ cv.profile.email }}</span>
        <span v-if="cv.profile.phone" class="cs-item"><i class="fas fa-phone"></i> {{ cv.profile.phone }}</span>
        <span v-if="cv.profile.address" class="cs-item"><i class="fas fa-map-marker-alt"></i> {{ cv.profile.address }}</span>
        <span v-if="cv.socials?.linkedin" class="cs-item"><i class="fab fa-linkedin"></i> {{ cv.socials.linkedin }}</span>
        <span v-if="cv.socials?.github" class="cs-item"><i class="fab fa-github"></i> {{ cv.socials.github }}</span>
      </div>
    </header>

    <!-- ══ BODY ══ -->
    <div class="cv-body">
      <div class="col-main">

        <section v-if="cv.experiences?.length" class="section">
          <h2 class="section-title">
            <span class="st-badge">01</span> Expériences
          </h2>
          <div v-for="exp in cv.experiences" :key="exp.id" class="exp-card">
            <div class="ec-header">
              <div>
                <div class="ec-title">{{ exp.title }}</div>
                <div class="ec-company">@ {{ exp.company }}</div>
              </div>
              <div class="ec-date">{{ exp.date }}</div>
            </div>
            <p class="ec-desc">{{ exp.description }}</p>
          </div>
        </section>

        <section v-if="cv.education?.length" class="section">
          <h2 class="section-title">
            <span class="st-badge">02</span> Formation
          </h2>
          <div v-for="edu in cv.education" :key="edu.id" class="exp-card flat">
            <div class="ec-header">
              <div>
                <div class="ec-title">{{ edu.title }}</div>
                <div class="ec-company">@ {{ edu.school }}</div>
              </div>
              <div class="ec-date">{{ edu.year }}</div>
            </div>
          </div>
        </section>

      </div>

      <div class="col-side">

        <section v-if="cv.skills?.length" class="section">
          <h2 class="section-title side">
            <span class="st-badge">03</span> Skills
          </h2>
          <div class="skills-grid">
            <div v-for="skill in cv.skills" :key="skill.name"
              class="skill-chip"
              :class="skill.level.toLowerCase().replace('é','e').replace('é','e')">
              <span class="chip-name">{{ skill.name }}</span>
              <span class="chip-level">{{ skill.level }}</span>
            </div>
          </div>
        </section>

        <section v-if="cv.languages?.length" class="section">
          <h2 class="section-title side">
            <span class="st-badge">04</span> Langues
          </h2>
          <div v-for="lang in cv.languages" :key="lang.name" class="lang-item">
            <span class="li-name">{{ lang.name }}</span>
            <div class="li-bar">
              <div class="li-fill" :style="{ width: ltp(lang.level) + '%' }"></div>
            </div>
            <span class="li-level">{{ lang.level }}</span>
          </div>
        </section>

        <section v-if="cv.interests?.length" class="section">
          <h2 class="section-title side">
            <span class="st-badge">05</span> Intérêts
          </h2>
          <div class="tags-wrap">
            <span v-for="i in cv.interests" :key="i" class="i-tag">{{ i }}</span>
          </div>
        </section>

      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({ cv: { type: Object, required: true } })
const ltp = (l) => ({ 'Débutant': 25, 'Intermédiaire': 50, 'Avancé': 75, 'Expert': 100, 'Notions': 20, 'Courant': 65, 'Bilingue': 85, 'Natif': 100 }[l] ?? 50)
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&family=Sora:wght@300;400;600;700;800&display=swap');

.cv-pulse {
  --theme: #10b981;
  width: 210mm;
  min-height: 297mm;
  background: #0d1117;
  font-family: 'Sora', sans-serif;
  color: #e6edf3;
}

/* HEADER */
.cv-header {
  position: relative;
  background: #161b22;
  border-bottom: 1px solid #30363d;
  overflow: hidden;
}
.header-bg {
  position: absolute;
  top: -40px; right: -40px;
  width: 220px; height: 220px;
  border-radius: 50%;
  background: radial-gradient(circle, color-mix(in srgb, var(--theme) 20%, transparent), transparent 70%);
  pointer-events: none;
}
.header-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 28px 28px 12px;
}
.hc-left { display: flex; flex-direction: column; gap: 5px; max-width: 340px; }
.name-tag {
  display: inline-block;
  background: color-mix(in srgb, var(--theme) 15%, transparent);
  border: 1px solid color-mix(in srgb, var(--theme) 40%, transparent);
  color: var(--theme);
  font-size: 10px;
  font-weight: 700;
  letter-spacing: .14em;
  text-transform: uppercase;
  padding: 3px 10px;
  border-radius: 4px;
  width: fit-content;
}
.name {
  font-size: 30px;
  font-weight: 800;
  margin: 0;
  color: #e6edf3;
  line-height: 1.05;
}
.about { font-size: 12px; color: #8b949e; line-height: 1.6; margin: 2px 0 0; }

.photo-hex {
  width: 80px; height: 80px;
  border-radius: 12px;
  overflow: hidden;
  border: 2px solid color-mix(in srgb, var(--theme) 50%, transparent);
  background: #21262d;
  display: flex; align-items: center; justify-content: center;
  flex-shrink: 0;
}
.photo { width: 100%; height: 100%; object-fit: cover; }
.photo-ph { font-size: 30px; color: #484f58; }

.contact-strip {
  display: flex;
  flex-wrap: wrap;
  gap: 6px 16px;
  padding: 10px 28px 16px;
  border-top: 1px solid #21262d;
}
.cs-item {
  font-size: 11px;
  color: #8b949e;
  display: flex;
  align-items: center;
  gap: 5px;
}
.cs-item i { color: var(--theme); font-size: 10px; }

/* BODY */
.cv-body {
  display: grid;
  grid-template-columns: 1fr 185px;
  min-height: calc(297mm - 120px);
}
.col-main {
  padding: 22px 24px;
  border-right: 1px solid #21262d;
  display: flex;
  flex-direction: column;
  gap: 20px;
}
.col-side {
  padding: 22px 16px;
  background: #0d1117;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

/* SECTIONS */
.section { display: flex; flex-direction: column; gap: 10px; }
.section-title {
  font-family: 'Space Mono', monospace;
  font-size: 12px;
  font-weight: 700;
  color: #e6edf3;
  margin: 0;
  display: flex;
  align-items: center;
  gap: 8px;
  text-transform: uppercase;
  letter-spacing: .06em;
}
.section-title::after {
  content: '';
  flex: 1;
  height: 1px;
  background: #21262d;
}
.st-badge {
  font-family: 'Space Mono', monospace;
  font-size: 10px;
  background: color-mix(in srgb, var(--theme) 15%, transparent);
  border: 1px solid color-mix(in srgb, var(--theme) 30%, transparent);
  color: var(--theme);
  padding: 2px 6px;
  border-radius: 4px;
}

/* EXP CARDS */
.exp-card {
  background: #161b22;
  border: 1px solid #30363d;
  border-radius: 8px;
  padding: 14px 16px;
  border-left: 3px solid var(--theme);
}
.exp-card.flat { padding: 10px 16px; }
.ec-header { display: flex; justify-content: space-between; align-items: flex-start; gap: 8px; margin-bottom: 6px; }
.ec-title { font-size: 13px; font-weight: 700; color: #e6edf3; }
.ec-company { font-size: 11px; color: var(--theme); font-weight: 600; }
.ec-date { font-size: 10px; color: #6e7681; white-space: nowrap; background: #21262d; padding: 2px 7px; border-radius: 4px; }
.ec-desc { font-size: 11.5px; color: #8b949e; line-height: 1.6; margin: 0; }

/* SKILLS CHIPS */
.skills-grid { display: flex; flex-direction: column; gap: 5px; }
.skill-chip {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 5px 10px;
  border-radius: 6px;
  border: 1px solid #30363d;
  background: #161b22;
}
.chip-name { font-size: 11px; color: #c9d1d9; font-weight: 600; }
.chip-level {
  font-size: 9px;
  font-family: 'Space Mono', monospace;
  color: var(--theme);
  background: color-mix(in srgb, var(--theme) 10%, transparent);
  padding: 1px 6px;
  border-radius: 3px;
}

/* LANGUES */
.lang-item { display: flex; align-items: center; gap: 6px; margin-bottom: 4px; }
.li-name { font-size: 11px; color: #c9d1d9; min-width: 55px; font-weight: 600; }
.li-bar { flex: 1; height: 3px; background: #21262d; border-radius: 99px; }
.li-fill { height: 100%; background: var(--theme); border-radius: 99px; }
.li-level { font-size: 9px; color: #6e7681; min-width: 38px; text-align: right; }

/* TAGS */
.tags-wrap { display: flex; flex-wrap: wrap; gap: 4px; }
.i-tag {
  font-size: 10px;
  background: #161b22;
  color: #8b949e;
  padding: 3px 8px;
  border-radius: 4px;
  border: 1px solid #30363d;
}
</style>