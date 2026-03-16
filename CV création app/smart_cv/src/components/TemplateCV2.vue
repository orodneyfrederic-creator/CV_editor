<template>
  <div class="cv-nova" :style="{ '--theme': cv.themeColor || '#10b981' }">

    <!-- ══ HEADER PLEINE LARGEUR ══ -->
    <header class="cv-header">
      <div class="header-left">
        <div class="photo-wrap">
          <img v-if="cv.profile.photo" :src="cv.profile.photo" class="photo" alt="Photo" />
          <div v-else class="photo-placeholder"><i class="fas fa-user"></i></div>
        </div>
        <div class="header-info">
          <h1 class="name">{{ cv.profile.name }}</h1>
          <div class="role">{{ cv.profile.job }}</div>
          <p class="about" v-if="cv.profile.about">{{ cv.profile.about }}</p>
        </div>
      </div>
      <div class="header-contact">
        <div v-if="cv.profile.email" class="contact-item">
          <span class="ci-icon">Email: </span><span>{{ cv.profile.email }}</span>
        </div>
        <div v-if="cv.profile.phone" class="contact-item">
          <span class="ci-icon">Téléphone: </span><span>{{ cv.profile.phone }}</span>
        </div>
        <div v-if="cv.profile.address" class="contact-item">
          <span class="ci-icon">Adresse: </span><span>{{ cv.profile.address }}</span>
        </div>
        <div v-if="cv.socials?.linkedin" class="contact-item">
          <span class="ci-icon">in</span><span>{{ cv.socials.linkedin }}</span>
        </div>
        <div v-if="cv.socials?.github" class="contact-item">
          <span class="ci-icon">GitHub: </span><span>{{ cv.socials.github }}</span>
        </div>
      </div>
    </header>

    <!-- ══ BODY : 2 colonnes ══ -->
    <div class="cv-body">

      <!-- Colonne principale -->
      <div class="col-main">

        <section v-if="cv.experiences?.length" class="section">
          <h2 class="section-title"><span class="title-line"></span>Expériences</h2>
          <div v-for="exp in cv.experiences" :key="exp.id" class="timeline-item">
            <div class="tl-dot"></div>
            <div class="tl-content">
              <div class="tl-top">
                <span class="tl-job">{{ exp.title }}</span>
                <span class="tl-date">{{ exp.date }}</span>
              </div>
              <div class="tl-company">{{ exp.company }}</div>
              <p class="tl-desc">{{ exp.description }}</p>
            </div>
          </div>
        </section>

        <section v-if="cv.education?.length" class="section">
          <h2 class="section-title"><span class="title-line"></span>Formation</h2>
          <div v-for="edu in cv.education" :key="edu.id" class="timeline-item">
            <div class="tl-dot"></div>
            <div class="tl-content">
              <div class="tl-top">
                <span class="tl-job">{{ edu.title }}</span>
                <span class="tl-date">{{ edu.year }}</span>
              </div>
              <div class="tl-company">{{ edu.school }}</div>
            </div>
          </div>
        </section>

      </div>

      <!-- Colonne latérale -->
      <div class="col-side">

        <section v-if="cv.skills?.length" class="section">
          <h2 class="section-title"><span class="title-line"></span>Compétences</h2>
          <div v-for="skill in cv.skills" :key="skill.name" class="skill-row">
            <span class="skill-name">{{ skill.name }}</span>
            <div class="skill-dots">
              <span
                v-for="n in 4" :key="n"
                class="dot"
                :class="{ filled: n <= skillLevelToDots(skill.level) }"
              ></span>
            </div>
          </div>
        </section>

        <section v-if="cv.languages?.length" class="section">
          <h2 class="section-title"><span class="title-line"></span>Langues</h2>
          <div v-for="lang in cv.languages" :key="lang.name" class="lang-row">
            <span class="skill-name">{{ lang.name }}</span>
            <span class="lang-badge">{{ lang.level }}</span>
          </div>
        </section>

        <section v-if="cv.interests?.length" class="section">
          <h2 class="section-title"><span class="title-line"></span>Intérêts</h2>
          <div class="tags-wrap">
            <span v-for="i in cv.interests" :key="i" class="tag">{{ i }}</span>
          </div>
        </section>

      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({ cv: { type: Object, required: true } })

const skillLevelToDots = (level) => {
  const map = { 'Débutant': 1, 'Intermédiaire': 2, 'Avancé': 3, 'Expert': 4,
                'Notions': 1, 'Courant': 3, 'Bilingue': 4, 'Natif': 4 }
  return map[level] ?? 2
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;700&family=DM+Serif+Display&display=swap');

.cv-nova {
  --theme: #10b981;
  width: 210mm;
  min-height: 297mm;
  background: #fafaf8;
  font-family: 'DM Sans', sans-serif;
  color: #1c1c1c;
}

/* HEADER */
.cv-header {
  background: var(--theme);
  padding: 32px 36px;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  gap: 24px;
  color: white;
}
.header-left { display: flex; gap: 20px; align-items: flex-start; flex: 1; }
.photo-wrap {
  width: 88px; height: 88px;
  border-radius: 12px;
  border: 3px solid rgba(255,255,255,0.4);
  overflow: hidden;
  flex-shrink: 0;
  background: rgba(255,255,255,0.15);
  display: flex; align-items: center; justify-content: center;
}
.photo { width: 100%; height: 100%; object-fit: cover; }
.photo-placeholder { font-size: 36px; color: rgba(255,255,255,0.5); }

.header-info { display: flex; flex-direction: column; gap: 4px; }
.name {
  font-family: 'DM Serif Display', serif;
  font-size: 28px;
  margin: 0;
  color: white;
  line-height: 1.1;
}
.role {
  font-size: 12px;
  font-weight: 700;
  letter-spacing: .14em;
  text-transform: uppercase;
  color: rgba(255,255,255,0.75);
}
.about { font-size: 12px; color: rgba(255,255,255,0.85); line-height: 1.55; margin: 6px 0 0; max-width: 320px; }

.header-contact {
  display: flex;
  flex-direction: column;
  gap: 7px;
  min-width: 170px;
}
.contact-item {
  display: flex; align-items: center; gap: 8px;
  font-size: 11px; color: rgba(255,255,255,0.9);
}
.ci-icon {
  width: 20px; height: 20px;
  background: rgba(255,255,255,0.2);
  border-radius: 4px;
  display: flex; align-items: center; justify-content: center;
  font-size: 10px; font-weight: 700;
  flex-shrink: 0;
}

/* BODY */
.cv-body {
  display: grid;
  grid-template-columns: 1fr 200px;
  gap: 0;
  padding: 28px 36px;
}
.col-main { padding-right: 28px; border-right: 1px solid #e8e8e4; display: flex; flex-direction: column; gap: 22px; }
.col-side { padding-left: 24px; display: flex; flex-direction: column; gap: 22px; }

/* SECTIONS */
.section { display: flex; flex-direction: column; gap: 10px; }
.section-title {
  font-family: 'DM Serif Display', serif;
  font-size: 15px;
  color: #1c1c1c;
  margin: 0;
  display: flex;
  align-items: center;
  gap: 8px;
}
.title-line {
  display: inline-block;
  width: 18px; height: 3px;
  background: var(--theme);
  border-radius: 2px;
  flex-shrink: 0;
}

/* TIMELINE */
.timeline-item { display: flex; gap: 12px; position: relative; }
.tl-dot {
  width: 10px; height: 10px;
  border-radius: 50%;
  background: var(--theme);
  flex-shrink: 0;
  margin-top: 5px;
}
.tl-content { display: flex; flex-direction: column; gap: 2px; padding-bottom: 14px; border-left: 1px dashed #ddd; padding-left: 14px; margin-left: -16px; }
.tl-top { display: flex; justify-content: space-between; align-items: baseline; gap: 8px; }
.tl-job { font-weight: 700; font-size: 13px; color: #1c1c1c; }
.tl-date { font-size: 10px; color: #999; font-style: italic; white-space: nowrap; }
.tl-company { color: var(--theme); font-size: 12px; font-weight: 600; }
.tl-desc { font-size: 11.5px; color: #666; line-height: 1.6; margin: 3px 0 0; }

/* SKILLS dots */
.skill-row { display: flex; justify-content: space-between; align-items: center; gap: 8px; }
.skill-name { font-size: 12px; font-weight: 500; color: #333; }
.skill-dots { display: flex; gap: 3px; }
.dot {
  width: 8px; height: 8px;
  border-radius: 50%;
  background: #e0e0da;
  transition: background .2s;
}
.dot.filled { background: var(--theme); }

/* LANGUES */
.lang-row { display: flex; justify-content: space-between; align-items: center; }
.lang-badge {
  font-size: 10px;
  font-weight: 700;
  color: var(--theme);
  background: color-mix(in srgb, var(--theme) 12%, white);
  padding: 2px 8px;
  border-radius: 99px;
}

/* TAGS */
.tags-wrap { display: flex; flex-wrap: wrap; gap: 5px; }
.tag {
  font-size: 10px;
  background: #f0f0ec;
  color: #555;
  padding: 3px 9px;
  border-radius: 99px;
  border: 1px solid #e0e0da;
}
</style>