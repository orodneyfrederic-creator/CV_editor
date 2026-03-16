<template>
  <div class="cv-lumiere" :style="{ '--theme': cv.themeColor || '#10b981' }">

    <!-- ══ TOP BAND ══ -->
    <div class="top-band"></div>

    <!-- ══ HEADER ══ -->
    <header class="cv-header">
      <div class="name-block">
        <h1 class="name">{{ cv.profile.name }}</h1>
        <div class="role">{{ cv.profile.job }}</div>
      </div>
      <div class="photo-block">
        <img v-if="cv.profile.photo" :src="cv.profile.photo" class="photo" alt="Photo" />
        <div v-else class="photo-ph"><i class="fas fa-user"></i></div>
      </div>
    </header>

    <div class="divider-deco">
      <span class="deco-line"></span>
      <span class="deco-diamond"></span>
      <span class="deco-line"></span>
    </div>

    <!-- ══ BODY ══ -->
    <div class="cv-body">

      <!-- GAUCHE -->
      <div class="col-left">

        <section v-if="cv.profile.about" class="section">
          <h2 class="section-title">Profil</h2>
          <p class="about">{{ cv.profile.about }}</p>
        </section>

        <section v-if="cv.experiences?.length" class="section">
          <h2 class="section-title">Expériences professionnelles</h2>
          <div v-for="exp in cv.experiences" :key="exp.id" class="entry">
            <div class="entry-meta">{{ exp.date }}</div>
            <div class="entry-title">{{ exp.title }}</div>
            <div class="entry-sub">{{ exp.company }}</div>
            <p class="entry-desc">{{ exp.description }}</p>
          </div>
        </section>

        <section v-if="cv.education?.length" class="section">
          <h2 class="section-title">Formation</h2>
          <div v-for="edu in cv.education" :key="edu.id" class="entry">
            <div class="entry-meta">{{ edu.year }}</div>
            <div class="entry-title">{{ edu.title }}</div>
            <div class="entry-sub">{{ edu.school }}</div>
          </div>
        </section>

      </div>

      <!-- DROITE -->
      <div class="col-right">

        <section class="section contact-section">
          <h2 class="section-title">Contact</h2>
          <div class="contact-list">
            <div v-if="cv.profile.email" class="contact-row">
              <span class="c-label">Email</span>
              <span class="c-value">{{ cv.profile.email }}</span>
            </div>
            <div v-if="cv.profile.phone" class="contact-row">
              <span class="c-label">Tél</span>
              <span class="c-value">{{ cv.profile.phone }}</span>
            </div>
            <div v-if="cv.profile.address" class="contact-row">
              <span class="c-label">Lieu</span>
              <span class="c-value">{{ cv.profile.address }}</span>
            </div>
            <div v-if="cv.socials?.linkedin" class="contact-row">
              <span class="c-label">LinkedIn</span>
              <span class="c-value">{{ cv.socials.linkedin }}</span>
            </div>
            <div v-if="cv.socials?.github" class="contact-row">
              <span class="c-label">GitHub</span>
              <span class="c-value">{{ cv.socials.github }}</span>
            </div>
          </div>
        </section>

        <section v-if="cv.skills?.length" class="section">
          <h2 class="section-title">Compétences</h2>
          <div v-for="skill in cv.skills" :key="skill.name" class="skill-entry">
            <div class="skill-top">
              <span class="skill-name">{{ skill.name }}</span>
              <span class="skill-level">{{ skill.level }}</span>
            </div>
            <div class="skill-bar">
              <div class="skill-fill" :style="{ width: levelToPercent(skill.level) + '%' }"></div>
            </div>
          </div>
        </section>

        <section v-if="cv.languages?.length" class="section">
          <h2 class="section-title">Langues</h2>
          <div v-for="lang in cv.languages" :key="lang.name" class="lang-row">
            <span class="skill-name">{{ lang.name }}</span>
            <span class="skill-level">{{ lang.level }}</span>
          </div>
        </section>

        <section v-if="cv.interests?.length" class="section">
          <h2 class="section-title">Intérêts</h2>
          <div class="interests">
            <span v-for="i in cv.interests" :key="i" class="interest">{{ i }}</span>
          </div>
        </section>

      </div>
    </div>

    <div class="bottom-band"></div>
  </div>
</template>

<script setup>
defineProps({ cv: { type: Object, required: true } })
const levelToPercent = (l) => ({ 'Débutant': 25, 'Intermédiaire': 50, 'Avancé': 75, 'Expert': 100, 'Notions': 20, 'Courant': 65, 'Bilingue': 85, 'Natif': 100 }[l] ?? 50)
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,600;0,700;1,400&family=Jost:wght@300;400;500;600&display=swap');

.cv-lumiere {
  --theme: #10b981;
  width: 210mm;
  min-height: 297mm;
  background: #fdf8f2;
  font-family: 'Jost', sans-serif;
  color: #2a2118;
  position: relative;
  display: flex;
  flex-direction: column;
}

.top-band { height: 8px; background: var(--theme); }
.bottom-band { height: 8px; background: var(--theme); margin-top: auto; }

/* HEADER */
.cv-header {
  padding: 32px 40px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.name-block { display: flex; flex-direction: column; gap: 5px; }
.name {
  font-family: 'Cormorant Garamond', serif;
  font-size: 42px;
  font-weight: 700;
  margin: 0;
  color: #1a110a;
  line-height: 1;
  letter-spacing: -0.5px;
}
.role {
  font-size: 11px;
  font-weight: 500;
  letter-spacing: .22em;
  text-transform: uppercase;
  color: var(--theme);
}

.photo-block {
  width: 90px; height: 90px;
  border-radius: 50%;
  overflow: hidden;
  border: 3px solid var(--theme);
  background: #e8e0d5;
  display: flex; align-items: center; justify-content: center;
}
.photo { width: 100%; height: 100%; object-fit: cover; }
.photo-ph { font-size: 32px; color: #b5a898; }

/* DECO DIVIDER */
.divider-deco {
  display: flex; align-items: center; gap: 8px;
  padding: 0 40px;
  margin-bottom: 6px;
}
.deco-line { flex: 1; height: 1px; background: var(--theme); opacity: .35; }
.deco-diamond {
  width: 7px; height: 7px;
  background: var(--theme);
  transform: rotate(45deg);
}

/* BODY */
.cv-body {
  display: grid;
  grid-template-columns: 3fr 2fr;
  gap: 20px;
  padding: 16px 40px 32px;
  flex: 1;
}
.col-left { display: flex; flex-direction: column; gap: 20px; }
.col-right { display: flex; flex-direction: column; gap: 20px; border-left: 1px solid #e2d9ce; padding-left: 24px; }

/* SECTIONS */
.section { display: flex; flex-direction: column; gap: 10px; }
.section-title {
  font-family: 'Cormorant Garamond', serif;
  font-size: 14px;
  font-weight: 600;
  letter-spacing: .12em;
  text-transform: uppercase;
  color: var(--theme);
  border-bottom: 1px solid color-mix(in srgb, var(--theme) 25%, transparent);
  padding-bottom: 4px;
  margin: 0;
}

/* ENTRIES */
.entry { display: flex; flex-direction: column; gap: 2px; margin-bottom: 10px; }
.entry-meta { font-size: 10px; color: #b5a898; font-weight: 500; letter-spacing: .06em; text-transform: uppercase; }
.entry-title { font-family: 'Cormorant Garamond', serif; font-size: 16px; font-weight: 600; color: #1a110a; }
.entry-sub { font-size: 12px; color: var(--theme); font-weight: 500; }
.entry-desc { font-size: 12px; color: #7a6a5a; line-height: 1.65; margin: 3px 0 0; }

/* CONTACT */
.contact-list { display: flex; flex-direction: column; gap: 6px; }
.contact-row { display: flex; flex-direction: column; gap: 1px; }
.c-label { font-size: 9px; font-weight: 600; letter-spacing: .1em; text-transform: uppercase; color: #b5a898; }
.c-value { font-size: 11px; color: #2a2118; word-break: break-all; }

/* SKILLS */
.skill-entry { display: flex; flex-direction: column; gap: 4px; margin-bottom: 6px; }
.skill-top { display: flex; justify-content: space-between; }
.skill-name { font-size: 12px; font-weight: 500; color: #2a2118; }
.skill-level { font-size: 10px; color: #b5a898; }
.skill-bar { height: 3px; background: #e8e0d5; border-radius: 99px; }
.skill-fill { height: 100%; background: var(--theme); border-radius: 99px; }

/* LANGUES */
.lang-row { display: flex; justify-content: space-between; margin-bottom: 4px; }

/* INTERESTS */
.interests { display: flex; flex-wrap: wrap; gap: 5px; }
.interest {
  font-size: 10px;
  color: #7a6a5a;
  background: #f0e8de;
  padding: 3px 10px;
  border-radius: 99px;
  border: 1px solid #e2d9ce;
}
</style>