<script setup>
import { ref } from 'vue'
import { loginWithGoogle } from '../firebase' 

const emit = defineEmits(['login-success'])

// 1. Déclaration des variables réactives EN PREMIER
const email = ref('')
const password = ref('')

// 2. Fonction pour la connexion Google
const startGoogleLogin = async () => {
  try {
    const user = await loginWithGoogle() 
    if (user) {
      console.log("Succès Google :", user.displayName)
      emit('login-success', user) 
    }
  } catch (error) {
    console.error("Erreur popup Google :", error)
    alert("La connexion Google a échoué.")
  }
}

// 3. Fonction pour le bouton vert (Login Classique)
const startEmailLogin = () => {
  if (!email.value || !password.value) {
    alert("Oups ! Tu dois remplir ton email et ton mot de passe.")
    return 
  }
  
  // On crée un objet utilisateur factice pour que App.vue ne plante pas
  const dummyUser = {
    uid: "email-" + Date.now(), // Génère un ID unique temporaire
    displayName: email.value.split('@')[0], // Utilise le début de l'email comme nom
    email: email.value,
    photoURL: `https://ui-avatars.com/api/?name=${email.value}&background=38b469&color=fff`
  }

  console.log("Connexion email simulée pour :", email.value)
  emit('login-success', dummyUser) // On envoie l'utilisateur factice
}
</script>

<template>
  <div class="login-wrapper">
    <div class="overlay"></div>
    <div class="login-card p-5 shadow-lg">
      <h1 class="welcome-text mb-2">Bienvenue</h1>
      <p class="subtitle mb-4">Créez des CV professionnels en quelques clics.</p>

      <div class="form-group mb-3">
        <input 
          v-model="email" 
          type="email" 
          class="custom-input" 
          placeholder="Adresse mail"
        >
      </div>
      <div class="form-group mb-4">
        <input 
          v-model="password" 
          type="password" 
          class="custom-input" 
          placeholder="••••••"
        >
      </div>

      <button @click="startEmailLogin" class="btn-login mb-4">
        Se connecter
      </button>

      <div class="divider mb-4">
        <span>OU</span>
      </div>

      <button @click="startGoogleLogin" class="google-btn mb-4">
        <img src="https://www.gstatic.com/firebasejs/ui/2.0.0/images/auth/google.svg" width="20">
        Continuer avec Google
      </button>

      <div class="footer-links">
        <a href="#">Mot de passe oublié</a>
        <span class="mx-2 text-white-50">|</span>
        <a href="#">S'inscrire</a>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Conserve ton style "Glassmorphism" ici... */
.login-wrapper {
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: url('https://images.unsplash.com/photo-1764249454489-f3fb1184fb88?q=80&w=687&auto=format&fit=crop') center/cover no-repeat;
  position: relative;
}
.overlay { position: absolute; inset: 0; background: rgba(0,0,0,0.3); backdrop-filter: blur(5px); }
.login-card { position: relative; z-index: 1; background: rgba(255, 255, 255, 0.1); backdrop-filter: blur(15px); border: 1px solid rgba(255,255,255,0.2); border-radius: 30px; width: 400px; text-align: center; color: white; }
.custom-input { width: 100%; padding: 12px 20px; background: rgba(255,255,255,0.1); border: 1px solid rgba(255,255,255,0.3); border-radius: 50px; color: white; outline: none; }
.btn-login { width: 100%; padding: 12px; background: #38b469; border: none; border-radius: 50px; color: white; font-weight: bold; }
.google-btn { width: 100%; padding: 12px; background: white; border: none; border-radius: 50px; color: #333; font-weight: 600; display: flex; align-items: center; justify-content: center; gap: 10px; }
.divider { display: flex; align-items: center; font-size: 0.8rem; color: rgba(255,255,255,0.5); }
.divider::before, .divider::after { content: ""; flex: 1; height: 1px; background: rgba(255,255,255,0.2); margin: 0 10px; }
.footer-links a { color: white; text-decoration: none; font-size: 0.8rem; opacity: 0.7; }
</style>