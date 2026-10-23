<script setup lang="ts">
import logo from '~/assets/brand/logo-blanc.png'
import { contacts, services } from '~/data/site'

const year = new Date().getFullYear()

const quickLinks = [
  { to: '/', label: 'Accueil' },
  { to: '/le-groupe', label: 'À propos' },
  { to: '/realisations', label: 'Nos réalisations' },
  { to: '/realisations', label: 'Nos projets' }
]

const address = [
  'GB avenue Yembelete',
  'Numéro 25, Immeuble Claudia Sasu',
  'Kinshasa, RDC'
]

</script>

<template>
  <footer class="mkm-footer">
    <div class="mkm-container mkm-footer__grid">
      <div class="mkm-footer__brand">
        <NuxtLink to="/" class="mkm-footer__logo">
          <img :src="logo" alt="Groupe MKM" width="1774" height="887">
        </NuxtLink>
        <p class="mkm-footer__text">
          Un groupe multisectoriel engagé à vous fournir des solutions innovantes et adaptées à vos besoins.
        </p>
        <ul class="mkm-footer__social">
          <li>
            <span class="mkm-footer__social-link" role="img" aria-label="Facebook">
              <svg viewBox="0 0 24 24" width="32" height="32" aria-hidden="true"><rect width="24" height="24" rx="5" fill="#1877F2" /><path fill="#fff" d="M13.4 19v-6.1h2l.3-2.3h-2.3V9.2c0-.7.2-1.1 1.2-1.1h1.3V6.1c-.2 0-1-.1-1.9-.1-1.9 0-3.2 1.2-3.2 3.3v1.3H9v2.3h2.1V19h2.3z" /></svg>
            </span>
          </li>
          <li>
            <span class="mkm-footer__social-link" role="img" aria-label="Instagram">
              <svg viewBox="0 0 24 24" width="32" height="32" aria-hidden="true"><defs><radialGradient id="mkm-ig" cx="30%" cy="107%" r="130%"><stop offset="0%" stop-color="#feda75" /><stop offset="25%" stop-color="#fa7e1e" /><stop offset="50%" stop-color="#d62976" /><stop offset="75%" stop-color="#962fbf" /><stop offset="100%" stop-color="#4f5bd5" /></radialGradient></defs><rect width="24" height="24" rx="5" fill="url(#mkm-ig)" /><rect x="6.2" y="6.2" width="11.6" height="11.6" rx="3.4" fill="none" stroke="#fff" stroke-width="1.6" /><circle cx="12" cy="12" r="2.5" fill="none" stroke="#fff" stroke-width="1.6" /><circle cx="16.3" cy="7.7" r=".9" fill="#fff" /></svg>
            </span>
          </li>
          <li>
            <span class="mkm-footer__social-link" role="img" aria-label="LinkedIn">
              <svg viewBox="0 0 24 24" width="32" height="32" aria-hidden="true"><rect width="24" height="24" rx="5" fill="#0A66C2" /><path fill="#fff" d="M6.7 9.5H4.6v8.7h2.1V9.5zM5.6 5.2a1.25 1.25 0 1 0 .02 2.5 1.25 1.25 0 0 0-.02-2.5zM19.4 18.2h-2.1v-4.2c0-1 0-2.3-1.4-2.3s-1.6 1.1-1.6 2.2v4.3h-2.1V9.5h2v1.2h.1c.3-.5 1-1.2 2.1-1.2 2.3 0 2.7 1.5 2.7 3.5v5.2z" /></svg>
            </span>
          </li>
          <li>
            <span class="mkm-footer__social-link" role="img" aria-label="YouTube">
              <svg viewBox="0 0 24 24" width="32" height="32" aria-hidden="true"><rect width="24" height="24" rx="5" fill="#FF0000" /><path fill="#fff" d="M10 8.4v7.2l6.4-3.6L10 8.4z" /></svg>
            </span>
          </li>
        </ul>
      </div>

      <div>
        <h2 class="mkm-footer__title">Liens rapides</h2>
        <ul class="mkm-footer__links">
          <li v-for="item in quickLinks" :key="item.label">
            <NuxtLink :to="item.to">{{ item.label }}</NuxtLink>
          </li>
        </ul>
      </div>

      <div>
        <h2 class="mkm-footer__title">Nos services</h2>
        <ul class="mkm-footer__links mkm-footer__services">
          <li v-for="service in services" :key="service.slug">
            <NuxtLink :to="`/services/${service.slug}`">{{ service.title }}</NuxtLink>
          </li>
        </ul>
      </div>

      <div>
        <h2 class="mkm-footer__title">Nous contacter</h2>
        <ul class="mkm-footer__contact">
          <li>
            <a :href="contacts.phoneHref">
              <MkmIcon name="phone" size="small" />
              <span>{{ contacts.phoneLabel }}</span>
            </a>
          </li>
          <li>
            <a :href="contacts.emailHref">
              <MkmIcon name="mail" size="small" />
              <span>{{ contacts.email }}</span>
            </a>
          </li>
          <li>
            <p class="mkm-footer__address">
              <MkmIcon name="pin" size="small" />
              <span>
                <span v-for="line in address" :key="line">{{ line }}</span>
              </span>
            </p>
          </li>
        </ul>
      </div>

      <div>
        <h2 class="mkm-footer__title">Newsletter</h2>
        <p class="mkm-footer__text">Abonnez-vous pour recevoir nos actualités et offres exclusives.</p>
        <form class="mkm-footer__form" @submit.prevent>
          <label class="mkm-sr-only" for="newsletter-email">Adresse e-mail</label>
          <input id="newsletter-email" type="email" name="email" placeholder="Votre adresse e-mail" autocomplete="email">
          <button type="submit" aria-label="S’inscrire à la newsletter">
            <MkmIcon name="arrow-right" size="small" />
          </button>
        </form>
      </div>
    </div>

    <div class="mkm-container mkm-footer__legal">
      <p>© {{ year }} Groupe MKM. Tous droits réservés.</p>
      <nav aria-label="Informations légales">
        <NuxtLink to="/mentions-legales">Mentions légales</NuxtLink>
        <span aria-hidden="true">|</span>
        <NuxtLink to="/confidentialite">Politique de confidentialité</NuxtLink>
      </nav>
    </div>
  </footer>
</template>
