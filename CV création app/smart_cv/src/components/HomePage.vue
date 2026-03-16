<template>
  <div class="home-container">
    <div class="overlay"></div>

    <div class="content">
      <header class="home-header">
        <div class="logo-area">
          <img src="https://images.unsplash.com/photo-1764249454489-f3fb1184fb88?q=80&w=687" alt="Logo" class="mini-logo">
          <h1 class="app-name">CV<span class="text-success">BUILDER</span> PRO</h1>
        </div>
      </header>

      <main class="home-main">
        <h2 class="welcome-title">Bienvenue sur votre Créateur de CV Professionnel</h2>
        <p class="welcome-text">
          Créez, éditez et téléchargez des CV élégants et percutants en quelques clics. 
          Démarquez-vous auprès des recruteurs avec des modèles conçus par des professionnels.
        </p>
        <button @click="goToLogin" class="btn-primary">
          Se connecter / Commencer
        </button>
      </main>

      <footer class="home-footer">
        <p>&copy; 2026 CVBuilder Pro. Tous droits réservés.</p>
      </footer>
    </div>
  </div>
</template>

<script setup>
import { defineEmits } from 'vue'

const emit = defineEmits(['go-to-login'])

const goToLogin = () => {
  emit('go-to-login')
}
</script>

<style scoped>
/* ================= GLOBAL & BACKGROUND ================= */
.home-container {
  height: 100vh;
  width: 100%;
  position: relative;
  /* Image de fond avec effet de parallaxe (fixed) */
  background-image: url('https://images.unsplash.com/photo-1764249454489-f3fb1184fb88?q=80&w=687&auto=format&fit=crop');
  background-size: cover;
  background-position: center;
  background-attachment: fixed;
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: 'Poppins', sans-serif;
  overflow: hidden;
}

.overlay {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7); /* Fond noir transparent */
  z-index: 1;
}

.content {
  position: relative;
  z-index: 2;
  color: white;
  text-align: center;
  width: 80%;
  max-width: 800px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  height: 80vh;
}

/* ================= HEADER ================= */
.home-header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px 0;
}

.logo-area {
  display: flex;
  align-items: center;
  gap: 10px;
}

.mini-logo {
  width: 40px;
  height: 40px;
  object-fit: cover;
  border-radius: 50%;
  border: 2px solid #28a745;
}

.app-name {
  font-size: 24px;
  font-weight: 700;
  margin: 0;
}

.text-success {
  color: #28a745;
}

/* ================= MAIN ================= */
.home-main {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 20px;
}

.welcome-title {
  font-size: 48px;
  font-weight: 800;
  margin: 0;
  line-height: 1.2;
}

.welcome-text {
  font-size: 18px;
  color: #ccc;
  line-height: 1.6;
  max-width: 600px;
}

/* ================= BOUTON ================= */
.btn-primary {
  background-color: #28a745;
  color: white;
  border: none;
  padding: 15px 40px;
  font-size: 18px;
  font-weight: bold;
  cursor: pointer;
  border-radius: 30px;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(40, 167, 69, 0.4);
}

.btn-primary:hover {
  background-color: #218838;
  transform: translateY(-3px);
  box-shadow: 0 6px 20px rgba(40, 167, 69, 0.6);
}

/* ================= FOOTER ================= */
.home-footer {
  font-size: 12px;
  color: #777;
  padding: 20px 0;
}
</style>