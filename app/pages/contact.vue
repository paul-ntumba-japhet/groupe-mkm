<script setup lang="ts">
import { contacts } from '~/data/site'

useSeoMeta({
  title: 'Contact · Groupe MKM',
  description: 'Écrivez à Groupe MKM à Kinshasa : formulaire, téléphone, e-mail et WhatsApp.'
})

const route = useRoute()
const sujets = ['Proposer un bien', 'Rechercher un bien']
const sujetInitial = typeof route.query.sujet === 'string' && sujets.includes(route.query.sujet) ? route.query.sujet : ''
const erreurs = ref<Record<string, string>>({})

function champ (formulaire: HTMLFormElement, nom: string) {
  return String(formulaire.elements.namedItem(nom) && (formulaire.elements.namedItem(nom) as HTMLInputElement).value || '').trim()
}

async function envoyer (event: Event) {
  const formulaire = event.target as HTMLFormElement
  const nom = champ(formulaire, 'nom')
  const email = champ(formulaire, 'email')
  const telephone = champ(formulaire, 'telephone')
  const sujet = champ(formulaire, 'sujet')
  const message = champ(formulaire, 'message')
  const suivantes: Record<string, string> = {}

  if (nom.length < 2) suivantes.nom = 'Indiquez votre nom.'
  if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)) suivantes.email = 'Indiquez une adresse e-mail valide.'
  if (telephone && telephone.length < 6) suivantes.telephone = 'Indiquez un numéro de téléphone utilisable, ou laissez le champ vide.'
  if (sujet.length < 3) suivantes.sujet = 'Indiquez le sujet de votre message.'
  if (message.length < 10) suivantes.message = 'Précisez votre message en quelques mots.'

  erreurs.value = suivantes
  if (Object.keys(suivantes).length) {
    await nextTick()
    const premier = formulaire.querySelector<HTMLElement>('[aria-invalid="true"]')
    premier?.focus()
    return
  }

  const corps = [
    `Nom : ${nom}`,
    `E-mail : ${email}`,
    telephone ? `Téléphone : ${telephone}` : '',
    '',
    message
  ].filter(ligne => ligne !== '').join('\n')

  window.location.href = `mailto:${contacts.email}?subject=${encodeURIComponent(sujet)}&body=${encodeURIComponent(corps)}`
}
</script>

<template>
  <section class="mkm-section" aria-labelledby="titre-contact">
    <div class="mkm-container">
      <p class="mkm-eyebrow">Contact</p>
      <h1 id="titre-contact">Contacter Groupe MKM</h1>
      <p class="mkm-lead">Écrivez-nous au sujet de votre activité ou de votre projet. Le message s’ouvre dans votre messagerie et est adressé à {{ contacts.email }}.</p>
      <div class="mkm-contact-page">
        <form class="mkm-contact-form" novalidate @submit.prevent="envoyer">
          <div class="mkm-contact-form__duo">
            <label class="mkm-field">
              <span class="mkm-label">Nom</span>
              <input
                class="mkm-input"
                type="text"
                name="nom"
                autocomplete="name"
                :aria-invalid="erreurs.nom ? 'true' : undefined"
                :aria-describedby="erreurs.nom ? 'erreur-nom' : undefined"
              >
              <p v-if="erreurs.nom" id="erreur-nom" class="mkm-error" role="alert">{{ erreurs.nom }}</p>
            </label>
            <label class="mkm-field">
              <span class="mkm-label">E-mail</span>
              <input
                class="mkm-input"
                type="email"
                name="email"
                autocomplete="email"
                :aria-invalid="erreurs.email ? 'true' : undefined"
                :aria-describedby="erreurs.email ? 'erreur-email' : undefined"
              >
              <p v-if="erreurs.email" id="erreur-email" class="mkm-error" role="alert">{{ erreurs.email }}</p>
            </label>
          </div>
          <div class="mkm-contact-form__duo">
            <label class="mkm-field">
              <span class="mkm-label">Téléphone <span class="mkm-help">(facultatif)</span></span>
              <input
                class="mkm-input"
                type="tel"
                name="telephone"
                autocomplete="tel"
                :aria-invalid="erreurs.telephone ? 'true' : undefined"
                :aria-describedby="erreurs.telephone ? 'erreur-telephone' : undefined"
              >
              <p v-if="erreurs.telephone" id="erreur-telephone" class="mkm-error" role="alert">{{ erreurs.telephone }}</p>
            </label>
            <label class="mkm-field">
              <span class="mkm-label">Sujet</span>
              <input
                class="mkm-input"
                type="text"
                name="sujet"
                :value="sujetInitial"
                :aria-invalid="erreurs.sujet ? 'true' : undefined"
                :aria-describedby="erreurs.sujet ? 'erreur-sujet' : undefined"
              >
              <p v-if="erreurs.sujet" id="erreur-sujet" class="mkm-error" role="alert">{{ erreurs.sujet }}</p>
            </label>
          </div>
          <label class="mkm-field">
            <span class="mkm-label">Message</span>
            <textarea
              class="mkm-textarea"
              name="message"
              :aria-invalid="erreurs.message ? 'true' : undefined"
              :aria-describedby="erreurs.message ? 'aide-message erreur-message' : 'aide-message'"
            />
            <p id="aide-message" class="mkm-help">N’indiquez pas de mot de passe dans ce message.</p>
            <p v-if="erreurs.message" id="erreur-message" class="mkm-error" role="alert">{{ erreurs.message }}</p>
          </label>
          <button type="submit" class="mkm-btn mkm-btn--primary">Envoyer le message</button>
        </form>
        <aside class="mkm-contact-aside" aria-labelledby="titre-coordonnees">
          <h2 id="titre-coordonnees">Coordonnées</h2>
          <ContactLinks />
        </aside>
      </div>
    </div>
  </section>
</template>
