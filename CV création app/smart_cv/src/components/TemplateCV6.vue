<template>
  <div class="cv-folio" :style="{ '--theme': cv.themeColor || '#10b981' }">

    <!-- ══ HEADER ÉDITORIAL ══ -->
    <header class="cv-header">
      <div class="header-main">
        <div class="header-top-line">
          <span class="ht-label">Curriculum Vitæ</span>
          <span class="ht-divider">—</span>
          <span class="ht-job">{{ cv.profile.job }}</span>
        </div>
        <h1 class="name">{{ cv.profile.name }}</h1>
        <div class="header-stripe"></div>
      </div>
      <div class="header-side">
        <div class="photo-frame">
          <img v-if="cv.profile.photo" :src="cv.profile.photo" class="photo" alt="Photo" />
          <div v-else class="photo-ph"><i class="fas fa-user"></i></div>
          <div class="photo-border"></div>
        </div>
      </div>
    </header>

    <!-- ══ CONTACT ROW ══ -->
    <div class="contact-row">
      <span v-if="cv.profile.email">{{ cv.profile.email }}</span>
      <span class="cr-dot" v-if="cv.profile.phone">·</span>
      <span v-if="cv.profile.phone">{{ cv.profile.phone }}</span>
      <span class="cr-dot" v-if="cv.profile.address">·</span>
      <span v-if="cv.profile.address">{{ cv.profile.address }}</span>
      <span class="cr-dot" v-if="cv.socials?.linkedin">·</span>
      <span v-if="cv.socials?.linkedin">{{ cv.socials.linkedin }}</span>
      <span class="cr-dot" v-if="cv.socials?.github">·</span>
      <span v-if="cv.socials?.github">{{ cv.socials.github }}</span>
    </div>

    <!-- ══ ABOUT ══ -->
    <div class="about-band" v-if="cv.profile.about">
      <div class="ab-quote">"</div>
      <p class="ab-text">{{ cv.profile.about }}</p>
    </div>

    <!-- ══ BODY 3 colonnes ══ -->
    <div class="cv-body">

      <!-- Col 1: Expériences -->
      <div class="col">
        <h2 class="col-title">Expériences</h2>
        <div v-for="exp in cv.experiences" :key="exp.id" class="entry">
          <div class="entry-year">{{ exp.date }}</div>
          <div class="entry-main">
            <div class="entry-role">{{ exp.title }}</div>
            <div class="entry-org">{{ exp.company }}</div>
            <p class="entry-desc">{{ exp.description }}</p>
          </div>
        </div>
      </div>

      <!-- Col 2: Formation + Langues -->
      <div class="col">
        <h2 class="col-title">Formation</h2>
        <div v-for="edu in cv.education" :key="edu.id" class="entry">
          <div class="entry-year">{{ edu.year }}</div>
          <div class="entry-main">
            <div class="entry-role">{{ edu.title }}</div>
            <div class="entry-org">{{ edu.school }}</div>
          </div>
        </div>

        <h2 class="col-title mt">Langues</h2>
        <div v-for="lang in cv.languages" :key="lang.name" class="lang-row">
          <span class="lr-name">{{ lang.name }}</span>
          <div class="lr-track">
            <div class="lr-fill" :style="{ width: ltp(lang.level) + '%' }"></div>
          </div>
          <span class="lr-level">{{ lang.level }}</span>
        </div>
      </div>

      <!-- Col 3: Compétences + Intérêts -->
      <div class="col">
        <h2 class="col-title">Compétences</h2>
        <div class="skills-stack">
          <div v-for="skill in cv.skills" :key="skill.name" class="skill-line">
            <div class="sl-fill-wrap">
              <div class="sl-fill" :style="{ width: ltp(skill.level) + '%' }"></div>
            </div>
            <div class="sl-info">
              <span class="sl-name">{{ skill.name }}</span>
              <span class="sl-tag">{{ skill.level }}</span>
            </div>
          </div>
        </div>

        <h2 class="col-title mt" v-if="cv.interests?.length">Intérêts</h2>
        <div class="interests-list" v-if="cv.interests?.length">
          <span v-for="i in cv.interests" :key="i" class="int-item">→ {{ i }}</span>
        </div>
      </div>

    </div>

    <div class="cv-footer">
      <div class="footer-line"></div>
      <span class="footer-name">{{ cv.profile.name }}</span>
      <div class="footer-line"></div>
    </div>

  </div>
</template>

<script setup>
defineProps({ cv: { type: Object, required: true } })
const ltp = (l) => ({ 'Débutant': 25, 'Intermédiaire': 50, 'Avancé': 75, 'Expert': 100, 'Notions': 20, 'Courant': 65, 'Bilingue': 85, 'Natif': 100 }[l] ?? 50)
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,700;0,900;1,400&family=IBM+Plex+Sans:wght@300;400;500;600&display=swap');

.cv-folio {
  --theme: #10b981;
  width: 210mm;
  min-height: 297mm;
  background: white;
  font-family: 'IBM Plex Sans', sans-serif;
  color: #1a1a1a;
  display: flex;
  flex-direction: column;
}

/* HEADER */
.cv-header {
  padding: 32px 36px 0;
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  gap: 20px;
}
.header-main { flex: 1; }
.header-top-line {
  display: flex;
  align-items: center;
  gap: 8px;
  margin-bottom: 6px;
}
.ht-label {
  font-size: 9px;
  font-weight: 600;
  letter-spacing: .2em;
  text-transform: uppercase;
  color: #9ca3af;
}
.ht-divider { color: var(--theme); font-weight: 300; }
.ht-job {
  font-size: 10px;
  font-weight: 500;
  letter-spacing: .12em;
  text-transform: uppercase;
  color: var(--theme);
}
.name {
  font-family: 'Playfair Display', serif;
  font-size: 46px;
  font-weight: 900;
  line-height: .95;
  margin: 0;
  color: #0a0a0a;
  letter-spacing: -1px;
}
.header-stripe {
  height: 4px;
  background: var(--theme);
  margin-top: 10px;
  width: 100%;
}

.header-side { padding-bottom: 4px; flex-shrink: 0; }
.photo-frame {
  width: 88px; height: 88px;
  position: relative;
  flex-shrink: 0;
}
.photo {
  width: 100%; height: 100%;
  object-fit: cover;
  border-radius: 4px;
}
.photo-ph {
  width: 100%; height: 100%;
  background: #f3f4f6;
  border-radius: 4px;
  display: flex; align-items: center; justify-content: center;
  font-size: 30px; color: #d1d5db;
}
.photo-border {
  position: absolute;
  top: 6px; left: 6px;
  width: 100%; height: 100%;
  border: 2px solid var(--theme);
  border-radius: 4px;
  z-index: -1;
}

/* CONTACT ROW */
.contact-row {
  display: flex;
  flex-wrap: wrap;
  gap: 4px 0;
  align-items: center;
  padding: 8px 36px;
  background: #f9fafb;
  font-size: 10.5px;
  color: #6b7280;
  border-top: 1px solid #e5e7eb;
  border-bottom: 1px solid #e5e7eb;
}
.contact-row span { padding: 0 6px; }
.cr-dot { color: var(--theme); font-weight: 700; padding: 0 2px; }

/* ABOUT BAND */
.about-band {
  display: flex;
  align-items: flex-start;
  gap: 8px;
  padding: 12px 36px;
  background: color-mix(in srgb, var(--theme) 6%, white);
  border-bottom: 1px solid color-mix(in srgb, var(--theme) 20%, white);
}
.ab-quote {
  font-family: 'Playfair Display', serif;
  font-size: 48px;
  line-height: .6;
  color: var(--theme);
  opacity: .5;
  flex-shrink: 0;
}
.ab-text { font-size: 12px; color: #4b5563; line-height: 1.65; margin: 0; font-style: italic; }

/* BODY 3 colonnes */
.cv-body {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  gap: 0;
  flex: 1;
  padding: 18px 0;
  border-top: 2px solid #1a1a1a;
}
.col {
  padding: 0 22px;
  border-right: 1px solid #e5e7eb;
  display: flex;
  flex-direction: column;
  gap: 8px;
}
.col:last-child { border-right: none; }

.col-title {
  font-family: 'Playfair Display', serif;
  font-size: 13px;
  font-weight: 700;
  letter-spacing: .04em;
  color: #0a0a0a;
  text-transform: uppercase;
  border-bottom: 2px solid var(--theme);
  padding-bottom: 4px;
  margin: 0 0 6px;
}
.col-title.mt { margin-top: 16px; }

/* ENTRIES */
.entry { display: flex; flex-direction: column; gap: 1px; margin-bottom: 10px; }
.entry-year { font-size: 9px; font-weight: 600; color: #9ca3af; letter-spacing: .08em; text-transform: uppercase; }
.entry-role { font-size: 13px; font-weight: 600; color: #0a0a0a; font-family: 'Playfair Display', serif; }
.entry-org { font-size: 11px; color: var(--theme); font-weight: 600; }
.entry-desc { font-size: 11px; color: #6b7280; line-height: 1.6; margin: 3px 0 0; }

/* LANGUES */
.lang-row { display: flex; align-items: center; gap: 6px; margin-bottom: 5px; }
.lr-name { font-size: 11px; font-weight: 500; color: #1a1a1a; min-width: 52px; }
.lr-track { flex: 1; height: 2px; background: #e5e7eb; border-radius: 99px; }
.lr-fill { height: 100%; background: var(--theme); border-radius: 99px; }
.lr-level { font-size: 9px; color: #9ca3af; min-width: 38px; text-align: right; }

/* SKILLS */
.skills-stack { display: flex; flex-direction: column; gap: 6px; }
.skill-line { display: flex; flex-direction: column; gap: 3px; }
.sl-fill-wrap { height: 2px; background: #f3f4f6; border-radius: 99px; }
.sl-fill { height: 100%; background: var(--theme); border-radius: 99px; }
.sl-info { display: flex; justify-content: space-between; align-items: baseline; }
.sl-name { font-size: 12px; font-weight: 500; color: #1a1a1a; }
.sl-tag { font-size: 9px; color: #9ca3af; }

/* INTÉRÊTS */
.interests-list { display: flex; flex-direction: column; gap: 3px; }
.int-item { font-size: 11px; color: #6b7280; }
.int-item::first-letter { color: var(--theme); font-weight: 700; }

/* FOOTER */
.cv-footer {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 10px 36px;
  border-top: 1px solid #e5e7eb;
}
.footer-line { flex: 1; height: 1px; background: #e5e7eb; }
.footer-name { font-size: 9px; font-weight: 600; letter-spacing: .2em; text-transform: uppercase; color: #9ca3af; white-space: nowrap; }
</style>