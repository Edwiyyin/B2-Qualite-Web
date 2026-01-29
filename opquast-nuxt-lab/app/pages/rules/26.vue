<template>
  <div class="min-h-screen bg-zinc-950 p-6">
    <div class="max-w-6xl mx-auto">
      <!-- Header -->
      <div class="text-center mb-8">
        <h1 class="text-4xl font-bold text-zinc-100 mb-2">
          Messages d'Erreur d'Authentification Sécurisés
        </h1>
        <p class="text-zinc-400">
          Prévenir l'usurpation de comptes en ne révélant pas l'existence des
          utilisateurs
        </p>
      </div>

      <!-- Main Content -->
      <div class="space-y-8">
        <!-- Why This Matters -->
        <div
          class="bg-zinc-900 rounded-lg shadow-md p-6 border border-zinc-800"
        >
          <h2 class="text-xl font-bold text-zinc-100 mb-3">
            Pourquoi c'est Important ?
          </h2>
          <div class="space-y-3">
            <p class="text-zinc-400">
              Les messages d'erreur qui révèlent l'existence ou l'absence d'un
              compte permettent aux attaquants de :
            </p>
            <ul class="space-y-2 ml-4">
              <li class="flex items-start">
                <span class="text-red-400 mr-3">✕</span>
                <span class="text-zinc-300">
                  <strong>Énumérer les comptes utilisateurs</strong> en testant
                  plusieurs adresses email
                </span>
              </li>
              <li class="flex items-start">
                <span class="text-red-400 mr-3">✕</span>
                <span class="text-zinc-300">
                  <strong>Cibler les attaques</strong> sur les comptes qui
                  existent réellement
                </span>
              </li>
              <li class="flex items-start">
                <span class="text-red-400 mr-3">✕</span>
                <span class="text-zinc-300">
                  <strong>Usurper l'identité</strong> avec une connaissance
                  partielle des comptes
                </span>
              </li>
              <li class="flex items-start">
                <span class="text-red-400 mr-3">✕</span>
                <span class="text-zinc-300">
                  <strong>Constituer des listes</strong> d'adresses email
                  valides à revendre
                </span>
              </li>
            </ul>
          </div>
        </div>

        <!-- Comparison: Insecure vs Secure -->
        <div class="grid md:grid-cols-2 gap-6">
          <!-- Insecure Messages -->
          <div
            class="bg-zinc-900 rounded-lg shadow-md p-6 border border-red-900"
          >
            <h2 class="text-xl font-bold text-red-400 mb-4">
              ❌ Messages Dangereux
            </h2>
            <div class="space-y-4">
              <div
                v-for="(scenario, index) in insecureScenarios"
                :key="index"
                class="bg-red-950 p-4 rounded border border-red-900"
              >
                <p class="text-sm font-bold text-zinc-300 mb-2">
                  {{ scenario.situation }}
                </p>
                <div
                  class="bg-zinc-950 p-3 rounded text-sm border border-red-900"
                >
                  <p class="text-red-400">{{ scenario.badMessage }}</p>
                </div>
                <p class="text-xs text-red-300 mt-2">
                  <strong>Risque :</strong> {{ scenario.risk }}
                </p>
              </div>
            </div>
          </div>

          <!-- Secure Messages -->
          <div
            class="bg-zinc-900 rounded-lg shadow-md p-6 border border-green-900"
          >
            <h2 class="text-xl font-bold text-green-400 mb-4">
              ✓ Messages Sécurisés
            </h2>
            <div class="space-y-4">
              <div
                v-for="(scenario, index) in insecureScenarios"
                :key="index"
                class="bg-green-950 p-4 rounded border border-green-900"
              >
                <p class="text-sm font-bold text-zinc-300 mb-2">
                  {{ scenario.situation }}
                </p>
                <div
                  class="bg-zinc-950 p-3 rounded text-sm border border-green-900"
                >
                  <p class="text-green-400">{{ scenario.goodMessage }}</p>
                </div>
                <p class="text-xs text-green-300 mt-2">
                  <strong>Avantage :</strong> {{ scenario.advantage }}
                </p>
              </div>
            </div>
          </div>
        </div>

        <!-- Interactive Demonstrations -->
        <div
          class="bg-zinc-900 rounded-lg shadow-md p-6 border border-zinc-800"
        >
          <h2 class="text-xl font-bold text-zinc-100 mb-4">
            Démonstrations Interactives
          </h2>
          <p class="text-zinc-400 mb-6">
            Testez vous-même en entrant des identifiants pour voir comment se
            comportent les deux approches.
          </p>

          <div class="grid md:grid-cols-2 gap-6">
            <!-- Insecure Demo -->
            <div class="border border-red-900 rounded-lg p-6 bg-red-950">
              <h3 class="text-lg font-bold text-red-400 mb-4">
                ❌ Approche Non Sécurisée
              </h3>
              <div class="space-y-4">
                <input
                  v-model="insecureEmail"
                  type="email"
                  placeholder="Adresse email"
                  class="w-full bg-zinc-800 border border-zinc-700 rounded px-3 py-2 text-zinc-100 placeholder-zinc-500 focus:outline-none focus:ring-2 focus:ring-red-500"
                />
                <input
                  v-model="insecurePassword"
                  type="password"
                  placeholder="Mot de passe"
                  class="w-full bg-zinc-800 border border-zinc-700 rounded px-3 py-2 text-zinc-100 placeholder-zinc-500 focus:outline-none focus:ring-2 focus:ring-red-500"
                />
                <button
                  @click="checkInsecureLogin"
                  class="w-full bg-red-900 hover:bg-red-800 text-red-100 font-bold py-2 px-4 rounded transition"
                >
                  Se connecter
                </button>

                <!-- Insecure Response -->
                <div
                  v-if="insecureResponse"
                  :class="[
                    'p-4 rounded border-l-4',
                    insecureResponse.isError
                      ? 'bg-red-950 border-red-500 text-red-300'
                      : 'bg-green-950 border-green-500 text-green-300',
                  ]"
                >
                  <p class="font-semibold">{{ insecureResponse.message }}</p>
                  <p v-if="insecureResponse.detail" class="text-sm mt-1">
                    {{ insecureResponse.detail }}
                  </p>
                </div>

                <!-- Info about test accounts -->
                <div
                  class="bg-zinc-800 p-3 rounded border border-zinc-700 text-xs text-zinc-400"
                >
                  <p class="font-semibold text-zinc-300 mb-1">
                    Comptes de test :
                  </p>
                  <p>Email valide : user@example.com</p>
                  <p>Mot de passe : password123</p>
                </div>
              </div>
            </div>

            <!-- Secure Demo -->
            <div class="border border-green-900 rounded-lg p-6 bg-green-950">
              <h3 class="text-lg font-bold text-green-400 mb-4">
                ✓ Approche Sécurisée
              </h3>
              <div class="space-y-4">
                <input
                  v-model="secureEmail"
                  type="email"
                  placeholder="Adresse email"
                  class="w-full bg-zinc-800 border border-zinc-700 rounded px-3 py-2 text-zinc-100 placeholder-zinc-500 focus:outline-none focus:ring-2 focus:ring-green-500"
                />
                <input
                  v-model="securePassword"
                  type="password"
                  placeholder="Mot de passe"
                  class="w-full bg-zinc-800 border border-zinc-700 rounded px-3 py-2 text-zinc-100 placeholder-zinc-500 focus:outline-none focus:ring-2 focus:ring-green-500"
                />
                <button
                  @click="checkSecureLogin"
                  class="w-full bg-green-900 hover:bg-green-800 text-green-100 font-bold py-2 px-4 rounded transition"
                >
                  Se connecter
                </button>

                <!-- Secure Response -->
                <div
                  v-if="secureResponse"
                  :class="[
                    'p-4 rounded border-l-4',
                    secureResponse.isError
                      ? 'bg-red-950 border-red-500 text-red-300'
                      : 'bg-green-950 border-green-500 text-green-300',
                  ]"
                >
                  <p class="font-semibold">{{ secureResponse.message }}</p>
                  <p v-if="secureResponse.detail" class="text-sm mt-1">
                    {{ secureResponse.detail }}
                  </p>
                </div>

                <!-- Info about test accounts -->
                <div
                  class="bg-zinc-800 p-3 rounded border border-zinc-700 text-xs text-zinc-400"
                >
                  <p class="font-semibold text-zinc-300 mb-1">
                    Comptes de test :
                  </p>
                  <p>Email valide : user@example.com</p>
                  <p>Mot de passe : password123</p>
                </div>
              </div>
            </div>
          </div>

          <!-- Observation -->
          <div class="mt-6 bg-zinc-800 p-4 rounded border border-zinc-700">
            <p class="text-zinc-300">
              <strong class="text-zinc-100">Observation :</strong> Remarquez que
              l'approche sécurisée affiche
              <em>exactement le même message</em> pour tous les cas d'erreur.
              L'attaquant ne peut pas distinguer si l'email existe ou si le mot
              de passe est faux.
            </p>
          </div>
        </div>

        <!-- Error Messages Reference -->
        <div
          class="bg-zinc-900 rounded-lg shadow-md p-6 border border-zinc-800"
        >
          <h2 class="text-xl font-bold text-zinc-100 mb-4">
            Tableau de Référence
          </h2>
          <div class="overflow-x-auto">
            <table class="w-full text-sm">
              <thead>
                <tr class="border-b border-zinc-700">
                  <th class="text-left py-2 px-3 text-zinc-300 font-semibold">
                    Situation
                  </th>
                  <th class="text-left py-2 px-3 text-red-400 font-semibold">
                    ❌ À Éviter
                  </th>
                  <th class="text-left py-2 px-3 text-green-400 font-semibold">
                    ✓ Recommandé
                  </th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(row, index) in referenceTable"
                  :key="index"
                  class="border-b border-zinc-800"
                >
                  <td class="py-3 px-3 text-zinc-300">{{ row.situation }}</td>
                  <td class="py-3 px-3 text-red-300 text-xs">
                    <code class="bg-red-950 px-2 py-1 rounded">
                      {{ row.bad }}
                    </code>
                  </td>
                  <td class="py-3 px-3 text-green-300 text-xs">
                    <code class="bg-green-950 px-2 py-1 rounded">
                      {{ row.good }}
                    </code>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>

        <!-- Best Practices -->
        <div
          class="bg-zinc-900 rounded-lg shadow-md p-6 border border-zinc-800"
        >
          <h2 class="text-xl font-bold text-zinc-100 mb-4">Bonnes Pratiques</h2>
          <ul class="space-y-3">
            <li class="flex items-start">
              <span class="text-green-400 mr-3">✓</span>
              <span class="text-zinc-300">
                Utiliser des messages
                <strong class="text-zinc-100">génériques</strong> pour tous les
                cas d'erreur d'authentification
              </span>
            </li>
            <li class="flex items-start">
              <span class="text-green-400 mr-3">✓</span>
              <span class="text-zinc-300">
                Ne jamais confirmer l'existence ou l'absence d'un compte
              </span>
            </li>
            <li class="flex items-start">
              <span class="text-green-400 mr-3">✓</span>
              <span class="text-zinc-300">
                Appliquer cette règle à
                <strong class="text-zinc-100">tous les formulaires</strong>
                (création, connexion, récupération mot de passe)
              </span>
            </li>
            <li class="flex items-start">
              <span class="text-green-400 mr-3">✓</span>
              <span class="text-zinc-300">
                Ajouter un
                <strong class="text-zinc-100"
                  >délai de réponse identique</strong
                >
                pour tous les cas (timing attack prevention)
              </span>
            </li>
            <li class="flex items-start">
              <span class="text-green-400 mr-3">✓</span>
              <span class="text-zinc-300">
                Implémenter le
                <strong class="text-zinc-100">rate limiting</strong> pour
                prévenir le brute force
              </span>
            </li>
            <li class="flex items-start">
              <span class="text-green-400 mr-3">✓</span>
              <span class="text-zinc-300">
                Enregistrer les tentatives échouées pour détecter les attaques
              </span>
            </li>
          </ul>
        </div>

        <!-- Implementation Checklist -->
        <div
          class="bg-zinc-900 rounded-lg shadow-md p-6 border border-zinc-800"
        >
          <h2 class="text-xl font-bold text-zinc-100 mb-4">
            Checklist d'Implémentation
          </h2>
          <div class="space-y-3">
            <label
              v-for="(item, index) in checklist"
              :key="index"
              class="flex items-center p-3 rounded bg-zinc-800 cursor-pointer hover:bg-zinc-700 transition"
            >
              <input
                type="checkbox"
                v-model="checklist[index].checked"
                class="w-4 h-4 rounded border-zinc-600"
              />
              <span class="ml-3 text-zinc-300">{{ item.text }}</span>
            </label>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const insecureEmail = ref('')
const insecurePassword = ref('')
const secureEmail = ref('')
const securePassword = ref('')

const insecureResponse = ref(null)
const secureResponse = ref(null)

const validEmail = 'user@example.com'
const validPassword = 'password123'

const insecureScenarios = [
  {
    situation: 'Création de compte - Email déjà utilisé',
    badMessage: '❌ Cet email est déjà utilisé. Utiliser une autre adresse.',
    goodMessage:
      '✓ Si vous avez déjà un compte, utilisez la récupération de mot de passe.',
    risk: "Révèle l'existence du compte",
    advantage: "Aucune information n'est divulguée",
  },
  {
    situation: 'Connexion - Mot de passe incorrect',
    badMessage: '❌ Mot de passe incorrect. Veuillez réessayer.',
    goodMessage:
      '✓ Identifiants incorrects. Veuillez vérifier votre adresse et mot de passe.',
    risk: "Confirme que l'email existe",
    advantage: 'Impossible de distinguer les erreurs',
  },
  {
    situation: 'Récupération mot de passe - Succès/Échec',
    badMessage:
      "❌ Un email de réinitialisation a été envoyé à cet email (ou Cet email n'existe pas)",
    goodMessage:
      '✓ Si un compte existe pour cette adresse, un email de réinitialisation a été envoyé.',
    risk: "Confirme l'existence du compte",
    advantage: 'Même message pour tous les cas',
  },
  {
    situation: 'Compte verrouillé après tentatives',
    badMessage: '❌ Votre compte a été verrouillé après 5 tentatives.',
    goodMessage:
      '✓ Impossible de se connecter. Réessayez plus tard ou utilisez la récupération de mot de passe.',
    risk: 'Révèle que le compte existe',
    advantage: "Pas d'indication sur le compte",
  },
]

const referenceTable = [
  {
    situation: 'Email inexistant à la création',
    bad: 'Utilisez cet email, il est nouveau.',
    good: 'Si vous avez déjà un compte, utilisez la récupération.',
  },
  {
    situation: 'Email valide + mauvais mot de passe',
    bad: 'Mot de passe incorrect.',
    good: 'Identifiants incorrects. Veuillez vérifier.',
  },
  {
    situation: 'Email inexistant à la connexion',
    bad: "Cet email n'est pas enregistré.",
    good: 'Identifiants incorrects. Veuillez vérifier.',
  },
  {
    situation: 'Récupération mot de passe - Email valide',
    bad: 'Email de réinitialisation envoyé.',
    good: 'Si un compte existe, un email a été envoyé.',
  },
  {
    situation: 'Récupération - Email inexistant',
    bad: "Cet email n'existe pas.",
    good: 'Si un compte existe, un email a été envoyé.',
  },
  {
    situation: 'Compte temporairement verrouillé',
    bad: 'Votre compte est verrouillé.',
    good: 'Impossible de se connecter. Réessayez plus tard.',
  },
]

const checkInsecureLogin = () => {
  if (!insecureEmail.value || !insecurePassword.value) {
    insecureResponse.value = {
      isError: true,
      message: 'Veuillez remplir tous les champs.',
      detail: null,
    }
    return
  }

  if (insecureEmail.value !== validEmail) {
    insecureResponse.value = {
      isError: true,
      message: "❌ Cet email n'est pas enregistré.",
      detail: 'Veuillez vérifier votre adresse ou créer un nouveau compte.',
    }
  } else if (insecurePassword.value !== validPassword) {
    insecureResponse.value = {
      isError: true,
      message: '❌ Mot de passe incorrect.',
      detail: 'Veuillez réessayer ou utiliser la récupération de mot de passe.',
    }
  } else {
    insecureResponse.value = {
      isError: false,
      message: '✓ Connexion réussie !',
      detail: 'Bienvenue user@example.com',
    }
  }
}

const checkSecureLogin = () => {
  if (!secureEmail.value || !securePassword.value) {
    secureResponse.value = {
      isError: true,
      message: 'Veuillez remplir tous les champs.',
      detail: null,
    }
    return
  }

  // Always show the same message for any error
  if (
    secureEmail.value !== validEmail ||
    securePassword.value !== validPassword
  ) {
    secureResponse.value = {
      isError: true,
      message:
        '✓ Identifiants incorrects. Veuillez vérifier votre adresse et mot de passe.',
      detail:
        'Si vous avez oublié vos identifiants, utilisez la récupération de mot de passe.',
    }
  } else {
    secureResponse.value = {
      isError: false,
      message: '✓ Connexion réussie !',
      detail: 'Bienvenue user@example.com',
    }
  }
}

const checklist = ref([
  {
    text: "Auditer tous les messages d'erreur d'authentification",
    checked: false,
  },
  {
    text: "Identifier les messages qui révèlent l'existence de comptes",
    checked: false,
  },
  {
    text: 'Remplacer par des messages génériques',
    checked: false,
  },
  {
    text: 'Appliquer à la création de compte',
    checked: false,
  },
  {
    text: 'Appliquer à la connexion',
    checked: false,
  },
  {
    text: 'Appliquer à la récupération de mot de passe',
    checked: false,
  },
  {
    text: 'Ajouter un délai de réponse identique',
    checked: false,
  },
  {
    text: 'Implémenter le rate limiting',
    checked: false,
  },
  {
    text: 'Tester avec des comptes valides et invalides',
    checked: false,
  },
  {
    text: 'Documenter la politique de sécurité des messages',
    checked: false,
  },
])
</script>
