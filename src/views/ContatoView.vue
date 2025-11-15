<template>
  <section class="bg-[#161E2C] py-24 text-white" id="hero">
    <div class="max-w-7xl mx-auto px-6 text-center mb-12 v-scroll-animate">
      <h2 class="text-4xl font-bold text-yellow-500 mb-3">Entre em Contato</h2>
      <p class="text-zinc-300">Estamos prontos para atender você e sua empresa. Escolha o melhor canal para você.</p>
    </div>

    <!-- CARDS DE CONTATO -->
    <div class="mx-auto max-w-7xl px-6 mt-12 grid grid-cols-1 md:grid-cols-3 gap-6">
      <Card
        v-for="(card, i) in cards"
        :key="i"
        :icon="card.icon"
        :title="card.title"
        :description="card.description"
        class="p-8 v-scroll-animate transition-transform"
      />
    </div>

    <!-- FORMULÁRIO + MAPA -->
    <div class="max-w-7xl mx-auto px-6 mt-20 grid md:grid-cols-2 gap-12">
      
      <!-- FORMULÁRIO -->
      <div class="bg-[#1B2533] border border-yellow-500/40 rounded-3xl p-8 shadow-lg v-scroll-animate">
        <h2 class="text-2xl font-bold mb-6">Envie uma Mensagem</h2>
        
        <div class="mb-4">
          <label for="nome" class="block text-sm font-semibold mb-1">Nome</label>
          <input 
            type="text" 
            id="nome" 
            v-model="nome"
            placeholder="Digite seu nome"
            class="w-full px-4 py-3 border border-zinc-600 rounded-lg bg-[#1B2533] outline-none focus:border-yellow-400" 
          />
        </div>

        <div class="mb-4">
          <label for="email" class="block text-sm font-semibold mb-1">Email</label>
          <input 
            type="email" 
            id="email" 
            v-model="email"
            placeholder="Digite seu email"
            class="w-full px-4 py-3 border border-zinc-600 rounded-lg bg-[#1B2533] outline-none focus:border-yellow-400" 
          />
        </div>

        <div class="mb-6">
          <label for="mensagem" class="block text-sm font-semibold mb-1">Mensagem</label>
          <textarea 
            id="mensagem" 
            v-model="mensagem"
            placeholder="Digite sua mensagem"
            class="w-full px-4 py-3 border border-zinc-600 rounded-lg bg-[#1B2533] outline-none min-h-[180px] resize-none focus:border-yellow-400">
          </textarea>
        </div>

        <button 
          class="w-full py-3 bg-yellow-500 hover:bg-yellow-600 font-bold rounded-3xl text-zinc-900 transition-all"
          @click="enviarMensagem"
        >
          Enviar Mensagem
        </button>

        <!-- EXIBIR STATUS -->
        <p v-if="status" class="text-center mt-4 font-semibold" :class="statusColor">
          {{ status }}
        </p>

      </div>

      <!-- MAPA + CONTATO -->
      <div class="space-y-6 v-scroll-animate">
        
        <!-- MAPA -->
        <div class="rounded-3xl overflow-hidden border border-yellow-500/40">
        <iframe 
          src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3959.6792206713393!2d-35.01294242408363!3d-8.119918081230918!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x7ab1ef1a3f7d3d1%3A0x2a0f0bca3b3c2b53!2sR.%20Maria%20Jos%C3%A9%20Cordeiro%2C%20216%20-%20Dois%20Carneiros%2C%20Jaboat%C3%A3o%20dos%20Guararapes%20-%20PE%2C%2054280-770!5e0!3m2!1spt-BR!2sbr!4v1735095000000!5m2!1spt-BR!2sbr" 
          width="100%" 
          height="280" 
          style="border:0;" 
          allowfullscreen="" 
          loading="lazy" 
          referrerpolicy="no-referrer-when-downgrade">
        </iframe>
        </div>

        <!-- INFORMAÇÕES -->
        <div class="space-y-4 text-zinc-200">
          <div class="flex bg-[#1B2533] items-center gap-3 border border-yellow-500/40 rounded-2xl p-4">
            <i class="fa-solid fa-phone text-yellow-400"></i>
            <span>(81) 9 8748-8446</span>
          </div>

          <div class="flex bg-[#1B2533] items-center gap-3 border border-yellow-500/40 rounded-2xl p-4">
            <i class="fa-solid fa-envelope text-yellow-400"></i>
            <span>comercial.mdterceirizacao@gmail.com</span>
          </div>

          <div class="flex bg-[#1B2533] items-center gap-3 border border-yellow-500/40 rounded-2xl p-4">
            <i class="fa-solid fa-location-dot text-yellow-400"></i>
            <span> Rua Maria José Cordeiro, 216. CEP 54280-770 Dois Carneiros Jaboatão dos Guararapes</span>
          </div>

          <a href="https://wa.me/5581987488446" target="_blank"
            class="block bg-green-600 hover:bg-green-700 text-white font-bold text-center py-3 rounded-3xl transition-all">
            <i class="fab fa-whatsapp text-white "></i>
            Fale Conosco no WhatsApp
          </a>
        </div>
      </div>

    </div>
  </section>
</template>


<script setup>
import Card from '../components/Cards.vue'
import '@fortawesome/fontawesome-free/css/all.min.css'
import { ref, onMounted } from 'vue'
import axios from 'axios'

// CAMPOS DO FORMULÁRIO
const nome = ref("")
const email = ref("")
const mensagem = ref("")
const status = ref("")
const statusColor = ref("")

// FUNÇÃO PARA ENVIAR MENSAGEM
const enviarMensagem = async () => {
  if (!nome.value || !email.value || !mensagem.value) {
    status.value = "⚠️ Preencha todos os campos."
    statusColor.value = "text-red-400"
    return
  }

  try {
    status.value = "Enviando..."
    statusColor.value = "text-yellow-400"

    const response = await axios.post("https://md-terceirizacao-api-1.onrender.com/api/contact", {
      nome: nome.value,
      email: email.value,
      mensagem: mensagem.value
    })

    status.value = "✔️ Mensagem enviada com sucesso!"
    statusColor.value = "text-green-400"

    nome.value = ""
    email.value = ""
    mensagem.value = ""

  } catch (err) {
    status.value = "❌ Erro ao enviar mensagem."
    statusColor.value = "text-red-400"
  }
}


// CARDS
const cards = [
  { title: 'Sede Principal', icon: 'fa-solid fa-building', description: 'Visite-nos para uma consulta presencial.' },
  { title: 'Atendimento Telefônico', icon: 'fa-solid fa-phone', description: 'Nossa equipe está pronta para ajudar.' },
  { title: 'Suporte Online', icon: 'fa-solid fa-comments', description: 'Conecte-se conosco digitalmente.' }
]

// ANIMAÇÃO
onMounted(() => {
  const elements = document.querySelectorAll('.v-scroll-animate')
  const observer = new IntersectionObserver((entries) => {
    entries.forEach((entry, index) => {
      if (entry.isIntersecting) {
        entry.target.style.transitionDelay = `${index * 100}ms`
        entry.target.classList.add('animate-fadeInUp')
      }
    })
  }, { threshold: 0.1 })

  elements.forEach(el => observer.observe(el))
})
</script>


<style scoped>
.transition-transform {
  transition: transform 0.5s ease-in-out;
}

/* Scroll Animation */
.v-scroll-animate {
  opacity: 0;
  transform: translateY(40px);
  transition: opacity 0.8s ease, transform 0.8s ease;
}

.animate-fadeInUp {
  opacity: 1;
  transform: translateY(0);
}
</style>
