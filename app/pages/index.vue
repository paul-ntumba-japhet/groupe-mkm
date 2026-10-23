<script setup lang="ts">
import { chiffres, expertises, realisationsAccueil } from '~/data/site'

useSeoMeta({
  title: 'Accueil · Groupe MKM',
  description: 'Présentation du Groupe MKM à Kinshasa : six expertises et une sélection de réalisations.'
})
</script>

<template>
  <section class="mkm-section mkm-section--divided mkm-expertises" aria-labelledby="titre-services">
    <div class="mkm-container">
      <div class="mkm-section__head">
        <div>
          <p class="mkm-eyebrow mkm-eyebrow--rule">Nos expertises</p>
          <h2 id="titre-services">Six expertises pour accompagner vos projets</h2>
          <p class="mkm-lead mkm-muted">
            Des solutions complémentaires pour vos chantiers, vos activités et vos projets de développement.
          </p>
        </div>
      </div>
      <ul class="mkm-grid mkm-grid--expertises">
        <li v-for="expertise in expertises" :key="expertise.slug">
          <NuxtLink :to="`/services/${expertise.slug}`" class="mkm-card mkm-card--interactive">
            <img
              class="mkm-card__illustration"
              :src="expertise.illustration"
              alt=""
              width="1254"
              height="1254"
            >
            <h3 class="mkm-card__title">{{ expertise.title }}</h3>
            <p class="mkm-card__description">{{ expertise.description }}</p>
            <span class="mkm-card__link">
              {{ expertise.link }}
              <MkmIcon name="arrow-right" size="small" />
            </span>
          </NuxtLink>
        </li>
      </ul>
    </div>
  </section>

  <section class="mkm-section mkm-section--divided mkm-realisations" aria-labelledby="titre-realisations">
    <div class="mkm-container">
      <div class="mkm-section__head">
        <div>
          <p class="mkm-eyebrow mkm-eyebrow--rule">Nos réalisations</p>
          <h2 id="titre-realisations">Des projets concrets,<br>une expertise sur le terrain.</h2>
          <p class="mkm-lead mkm-muted">Découvrez comment nous accompagnons les projets de nos clients.</p>
        </div>
        <NuxtLink to="/realisations" class="mkm-btn mkm-btn--secondary">
          Toutes nos réalisations
          <MkmIcon name="arrow-right" />
        </NuxtLink>
      </div>
      <ul class="mkm-showcase">
        <li v-for="item in realisationsAccueil" :key="item.title" :class="{ 'mkm-showcase__item--lead': item.lead }">
          <NuxtLink to="/realisations" class="mkm-showcase__card">
            <img class="mkm-showcase__media" :src="item.image" :alt="item.alt" :width="item.width" :height="item.height" loading="lazy">
            <div class="mkm-showcase__body">
              <p class="mkm-showcase__activity">{{ item.activity }}</p>
              <h3 class="mkm-showcase__title">{{ item.title }}</h3>
              <p class="mkm-showcase__text">{{ item.description }}</p>
              <div class="mkm-showcase__foot">
                <p v-if="item.location" class="mkm-showcase__meta">
                  <MkmIcon name="pin" size="small" />
                  {{ item.location }}
                </p>
                <span class="mkm-showcase__link">
                  {{ item.link }}
                  <MkmIcon name="arrow-right" size="small" />
                </span>
              </div>
            </div>
          </NuxtLink>
        </li>
      </ul>
      <p class="mkm-realisations__note">Maquette — sélection des projets et informations à valider avec Groupe MKM.</p>
    </div>
  </section>

  <section class="mkm-section mkm-stats" aria-labelledby="titre-chiffres">
    <div class="mkm-container">
      <p class="mkm-eyebrow mkm-eyebrow--rule">En chiffres</p>
      <h2 id="titre-chiffres">Une expertise qui se traduit en actions</h2>
      <ul class="mkm-stats__list">
        <li v-for="item in chiffres" :key="item.label">
          <p class="mkm-stats__value">{{ item.value }}</p>
          <p class="mkm-stats__label">{{ item.label }}</p>
        </li>
      </ul>
    </div>
  </section>
</template>
