<script setup lang="ts">
import { getCurrentInstance } from 'vue'
import { slides } from '~/data/site'

const instance = getCurrentInstance()

const SLIDE_MS = 720

const index = ref(0)
const leaving = ref<number | null>(null)
const userPaused = ref(false)
const keyboardPaused = ref(false)
const hovering = ref(false)
const reducedMotion = ref(false)
const hero = ref<HTMLElement | null>(null)
const tone = useState<'light' | 'dark'>('mkm-masthead-tone', () => 'light')
let timer: ReturnType<typeof setInterval> | undefined
let leaveTimer: ReturnType<typeof setTimeout> | undefined
let removeMedia: (() => void) | undefined

const current = computed(() => slides[index.value]!)

watch(index, (value) => {
  tone.value = slides[value]!.tone
}, { immediate: true })

function show(next: number) {
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

function restart() {
  if (timer) clearInterval(timer)
  timer = undefined
  if (!import.meta.client || userPaused.value || keyboardPaused.value || hovering.value || reducedMotion.value) return
  timer = setInterval(() => show(index.value + 1), 7000)
}

function onFocusIn(event: FocusEvent) {
  const target = event.target
  if (!(target instanceof Element)) return
  if (target.closest('[data-hero-playback], .mkm-slider-controls, .mkm-slider-pagination')) return
  keyboardPaused.value = true
}

function onFocusOut(event: FocusEvent) {
  const next = event.relatedTarget
  if (!(next instanceof Node) || !hero.value?.contains(next)) hovering.value = false
}

const holding = computed(() => userPaused.value || keyboardPaused.value || reducedMotion.value)

function togglePlayback() {
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
    class="mkm-hero"
    :class="{
      'mkm-hero--pattern': current.tone === 'light',
      'mkm-hero--groupe': 'collage' in current
    }"
    aria-roledescription="carrousel"
    aria-label="Présentation du Groupe MKM"
    @mouseenter="hovering = true"
    @mouseleave="hovering = false"
    @focusin="onFocusIn"
    @focusout="onFocusOut"
  >
    <img
      v-if="current.tone === 'dark' && 'image' in current && current.image"
      class="mkm-hero__background"
      :src="current.image"
      :alt="current.imageAlt"
    >
    <div class="mkm-hero__stage">
    <div
      v-for="(slide, slideIndex) in slides"
      :key="slide.id"
      class="mkm-container mkm-hero__inner"
      :class="{
        'is-active': slideIndex === index,
        'is-leaving': slideIndex === leaving
      }"
      :inert="slideIndex !== index"
      :aria-hidden="slideIndex === index ? undefined : 'true'"
    >
      <div class="mkm-hero__content">
        <p class="mkm-eyebrow" :class="{ 'mkm-eyebrow--rule': 'collage' in slide }">{{ slide.eyebrow }}</p>
        <component :is="slideIndex === 0 ? 'h1' : 'h2'" class="mkm-hero__title">
          {{ slide.title }}
          <em>{{ slide.accent }}</em>
        </component>
        <p class="mkm-hero__description">{{ slide.description }}</p>
        <div class="mkm-actions">
          <NuxtLink
            :to="slide.primary.to"
            :class="slide.tone === 'dark' ? 'mkm-btn mkm-btn--gold' : 'mkm-btn mkm-btn--primary'"
          >
            {{ slide.primary.label }}
            <MkmIcon name="arrow-right" />
          </NuxtLink>
          <NuxtLink
            :to="slide.secondary.to"
            :class="slide.tone === 'dark' ? 'mkm-btn mkm-btn--inverse' : 'mkm-btn mkm-btn--secondary'"
          >
            {{ slide.secondary.label }}
          </NuxtLink>
        </div>
      </div>
      <div v-if="'collage' in slide" class="mkm-hero__media">
        <div class="mkm-collage mkm-collage--groupe" :class="`mkm-collage--${slide.id}`">
          <figure
            v-for="item in slide.collage"
            :key="item.alt"
            class="mkm-collage__item"
            :class="{ 'mkm-collage__item--wide': item.wide }"
          >
            <img :src="item.src" :alt="item.alt">
          </figure>
        </div>
      </div>
      <div v-else-if="'image' in slide" class="mkm-hero__media">
        <figure class="mkm-collage">
          <div class="mkm-collage__item mkm-collage__item--wide">
            <img :src="slide.image" :alt="slide.imageAlt">
          </div>
        </figure>
      </div>
    </div>
    </div>
    <div class="mkm-container mkm-slider-footer">
      <div class="mkm-slider-pagination" role="group" aria-label="Diapositives">
        <button
          v-for="(slide, slideIndex) in slides"
          :key="slide.id"
          type="button"
          class="mkm-slider-page"
          :aria-current="slideIndex === index ? 'true' : undefined"
          :aria-label="`Afficher la diapositive ${slideIndex + 1} : ${slide.eyebrow}`"
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
  </section>
</template>
