<script setup lang="ts">
import { contacts } from '~/data/site'
import { findEngin } from '~/data/engins'

const route = useRoute()
const slug = computed(() => String(route.params.engin ?? ''))
const engin = computed(() => findEngin(slug.value))
const dureesLocation = [
  { value: 'Jour', label: 'Jour' },
  { value: 'Semaine', label: 'Semaine' },
  { value: 'Mois', label: 'Mois' }
]
const dureeManquante = ref(false)

if (!engin.value) {
  throw createError({ statusCode: 404, statusMessage: 'Engin introuvable' })
}

useSeoMeta({
  title: () => engin.value ? `${engin.value.name} · Groupe MKM` : 'Engin introuvable · Groupe MKM',
  description: () => engin.value?.role ?? ''
})

function preparerDemande(event: Event) {
  const formulaire = event.target as HTMLFormElement
  const donnees = new FormData(formulaire)
  const durees = donnees.getAll('duree').map(String)
  if (!durees.length || !engin.value) {
    dureeManquante.value = true
    return
  }
  dureeManquante.value = false
  const message = [
    `Engin : ${engin.value.name}`,
    `Rôle : ${engin.value.role}`,
    `Durée estimée : ${durees.join(', ')}`,
    `Date de début souhaitée : ${donnees.get('debut')}`,
    `Lieu du chantier : ${donnees.get('lieu')}`,
    '',
    'Détails :',
    String(donnees.get('details') ?? '')
  ].join('\n')
  const sujet = `Demande de location — ${engin.value.name}`
  window.location.href = `mailto:${contacts.email}?subject=${encodeURIComponent(sujet)}&body=${encodeURIComponent(message)}`
}
</script>

<template>
  <section v-if="engin" class="mkm-section mkm-engin-fiche" aria-labelledby="titre-fiche-engin">
    <div class="mkm-container">
      <NuxtLink to="/services/location-engins" class="mkm-engin-fiche__retour">Location d’engins</NuxtLink>
      <div class="mkm-engin-fiche__intro">
        <img :src="engin.image" :alt="engin.alt">
        <div>
          <h1 id="titre-fiche-engin">{{ engin.name }}</h1>
          <p>{{ engin.role }}</p>
          <ul class="mkm-engin-fiche__apercus">
            <li v-for="item in engin.specs" :key="item.label">
              <MkmIcon :name="item.icon" />
              <span>{{ item.label }}</span>
            </li>
          </ul>
          <h2>Travaux adaptés</h2>
          <ul class="mkm-engin-fiche__travaux">
            <li v-for="item in engin.works" :key="item">{{ item }}</li>
          </ul>
          <div class="mkm-actions">
            <NuxtLink to="/devis" class="mkm-btn mkm-btn--primary">Demander un devis</NuxtLink>
          </div>
        </div>
      </div>
    </div>
  </section>

  <section v-if="engin" class="mkm-section mkm-section--divided mkm-engin-fiche" aria-labelledby="titre-caracteristiques-engin">
    <div class="mkm-container mkm-engin-fiche__bilan">
      <div>
        <h2 id="titre-caracteristiques-engin">Caractéristiques</h2>
        <div class="mkm-engin-fiche__tableau">
          <table>
            <thead>
              <tr>
                <th scope="col">Caractéristique</th>
                <th scope="col">Valeur</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="row in engin.sheet" :key="row.label">
                <th scope="row">{{ row.label }}</th>
                <td>{{ row.value }}</td>
              </tr>
            </tbody>
          </table>
        </div>
        <p class="mkm-engin-fiche__note">{{ engin.note }}</p>
        <p v-if="engin.sourceUrl" class="mkm-engin-fiche__source">
          Source :
          <a :href="engin.sourceUrl" rel="noopener noreferrer" target="_blank">{{ engin.sourceName }}</a>
        </p>
        <p v-else class="mkm-engin-fiche__source">Source : {{ engin.sourceName }}</p>
      </div>
      <form class="mkm-engin-fiche__offre" @submit.prevent="preparerDemande">
        <h2 id="titre-offre-location">Demander une offre de location</h2>
        <p class="mkm-engin-fiche__offre-nom">{{ engin.name }}</p>
        <p class="mkm-engin-fiche__offre-role">{{ engin.role }}</p>
        <fieldset class="mkm-engin-fiche__durees" :aria-invalid="dureeManquante || undefined">
          <legend class="mkm-label">Durée estimée de location</legend>
          <label v-for="item in dureesLocation" :key="item.value">
            <input type="checkbox" name="duree" :value="item.value" @change="dureeManquante = false">
            <span>{{ item.label }}</span>
          </label>
        </fieldset>
        <p v-if="dureeManquante" class="mkm-error" role="alert">Choisissez au moins une durée.</p>
        <label class="mkm-field">
          <span class="mkm-label">Date de début souhaitée</span>
          <input class="mkm-input" type="date" name="debut" required>
        </label>
        <label class="mkm-field">
          <span class="mkm-label">Lieu du chantier</span>
          <input class="mkm-input" type="text" name="lieu" required autocomplete="off">
        </label>
        <label class="mkm-field">
          <span class="mkm-label">Nature du chantier et contraintes d’accès</span>
          <textarea class="mkm-textarea" name="details" required />
        </label>
        <button type="submit" class="mkm-btn mkm-btn--primary">Préparer la demande de devis</button>
      </form>
    </div>
  </section>
</template>
