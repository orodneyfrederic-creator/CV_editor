<script setup>
import { ref } from 'vue'
import HomePage from './components/HomePage.vue'
import Login from './components/Login.vue'
import Dashboard from './components/Dashboard.vue'
import Editor from './components/Editeur.vue'
import { db } from './firebase' 
import { doc, setDoc } from "firebase/firestore"

const currentPage = ref('home')
const selectedTemplateId = ref(null)
const userData = ref(null)

const handleLoginSuccess = async (user) => {
  if (user && user.uid) {
    const userProfile = {
      uid: user.uid,
      displayName: user.displayName || "Utilisateur",
      email: user.email,
      photoURL: user.photoURL || `https://ui-avatars.com/api/?name=${user.displayName}`,
      lastLogin: new Date()
    }
    try {
      await setDoc(doc(db, "users", user.uid), userProfile, { merge: true })
      userData.value = userProfile
    } catch (e) {
      console.error("Erreur Firestore :", e)
      userData.value = userProfile
    }
  }
  currentPage.value = 'dashboard'
}

const handleTemplateSelect = (id) => {
  selectedTemplateId.value = id   // reçoit 'TemplateCV1' … 'TemplateCV6'
  currentPage.value = 'editor'
}
</script>

<template>
  <HomePage
    v-if="currentPage === 'home'"
    @go-to-login="currentPage = 'login'"
  />

  <Login
    v-else-if="currentPage === 'login'"
    @login-success="handleLoginSuccess"
  />

  <Dashboard
    v-else-if="currentPage === 'dashboard'"
    :user="userData"
    @select-template="handleTemplateSelect"
  />

  <Editor
    v-else-if="currentPage === 'editor'"
    :user="userData"
    :templateId="selectedTemplateId"
    @back="currentPage = 'dashboard'"
  />
</template>

<style>
body { margin: 0; padding: 0; }
.editor-container { min-height: 100vh; background-color: #0f172a; }
</style>