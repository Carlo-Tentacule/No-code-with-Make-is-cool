<template>
    <div class="flex items-center justify-center min-h-screen bg-gradient-to-br from-gray-900 to-gray-800">
      <div class="flex flex-col w-full items-center justify-center">
        <!-- Alerte en haut du formulaire -->
        <div
          v-if="submitted"
          :class="[
            'w-full max-w-lg p-4 mt-6 text-center text-lg font-medium rounded-lg',
            urgent ? 'bg-red-100 border-red-400 text-red-800' : 'bg-green-100 border-green-400 text-green-800'
          ]"
        >
          <p>{{ confirmationMessage }}</p>
        </div>
  
        <!-- Formulaire -->
        <div class="max-w-lg w-full p-8 bg-gray-800 bg-opacity-50 backdrop-filter backdrop-blur-xl rounded-2xl shadow-2xl transition-all duration-300 hover:shadow-blue-500/20">
          <h1 class="text-4xl font-bold text-center text-transparent bg-clip-text bg-gradient-to-r from-blue-400 to-purple-500 mb-8">
            Faux Formulaire
          </h1>
          <form class="space-y-6">
            <!-- Champ Nom -->
            <div class="space-y-2">
              <label for="name" class="block text-lg font-medium text-gray-200">Nom</label>
              <input
                type="text"
                id="name"
                v-model="formData.name"
                placeholder="Entrez votre nom"
                required
                class="w-full p-4 bg-gray-700 text-gray-100 border border-gray-600 rounded-xl shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 transition-all duration-300"
              />
            </div>
  
            <!-- Champ Email -->
            <div class="space-y-2">
              <label for="email" class="block text-lg font-medium text-gray-200">Email</label>
              <input
                type="email"
                id="email"
                v-model="formData.email"
                placeholder="Entrez votre email"
                required
                class="w-full p-4 bg-gray-700 text-gray-100 border border-gray-600 rounded-xl shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 transition-all duration-300"
              />
            </div>
  
            <!-- Champ Message -->
            <div class="space-y-2">
              <label for="message" class="block text-lg font-medium text-gray-200">Message</label>
              <textarea
                id="message"
                v-model="formData.message"
                placeholder="Écrivez votre message ici..."
                required
                rows="4"
                class="w-full p-4 bg-gray-700 text-gray-100 border border-gray-600 rounded-xl shadow-sm focus:outline-none focus:ring-2 focus:ring-blue-500 transition-all duration-300"
              ></textarea>
            </div>
  
            <!-- Bouton d'envoi -->
            <div class="text-center">
              <button
                type="button"
                @click="submitForm"
                class="w-full py-4 px-6 bg-gradient-to-r from-blue-500 to-purple-600 text-white text-lg font-semibold rounded-xl hover:from-blue-600 hover:to-purple-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-offset-2 focus:ring-offset-gray-800 transition-all duration-300 transform hover:scale-105"
              >
                Envoyer
              </button>
            </div>
          </form>
        </div>
      </div>
    </div>
</template>
  
<script setup>
    import { ref } from 'vue'

    // Données du formulaire
    const formData = ref({
    name: '',
    email: '',
    message: ''
    })

    // États pour afficher les messages
    const submitted = ref(false)
    const urgent = ref(false)
    const confirmationMessage = ref('')
  
    const submitForm = async () => {
        const { data, error } = await useFetch('https://hook.eu2.make.com/on79b2cn0s5g89klhi1k4p84umeoqaep', {
            method: 'POST',
            body: formData.value
        })

        // Vérifier si le message contient "urgent"
        if (formData.value.message.toLowerCase().includes('urgent')) {
            confirmationMessage.value = 'Nous traiterons votre demande en priorité.'
            urgent.value = true
        } else {
            confirmationMessage.value = 'Merci pour votre message, nous reviendrons vers vous rapidement.'
            urgent.value = false
        }
        
            // Indiquer que le formulaire a été soumis
            submitted.value = true
        
            // Réinitialiser les champs du formulaire
            setTimeout(() => {
            submitted.value = false
            formData.value = { name: '', email: '', message: '' }
        }, 3000) // Réinitialisation après 3 secondes
    }
  </script>
  