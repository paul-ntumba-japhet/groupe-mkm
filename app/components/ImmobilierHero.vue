<script setup lang="ts">
import { getCurrentInstance } from 'vue'
import hero1 from '~/assets/image/services/immobilier/hero1.png'
import hero2 from '~/assets/image/services/immobilier/hero2.png'
import hero3 from '~/assets/image/services/immobilier/hero3.png'

const slides = [
  {
    src: hero1,
    alt: 'Villa contemporaine au coucher du soleil, avec terrasse, palmiers et vue sur la ville.'
  },
  {
    src: hero2,
    alt: 'Maison blanche moderne avec un porche en béton, une allée pavée et un jardin de palmiers.'
  },
  {
    src: hero3,
    alt: 'Résidence avec une piscine, des transats et des palmiers devant les balcons.'
  }
]

const instance = getCurrentInstance()
const SLIDE_MS = 720
const index = ref(0)
const leaving = ref<number | null>(null)
const userPaused = ref(false)
const keyboardPaused = ref(false)
const hovering = ref(false)
const reducedMotion = ref(false)
const hero = ref<HTMLElement | null>(null)
let timer: ReturnType<typeof setInterval> | undefined
let leaveTimer: ReturnType<typeof setTimeout> | undefined
let removeMedia: (() => void) | undefined

const holding = computed(() => userPaused.value || keyboardPaused.value || reducedMotion.value)

function show (next: number) {
  const normalized = (next + slides.length) % slides.length
  if (normalized === index.value) return
  if (!reducedMotion.value) {
    leaving.value = index.value
    if (leaveTimer) clearTimeout(leaveTimer)
    leaveTimer = setTimeout(() => {
      leaving.value = null
      instance?.update()
    }, SLIDE_MS)
  }
  index.value = normalized
  instance?.update()
}

function restart () {
  if (timer) clearInterval(timer)
  timer = undefined
  if (!import.meta.client || userPaused.value || keyboardPaused.value || hovering.value || reducedMotion.value) return
  timer = setInterval(() => show(index.value + 1), 7000)
}

function onFocusIn (event: FocusEvent) {
  const target = event.target
  if (!(target instanceof Element)) return
  if (target.closest('[data-hero-playback], .mkm-slider-controls, .mkm-slider-pagination')) return
  keyboardPaused.value = true
}

function onFocusOut (event: FocusEvent) {
  const next = event.relatedTarget
  if (!(next instanceof Node) || !hero.value?.contains(next)) {
    keyboardPaused.value = false
    hovering.value = false
  }
}

function togglePlayback () {
  if (reducedMotion.value) return
  if (userPaused.value || keyboardPaused.value) {
    userPaused.value = false
    keyboardPaused.value = false
    return
  }
  userPaused.value = true
}

onMounted(() => {
  const media = window.matchMedia('(prefers-reduced-motion: reduce)')
  const apply = () => {
    reducedMotion.value = media.matches
  }
  apply()
  media.addEventListener('change', apply)
  removeMedia = () => media.removeEventListener('change', apply)
  restart()
})

watch([userPaused, keyboardPaused, hovering, reducedMotion], restart)

onBeforeUnmount(() => {
  if (timer) clearInterval(timer)
  if (leaveTimer) clearTimeout(leaveTimer)
  removeMedia?.()
})
</script>

<template>
  <section
    ref="hero"
    class="mkm-immo-hero"
    aria-roledescription="carrousel"
    aria-labelledby="titre-immobilier"
    @mouseenter="hovering = true"
    @mouseleave="hovering = false"
    @focusin="onFocusIn"
    @focusout="onFocusOut"
  >
    <div class="mkm-immo-hero__cadre">
        <div class="mkm-immo-hero__stage">
          <img
            v-for="(slide, slideIndex) in slides"
            :key="slide.src"
            :src="slide.src"
            :alt="slideIndex === index ? slide.alt : ''"
            :class="{ 'is-active': slideIndex === index, 'is-leaving': slideIndex === leaving }"
            :aria-hidden="slideIndex === index ? undefined : 'true'"
          >
        </div>
        <div class="mkm-immo-hero__voile" aria-hidden="true" />
        <div class="mkm-container mkm-immo-hero__contenu">
        <div class="mkm-immo-hero__copy">
          <p class="mkm-eyebrow">Nos services</p>
          <h1 id="titre-immobilier">Donnez vie à vos ambitions immobilières</h1>
          <p>Acheter, vendre ou louer un bien immobilier est une décision importante. Le Groupe MKM vous accompagne dans vos projets immobiliers en vous proposant un service de proximité, une écoute attentive et des solutions adaptées à vos besoins.</p>
          <div class="mkm-actions">
            <NuxtLink :to="{ path: '/contact', query: { sujet: 'Proposer un bien' } }" class="mkm-btn mkm-btn--gold">Proposer un bien</NuxtLink>
            <NuxtLink :to="{ path: '/contact', query: { sujet: 'Rechercher un bien' } }" class="mkm-btn mkm-btn--inverse">Rechercher un bien</NuxtLink>
          </div>
        </div>
        <div class="mkm-immo-hero__barre">
          <div class="mkm-slider-pagination" role="group" aria-label="Diapositives">
            <button
              v-for="(slide, slideIndex) in slides"
              :key="slide.src"
              type="button"
              class="mkm-slider-page"
              :aria-current="slideIndex === index ? 'true' : undefined"
              :aria-label="`Afficher la diapositive ${slideIndex + 1}`"
              @click="show(slideIndex)"
            >
              {{ String(slideIndex + 1).padStart(2, '0') }}
            </button>
          </div>
          <div class="mkm-slider-controls">
            <button type="button" class="mkm-slider-control" aria-label="Diapositive précédente" @click="show(index - 1)">
              <MkmIcon name="chevron-left" />
            </button>
            <button
              type="button"
              class="mkm-slider-control"
              data-hero-playback
              :aria-pressed="holding"
              :disabled="reducedMotion"
              :aria-label="holding ? 'Reprendre le défilement' : 'Mettre le défilement en pause'"
              @click="togglePlayback"
            >
              <MkmIcon :name="holding ? 'play' : 'pause'" />
            </button>
            <button type="button" class="mkm-slider-control" aria-label="Diapositive suivante" @click="show(index + 1)">
              <MkmIcon name="chevron-right" />
            </button>
          </div>
        </div>
        </div>
      </div>
  </section>
</template>
