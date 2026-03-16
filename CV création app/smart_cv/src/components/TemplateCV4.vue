<template>
  <div class="cv-atlas" :style="{ '--theme': cv.themeColor || '#10b981' }">

    <!-- ══ COLONNE PRINCIPALE ══ -->
    <div class="col-main">

      <header class="cv-header">
        <div class="header-photo">
          <img v-if="cv.profile.photo" :src="cv.profile.photo" class="photo" alt="Photo" />
          <div v-else class="photo-ph"><i class="fas fa-user"></i></div>
        </div>
        <div class="header-text">
          <h1 class="name">{{ cv.profile.name }}</h1>
          <div class="role">{{ cv.profile.job }}</div>
        </div>
      </header>

      <div class="header-accent"></div>

      <section v-if="cv.profile.about" class="section">
        <h2 class="section-title">À propos</h2>
        <p class="about">{{ cv.profile.about }}</p>
      </section>

      <section v-if="cv.experiences?.length" class="section">
        <h2 class="section-title">Expériences</h2>
        <div v-for="exp in cv.experiences" :key="exp.id" class="block-item">
          <div class="bi-left">
            <div class="bi-date">{{ exp.date }}</div>
            <div class="bi-bar"></div>
          </div>
          <div class="bi-right">
            <div class="bi-title">{{ exp.title }}</div>
            <div class="bi-company">{{ exp.company }}</div>
            <p class="bi-desc">{{ exp.description }}</p>
          </div>
        </div>
      </section>

      <section v-if="cv.education?.length" class="section">
        <h2 class="section-title">Formation</h2>
        <div v-for="edu in cv.education" :key="edu.id" class="block-item">
          <div class="bi-left">
            <div class="bi-date">{{ edu.year }}</div>
            <div class="bi-bar"></div>
          </div>
          <div class="bi-right">
            <div class="bi-title">{{ edu.title }}</div>
            <div class="bi-company">{{ edu.school }}</div>
          </div>
        </div>
      </section>

    </div>

    <!-- ══ SIDEBAR DROITE ══ -->
    <div class="col-side">

      <div class="side-top">
        <div class="side-avatar">
          <img v-if="cv.profile.photo" :src="cv.profile.photo" class="sa-photo" alt="Photo" />
          <div v-else class="sa-ph"><i class="fas fa-user"></i></div>
        </div>
      </div>

      <section class="side-section">
        <h3 class="side-title">Contact</h3>
        <div v-if="cv.profile.email" class="side-row"><span class="sr-label">Email</span><span>{{ cv.profile.email }}</span></div>
        <div v-if="cv.profile.phone" class="side-row"><span class="sr-label">Tél.</span><span>{{ cv.profile.phone }}</span></div>
        <div v-if="cv.profile.address" class="side-row"><span class="sr-label">Lieu</span><span>{{ cv.profile.address }}</span></div>
        <div v-if="cv.socials?.linkedin" class="side-row"><span class="sr-label">LinkedIn</span><span>{{ cv.socials.linkedin }}</span></div>
        <div v-if="cv.socials?.github" class="side-row"><span class="sr-label">GitHub</span><span>{{ cv.socials.github }}</span></div>
      </section>

      <section v-if="cv.skills?.length" class="side-section">
        <h3 class="side-title">Compétences</h3>
        <div v-for="skill in cv.skills" :key="skill.name" class="side-skill">
          <div class="ss-header">
            <span>{{ skill.name }}</span>
            <span class="ss-tag">{{ skill.level }}</span>
          </div>
          <div class="ss-bar">
            <div class="ss-fill" :style="{ width: ltp(skill.level) + '%' }"></div>
          </div>
        </div>
      </section>

      <section v-if="cv.languages?.length" class="side-section">
        <h3 class="side-title">Langues</h3>
        <div v-for="lang in cv.languages" :key="lang.name" class="side-lang">
          <span>{{ lang.name }}</span>
          <span class="sl-badge">{{ lang.level }}</span>
        </div>
      </section>

      <section v-if="cv.interests?.length" class="side-section">
        <h3 class="side-title">Intérêts</h3>
        <div class="side-tags">
          <span v-for="i in cv.interests" :key="i" class="s-tag">{{ i }}</span>
        </div>
      </section>

    </div>
  </div>
</template>

<script setup>
defineProps({ cv: { type: Object, required: true } })
const ltp = (l) => ({ 'Débutant': 25, 'Intermédiaire': 50, 'Avancé': 75, 'Expert': 100, 'Notions': 20, 'Courant': 65, 'Bilingue': 85, 'Natif': 100 }[l] ?? 50)
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;600;700;800&display=swap');

.cv-atlas {
  --theme: #10b981;
  width: 210mm;
  min-height: 297mm;
  display: grid;
  grid-template-columns: 1fr 195px;
  font-family: 'Outfit', sans-serif;
  background: white;
}

/* MAIN */
.col-main {
  padding: 0;
  display: flex;
  flex-direction: column;
  background: white;
}

/* HEADER */
.cv-header {
  padding: 28px 28px 20px;
  display: flex;
  align-items: center;
  gap: 18px;
  background: #f9fafb;
  border-bottom: 3px solid var(--theme);
}
.header-photo {
  width: 70px; height: 70px;
  border-radius: 10px;
  overflow: hidden;
  background: #e5e7eb;
  display: flex; align-items: center; justify-content: center;
  flex-shrink: 0;
}
.photo { width: 100%; height: 100%; object-fit: cover; }
.photo-ph { font-size: 26px; color: #9ca3af; }
.name {
  font-size: 26px;
  font-weight: 800;
  margin: 0;
  color: #111827;
  letter-spacing: -.5px;
}
.role {
  font-size: 12px;
  font-weight: 600;
  letter-spacing: .1em;
  text-transform: uppercase;
  color: var(--theme);
  margin-top: 3px;
}
.header-accent { display: none; }

.section { padding: 18px 28px; border-bottom: 1px solid #f3f4f6; }
.section-title {
  font-size: 11px;
  font-weight: 800;
  letter-spacing: .15em;
  text-transform: uppercase;
  color: var(--theme);
  margin: 0 0 12px;
  display: flex;
  align-items: center;
  gap: 8px;
}
.section-title::after {
  content: '';
  flex: 1;
  height: 1px;
  background: #f3f4f6;
}
.about { font-size: 12.5px; color: #6b7280; line-height: 1.65; margin: 0; }

/* BLOCK ITEMS */
.block-item { display: flex; gap: 14px; margin-bottom: 14px; }
.bi-left { display: flex; flex-direction: column; align-items: center; min-width: 70px; }
.bi-date { font-size: 10px; color: #9ca3af; font-weight: 600; white-space: nowrap; margin-bottom: 4px; }
.bi-bar { flex: 1; width: 2px; background: linear-gradient(to bottom, var(--theme), transparent); border-radius: 1px; }
.bi-right { display: flex; flex-direction: column; gap: 2px; padding-bottom: 4px; }
.bi-title { font-size: 14px; font-weight: 700; color: #111827; }
.bi-company { font-size: 12px; font-weight: 600; color: var(--theme); }
.bi-desc { font-size: 11.5px; color: #6b7280; line-height: 1.6; margin: 3px 0 0; }

/* SIDEBAR */
.col-side {
  background: var(--theme);
  padding: 24px 16px;
  display: flex;
  flex-direction: column;
  gap: 20px;
  color: white;
}
.side-top { display: flex; justify-content: center; }
.side-avatar {
  width: 80px; height: 80px;
  border-radius: 50%;
  overflow: hidden;
  border: 3px solid rgba(255,255,255,0.4);
  background: rgba(255,255,255,0.15);
  display: flex; align-items: center; justify-content: center;
}
.sa-photo { width: 100%; height: 100%; object-fit: cover; }
.sa-ph { font-size: 28px; color: rgba(255,255,255,0.5); }

.side-section { display: flex; flex-direction: column; gap: 7px; }
.side-title {
  font-size: 10px;
  font-weight: 800;
  letter-spacing: .14em;
  text-transform: uppercase;
  color: rgba(255,255,255,0.6);
  border-bottom: 1px solid rgba(255,255,255,0.2);
  padding-bottom: 4px;
  margin: 0;
}
.side-row { display: flex; flex-direction: column; gap: 1px; }
.sr-label { font-size: 9px; color: rgba(255,255,255,0.55); text-transform: uppercase; letter-spacing: .08em; }
.side-row span:last-child { font-size: 11px; color: rgba(255,255,255,0.9); word-break: break-all; }

.side-skill { margin-bottom: 5px; }
.ss-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 3px; }
.ss-header span:first-child { font-size: 12px; color: white; font-weight: 500; }
.ss-tag { font-size: 9px; color: rgba(255,255,255,0.6); }
.ss-bar { height: 3px; background: rgba(255,255,255,0.2); border-radius: 99px; }
.ss-fill { height: 100%; background: white; border-radius: 99px; }

.side-lang { display: flex; justify-content: space-between; align-items: center; }
.side-lang span:first-child { font-size: 12px; color: white; }
.sl-badge {
  font-size: 9px;
  background: rgba(255,255,255,0.2);
  color: white;
  padding: 2px 7px;
  border-radius: 99px;
}

.side-tags { display: flex; flex-wrap: wrap; gap: 4px; }
.s-tag {
  font-size: 10px;
  background: rgba(255,255,255,0.15);
  color: rgba(255,255,255,0.9);
  padding: 3px 8px;
  border-radius: 99px;
  border: 1px solid rgba(255,255,255,0.2);
}
</style>