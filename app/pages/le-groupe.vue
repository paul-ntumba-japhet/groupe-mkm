<script setup lang="ts">
import portrait from '~/assets/image/sliders/michee-makengo.png'
import imageEngins from '~/assets/image/group/location_engins.png'
import imageLivres from '~/assets/image/group/livres.png'
import imageImmeuble from '~/assets/image/group/immobilier.png'
import imageTransport from '~/assets/image/group/transport.png'
import imageNumerique from '~/assets/image/group/numeriques.png'
import imageAccompagnement from '~/assets/image/group/accompagnement.png'
import imageCollaboration from '~/assets/image/group/collaboration_projet_client.png'
import photoMarieFrance from '~/assets/image/group-equipe/marie-france.png'
import photoBenMbongo from '~/assets/image/group-equipe/ben-mbongo.png'
import photoTychique from '~/assets/image/group-equipe/tychique-tuba.png'
import photoBeaker from '~/assets/image/group-equipe/beraka-mabanza.png'
import photoJennifer from '~/assets/image/group-equipe/jennifer-veko.png'
import photoPhilia from '~/assets/image/group-equipe/philia-mbanza.png'
import photoSami from '~/assets/image/group-equipe/sami-milambo.png'
import photoJaphet from '~/assets/image/group-equipe/japhet-ntumba.png'

useSeoMeta({
  title: 'Le groupe · Groupe MKM',
  description: 'GROUPE MKM, entreprise congolaise multisectorielle basée à Kinshasa : expertises, mission, vision et valeurs.'
})

const domaines = [
  {
    slug: 'location-engins',
    title: 'Location d’engins et de camions',
    text: 'Équipements adaptés aux travaux de construction, au terrassement et aux besoins concrets des chantiers.',
    link: 'Découvrir nos engins',
    image: imageEngins
  },
  {
    slug: 'educ-afrik',
    title: 'Livres et fournitures',
    text: 'Ouvrages, fournitures scolaires et ressources pédagogiques, avec distribution et impression à la demande.',
    link: 'Découvrir l’offre éducative',
    image: imageLivres
  },
  {
    slug: 'immobilier',
    title: 'Immobilier',
    text: 'Accompagnement pour l’achat, la vente, la location et la gestion de biens immobiliers.',
    link: 'Explorer nos services immobiliers',
    image: imageImmeuble
  },
  {
    slug: 'logistique-transport',
    title: 'Logistique et transport',
    text: 'Solutions adaptées à l’acheminement des marchandises, des matériaux et des équipements.',
    link: 'Découvrir nos solutions logistiques',
    image: imageTransport
  },
  {
    slug: 'solutions-numeriques',
    title: 'Solutions numériques',
    text: 'Sites web, applications mobiles et services d’hébergement adaptés à vos projets.',
    link: 'Explorer nos solutions numériques',
    image: imageNumerique
  },
  {
    slug: 'conseil',
    title: 'Conseil & accompagnement',
    text: 'Appui aux entrepreneurs et aux organisations pour structurer leurs projets et développer leurs activités.',
    link: 'Découvrir notre accompagnement',
    image: imageAccompagnement
  }
]

const valeurs = [
  {
    title: 'Professionnalisme',
    text: 'Des prestations clairement définies, menées avec méthode et rigueur.',
    icon: 'file-check'
  },
  {
    title: 'Intégrité',
    text: 'Des échanges honnêtes et transparents sur nos capacités et nos interventions.',
    icon: 'shield-check'
  },
  {
    title: 'Innovation',
    text: 'Des solutions utiles et des méthodes adaptées aux réalités du terrain.',
    icon: 'lightbulb'
  },
  {
    title: 'Engagement',
    text: 'Une implication attentive, un suivi régulier et des clients informés.',
    icon: 'handshake'
  },
  {
    title: 'Satisfaction du client',
    text: 'Une écoute active et la prise en compte de vos retours pour améliorer nos services.',
    icon: 'message'
  }
]

const equipe = [
  { name: 'Marie France', role: 'Responsable administrative et financière', image: photoMarieFrance },
  { name: 'Ben Mbongo', role: 'Responsable de la communication et multimédia', image: photoBenMbongo },
  { name: 'Tychique Meba', role: 'Responsable logistique et transport', image: photoTychique },
  { name: 'Beaker Mabanza', role: 'Responsable clientèle', image: photoBeaker },
  { name: 'Jennifer Veko', role: 'Responsable clientèle adjointe', image: photoJennifer },
  { name: 'Philia Mbanza', role: 'Responsable logistique', image: photoPhilia },
  { name: 'Sami Milambo', role: 'Responsable du livre et chargé de la distribution', image: photoSami },
  { name: 'Japhet Ntumba', role: 'Responsable des services numériques', image: photoJaphet }
]

const engagements = [
  {
    title: 'Comprendre votre besoin',
    text: 'Prendre en compte vos objectifs, vos contraintes et le contexte du projet.'
  },
  {
    title: 'Réunir les bonnes expertises',
    text: 'Identifier les services pertinents et les associer lorsque le projet le nécessite.'
  },
  {
    title: 'Accompagner la réalisation',
    text: 'Privilégier des échanges clairs et un suivi adapté à la prestation.'
  }
]

const etapes = [
  {
    title: 'Écouter et comprendre',
    text: 'Échanger sur votre projet, vos objectifs, votre budget et vos contraintes.'
  },
  {
    title: 'Définir la solution',
    text: 'Identifier les services et les moyens nécessaires, puis préciser le périmètre de notre intervention.'
  },
  {
    title: 'Réaliser et suivre',
    text: 'Coordonner les prestations convenues et maintenir une communication claire pendant leur réalisation.'
  },
  {
    title: 'Évaluer et améliorer',
    text: 'Recueillir vos observations et identifier les améliorations utiles pour la suite.'
  }
]
</script>

<template>
  <article class="mkm-groupe">
    <section class="mkm-section" aria-labelledby="titre-groupe">
      <div class="mkm-container mkm-groupe__intro">
        <div>
          <p class="mkm-eyebrow mkm-eyebrow--rule">Groupe MKM</p>
          <h1 id="titre-groupe">Un partenaire unique pour des solutions multiples.</h1>
          <p>
            Basé à Kinshasa, GROUPE MKM accompagne les entreprises, les institutions et les particuliers dans leurs projets.
          </p>
          <NuxtLink to="#titre-projet" class="mkm-btn mkm-btn--primary">
            Parlons de votre projet
            <MkmIcon name="arrow-right" />
          </NuxtLink>
        </div>
        <figure class="mkm-groupe__figure">
          <img
            class="mkm-groupe__portrait"
            :src="portrait"
            alt="Mafua Makengo Michée, professionnel et entrepreneur congolais en gestion financière et administrative."
            width="1254"
            height="1254"
          >
          <figcaption class="mkm-groupe__legend">
            <strong>Mafua Makengo Michée</strong>
            Professionnel et entrepreneur congolais en Gestion financière et administrative.
          </figcaption>
        </figure>
      </div>
    </section>

    <section class="mkm-section mkm-section--divided" aria-labelledby="titre-qui">
      <div class="mkm-container">
        <div class="mkm-groupe__qui">
        <div class="mkm-groupe__identite">
          <p class="mkm-eyebrow mkm-eyebrow--rule">Qui sommes-nous ?</p>
          <h2 id="titre-qui">Un groupe congolais, des expertises complémentaires</h2>
          <p>
            GROUPE MAFUA MAKENGO MANAGEMENT, en sigle <strong>GROUPE MKM</strong>, est une entreprise congolaise implantée à Kinshasa. Notre vocation est multisectorielle : plusieurs métiers sont réunis dans un même groupe, afin d’accompagner des projets qui ne se limitent pas à un seul domaine. Le groupe y concentre ses équipes et l’essentiel de ses moyens.
          </p>
          <p>
            Nous répondons aux besoins des entreprises, des institutions et des particuliers qui portent un projet. Équiper un chantier, transporter des marchandises ou développer une présence numérique : ces situations demandent des moyens différents. Nous choisissons le service qui correspond, plutôt qu’une offre identique pour tous.
          </p>
          <p>
            Notre approche tient en trois temps. Nous commençons par comprendre le besoin, son contexte et ses contraintes. Nous proposons ensuite une réponse adaptée aux moyens réellement nécessaires. Puis nous accompagnons la réalisation, en maintenant un échange clair jusqu’à l’aboutissement du projet.
          </p>
        </div>
        <aside class="mkm-groupe__bref" aria-labelledby="titre-bref">
          <h3 id="titre-bref">Le groupe en bref</h3>
          <ul class="mkm-groupe__reperes">
            <li>
              <MkmIcon name="pin" size="small" />
              <p><strong>Notre ancrage</strong> Kinshasa, République démocratique du Congo</p>
            </li>
            <li>
              <MkmIcon name="layers" size="small" />
              <p><strong>Nos métiers</strong> Six domaines d’expertise complémentaires</p>
            </li>
            <li>
              <MkmIcon name="users" size="small" />
              <p><strong>Nos clients</strong> Entreprises, institutions et particuliers</p>
            </li>
            <li>
              <MkmIcon name="flag" size="small" />
              <p><strong>Notre ambition</strong> Contribuer au développement économique en RDC et en Afrique</p>
            </li>
          </ul>
        </aside>
        </div>
        <ul class="mkm-groupe__engagements">
        <li v-for="item in engagements" :key="item.title">
          <h3>{{ item.title }}</h3>
          <p>{{ item.text }}</p>
        </li>
      </ul>
      </div>
    </section>

    <section class="mkm-section mkm-section--divided" aria-labelledby="titre-expertises-groupe">
      <div class="mkm-container">
        <h2 id="titre-expertises-groupe">Des expertises complémentaires</h2>
        <p class="mkm-groupe__lead">Six domaines d’activité pour répondre à vos besoins, mobilisés individuellement ou associés selon les exigences de votre projet.</p>
        <ul class="mkm-groupe__domaines">
          <li v-for="item in domaines" :key="item.slug">
            <NuxtLink :to="`/services/${item.slug}`" class="mkm-groupe__domaine">
              <img class="mkm-groupe__domaine-media" :src="item.image" alt="" width="1024" height="1024">
              <span class="mkm-groupe__domaine-body">
                <h3>{{ item.title }}</h3>
                <p>{{ item.text }}</p>
                <span class="mkm-groupe__service-link">
                  {{ item.link }}
                  <MkmIcon name="arrow-right" size="small" />
                </span>
              </span>
            </NuxtLink>
          </li>
        </ul>
      </div>
    </section>

    <section class="mkm-section mkm-section--divided mkm-groupe__equipe-section" aria-labelledby="titre-equipe">
      <div class="mkm-container">
        <header class="mkm-groupe__equipe-intro">
          <p class="mkm-eyebrow mkm-eyebrow--rule">Notre équipe</p>
          <h2 id="titre-equipe">Des interlocuteurs pour chaque étape de votre projet</h2>
          <p>Huit responsables, chacun dans son domaine, pour vous accompagner avec clarté de la première demande jusqu’à la réalisation.</p>
        </header>
        <ul class="mkm-groupe__equipe">
          <li v-for="person in equipe" :key="person.name">
            <img :src="person.image" :alt="`${person.name}, ${person.role}.`">
            <h3>{{ person.name }}</h3>
            <p>{{ person.role }}</p>
          </li>
        </ul>
        <div class="mkm-groupe__equipe-cta">
          <p>Vous avez un projet en tête ? Présentez-le à l’équipe : nous vous orienterons vers l’interlocuteur adapté.</p>
          <NuxtLink to="/contact" class="mkm-btn mkm-btn--primary">Contacter l’équipe</NuxtLink>
        </div>
      </div>
    </section>

    <section class="mkm-section mkm-section--divided" aria-label="Mission et vision">
      <div class="mkm-container mkm-groupe__split">
        <article class="mkm-groupe__panneau mkm-groupe__panneau--mission">
          <MkmIcon name="target" />
          <p class="mkm-groupe__panneau-label">Notre mission</p>
          <h2 id="titre-mission">Accompagner vos projets au quotidien</h2>
          <p>Nous accompagnons nos clients dans la réalisation de leurs projets. Nous leur proposons des services fiables, des solutions adaptées à chaque situation et un suivi professionnel, de la première demande jusqu’à la concrétisation.</p>
          <p>Cette mission commence par l’écoute. Comprendre les objectifs, les contraintes et les priorités de chaque client nous permet de proposer une réponse pertinente et de définir clairement les modalités de notre intervention.</p>
        </article>
        <article class="mkm-groupe__panneau mkm-groupe__panneau--vision">
          <MkmIcon name="horizon" />
          <p class="mkm-groupe__panneau-label">Notre vision</p>
          <h2 id="titre-vision">Construire un groupe de référence</h2>
          <p>Nous construisons un groupe congolais solide, capable de durer et de répondre aux besoins de ses clients. Le développement commence en République démocratique du Congo, puis se poursuit en Afrique, au rythme de nos expertises et de relations de confiance.</p>
          <p>Cette ambition reste ancrée dans le tissu économique local. Elle consiste à soutenir les initiatives entrepreneuriales, à faciliter l’accès aux services et à contribuer à la réalisation des projets.</p>
        </article>
      </div>
    </section>

    <section class="mkm-section mkm-section--divided" aria-labelledby="titre-valeurs">
      <div class="mkm-container mkm-groupe__valeurs">
        <div class="mkm-groupe__valeurs-intro">
          <p class="mkm-eyebrow mkm-eyebrow--rule">Nos valeurs</p>
          <h2 id="titre-valeurs">Les valeurs qui guident nos actions</h2>
          <p>Cinq principes pour orienter nos décisions, nos relations et notre manière de vous accompagner.</p>
        </div>
        <ul class="mkm-groupe__valeurs-liste">
          <li v-for="item in valeurs" :key="item.title">
            <MkmIcon :name="item.icon" />
            <div>
              <h3>{{ item.title }}</h3>
              <p>{{ item.text }}</p>
            </div>
          </li>
        </ul>
      </div>
    </section>

    <section class="mkm-section mkm-section--divided" aria-labelledby="titre-accompagnement">
      <div class="mkm-container">
        <h2 id="titre-accompagnement">Notre manière de vous accompagner</h2>
        <p class="mkm-groupe__parcours-lead">Du premier échange au bilan, une démarche claire pour faire avancer votre projet.</p>
        <ol class="mkm-groupe__etapes">
          <li v-for="(item, index) in etapes" :key="item.title">
            <span class="mkm-groupe__numero">{{ String(index + 1).padStart(2, '0') }}</span>
            <h3>{{ item.title }}</h3>
            <p>{{ item.text }}</p>
          </li>
        </ol>
      </div>
    </section>

    <section class="mkm-section mkm-groupe__cta-section" aria-labelledby="titre-projet">
      <div class="mkm-container mkm-groupe__cta">
        <div class="mkm-groupe__cta-copy">
          <h2 id="titre-projet">Parlons de votre projet</h2>
          <p>Vous recherchez un équipement, un service ou un accompagnement pour concrétiser votre projet ? Présentez-nous votre besoin afin d’étudier ensemble une réponse adaptée.</p>
          <div class="mkm-actions">
            <NuxtLink to="/contact" class="mkm-btn mkm-btn--primary">Nous contacter</NuxtLink>
            <NuxtLink to="/devis" class="mkm-btn mkm-btn--secondary">Demander un devis</NuxtLink>
          </div>
        </div>
        <img
          class="mkm-groupe__cta-media"
          :src="imageCollaboration"
          alt="Deux personnes échangent autour des plans d’un projet."
        >
      </div>
    </section>
  </article>
</template>
