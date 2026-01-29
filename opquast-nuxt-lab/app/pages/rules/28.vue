<template>
  <div class="min-h-screen bg-zinc-950 p-6">
    <div class="max-w-6xl mx-auto">
      <!-- Header -->
      <div class="text-center mb-8">
        <h1 class="text-4xl font-bold text-zinc-100 mb-2">
          Protéger les Données dans l'URL
        </h1>
        <p class="text-zinc-400">
          Ne pas transmettre les données sensibles par la méthode GET
        </p>
      </div>

      <!-- Main Content -->
      <div class="space-y-8">
        <!-- GET vs POST Comparison -->
        <div class="grid md:grid-cols-2 gap-6">
          <!-- GET (Insecure) -->
          <div
            class="bg-zinc-900 rounded-lg shadow-md p-6 border border-red-900"
          >
            <h2 class="text-xl font-bold text-red-400 mb-4 flex items-center">
              <span class="mr-2">❌</span> GET (Non Sécurisé)
            </h2>
            <div class="space-y-4">
              <div class="text-sm">
                <p class="text-zinc-400 mb-2">
                  <strong class="text-zinc-100">URL Résultante :</strong>
                </p>
                <div
                  class="bg-red-950 p-3 rounded border border-red-900 font-mono text-xs break-all"
                >
                  <span class="text-red-300"
                    >login.php?user=john@example.com&password=P@ssw0rd123</span
                  >
                </div>
              </div>

              <div class="text-sm">
                <p class="text-zinc-400 mb-2">
                  <strong class="text-zinc-100">Données visibles dans :</strong>
                </p>
                <ul class="space-y-1 text-red-300 text-xs">
                  <li>📍 Barre d'adresse du navigateur</li>
                  <li>📜 Historique du navigateur</li>
                  <li>🔍 Historique des serveurs (logs)</li>
                  <li>📡 Fournisseur Internet (FAI)</li>
                  <li>🔌 Proxies et points d'accès</li>
                  <li>📊 Services tiers (analytics)</li>
                  <li>💾 Fichiers cache</li>
                </ul>
              </div>

              <div class="text-sm">
                <p class="text-zinc-400 mb-2">
                  <strong class="text-zinc-100">Risques :</strong>
                </p>
                <ul class="space-y-1 text-red-300 text-xs">
                  <li>🔓 Données visibles par-dessus l'épaule</li>
                  <li>💔 Partage accidentel du lien</li>
                  <li>🔍 Recherche et indexation</li>
                  <li>⚠️ Sauvegarde en clair</li>
                </ul>
              </div>
            </div>
          </div>

          <!-- POST (Secure) -->
          <div
            class="bg-zinc-900 rounded-lg shadow-md p-6 border border-green-900"
          >
            <h2 class="text-xl font-bold text-green-400 mb-4 flex items-center">
              <span class="mr-2">✓</span> POST (Sécurisé)
            </h2>
            <div class="space-y-4">
              <div class="text-sm">
                <p class="text-zinc-400 mb-2">
                  <strong class="text-zinc-100">URL Résultante :</strong>
                </p>
                <div
                  class="bg-green-950 p-3 rounded border border-green-900 font-mono text-xs"
                >
                  <span class="text-green-300">🔒 login.php</span>
                </div>
              </div>

              <div class="text-sm">
                <p class="text-zinc-400 mb-2">
                  <strong class="text-zinc-100"
                    >Données transmises dans :</strong
                  >
                </p>
                <ul class="space-y-1 text-green-300 text-xs">
                  <li>📦 Corps de la requête (non visible)</li>
                  <li>🔐 Chiffré en HTTPS</li>
                  <li>✓ Pas dans l'historique</li>
                  <li>✓ Pas dans les logs serveur (généralement)</li>
                  <li>✓ Pas visible en barre d'adresse</li>
                  <li>✓ Pas capturé par-dessus l'épaule</li>
                </ul>
              </div>

              <div class="text-sm">
                <p class="text-zinc-400 mb-2">
                  <strong class="text-zinc-100">Avantages :</strong>
                </p>
                <ul class="space-y-1 text-green-300 text-xs">
                  <li>✓ Données protégées</li>
                  <li>✓ Non indexé</li>
                  <li>✓ URL propre et facile à partager</li>
                  <li>✓ Pas de limite de taille</li>
                </ul>
              </div>
            </div>
          </div>
        </div>

        <!-- Real-World Attack Scenarios -->
        <div
          class="bg-zinc-900 rounded-lg shadow-md p-6 border border-zinc-800"
        >
          <h2 class="text-xl font-bold text-zinc-100 mb-4">
            Scénarios d'Attaque Réels
          </h2>
          <p class="text-zinc-400 mb-6">
            Voici comment les données sensibles en URL peuvent être compromises
            :
          </p>

          <div class="space-y-4">
            <div
              v-for="scenario in attackScenarios"
              :key="scenario.id"
              class="bg-zinc-800 p-4 rounded border border-red-900"
            >
              <h3 class="text-sm font-bold text-red-400 mb-2">
                {{ scenario.title }}
              </h3>
              <p class="text-sm text-zinc-400 mb-3">
                {{ scenario.description }}
              </p>
              <div
                class="bg-zinc-900 p-3 rounded border border-zinc-700 text-xs"
              >
                <p class="text-red-300 font-mono mb-1">
                  <strong>Exemple :</strong>
                </p>
                <p class="text-zinc-300 font-mono break-all">
                  {{ scenario.example }}
                </p>
              </div>
            </div>
          </div>
        </div>

        <!-- Data Transmission Methods -->
        <div
          class="bg-zinc-900 rounded-lg shadow-md p-6 border border-zinc-800"
        >
          <h2 class="text-xl font-bold text-zinc-100 mb-4">
            Méthodes de Transmission
          </h2>

          <div class="space-y-4">
            <div class="bg-zinc-800 p-4 rounded border border-zinc-700">
              <h3 class="text-sm font-bold text-zinc-100 mb-3">
                1. GET (À Éviter pour les Données Sensibles)
              </h3>
              <div class="grid md:grid-cols-2 gap-4 text-xs">
                <div>
                  <p class="text-zinc-400 font-semibold mb-2">
                    Caractéristiques :
                  </p>
                  <ul class="space-y-1 text-zinc-300">
                    <li>• Données dans l'URL</li>
                    <li>• Visible en clair</li>
                    <li>• Limité en taille</li>
                    <li>• Sauvegardé en historique</li>
                  </ul>
                </div>
                <div>
                  <p class="text-zinc-400 font-semibold mb-2">Cas d'usage :</p>
                  <ul class="space-y-1 text-zinc-300">
                    <li>• Recherche/filtrage</li>
                    <li>• Pagination</li>
                    <li>• URLs partageables</li>
                    <li>• Paramètres publics</li>
                  </ul>
                </div>
              </div>
            </div>

            <div class="bg-zinc-800 p-4 rounded border border-green-900">
              <h3 class="text-sm font-bold text-green-400 mb-3">
                2. POST (Recommandé pour les Données Sensibles)
              </h3>
              <div class="grid md:grid-cols-2 gap-4 text-xs">
                <div>
                  <p class="text-zinc-400 font-semibold mb-2">
                    Caractéristiques :
                  </p>
                  <ul class="space-y-1 text-zinc-300">
                    <li>• Données en corps de requête</li>
                    <li>• Cachée de l'URL</li>
                    <li>• Sans limite de taille</li>
                    <li>• Non en historique</li>
                  </ul>
                </div>
                <div>
                  <p class="text-zinc-400 font-semibold mb-2">Cas d'usage :</p>
                  <ul class="space-y-1 text-zinc-300">
                    <li>• Identifiants/mots de passe</li>
                    <li>• Données bancaires</li>
                    <li>• Informations personnelles</li>
                    <li>• Données de santé</li>
                  </ul>
                </div>
              </div>
            </div>

            <div class="bg-zinc-800 p-4 rounded border border-purple-900">
              <h3 class="text-sm font-bold text-purple-400 mb-3">
                3. COOKIES (Pour les Identifiants de Session)
              </h3>
              <div class="grid md:grid-cols-2 gap-4 text-xs">
                <div>
                  <p class="text-zinc-400 font-semibold mb-2">
                    Caractéristiques :
                  </p>
                  <ul class="space-y-1 text-zinc-300">
                    <li>• Stocké côté client</li>
                    <li>• Envoyé automatiquement</li>
                    <li>• Pas en URL</li>
                    <li>• HttpOnly si possible</li>
                  </ul>
                </div>
                <div>
                  <p class="text-zinc-400 font-semibold mb-2">Cas d'usage :</p>
                  <ul class="space-y-1 text-zinc-300">
                    <li>• Identifiants de session</li>
                    <li>• Jetons (tokens)</li>
                    <li>• Préférences utilisateur</li>
                    <li>• Données persistantes</li>
                  </ul>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Session ID Exposure -->
        <div class="bg-zinc-900 rounded-lg shadow-md p-6 border border-red-900">
          <h2 class="text-xl font-bold text-red-400 mb-4">
            Session ID dans l'URL (Danger Critique)
          </h2>
          <p class="text-zinc-400 mb-6">
            Transmettre l'identifiant de session dans l'URL est particulièrement
            dangereux car quiconque connaît l'ID peut accéder au compte :
          </p>

          <div class="grid md:grid-cols-2 gap-6">
            <!-- Vulnerable Example -->
            <div class="bg-red-950 p-4 rounded border border-red-900">
              <h3 class="text-sm font-bold text-red-300 mb-3">
                ❌ Exemple Vulnérable
              </h3>
              <div class="space-y-3">
                <div
                  class="bg-zinc-950 p-3 rounded border border-red-900 text-xs font-mono break-all"
                >
                  <p class="text-red-400">
                    page.php?SESSIONID=abc123def456ghi789
                  </p>
                </div>
                <ul class="space-y-2 text-red-300 text-sm">
                  <li class="flex items-start">
                    <span class="mr-2">⚠️</span>
                    <span>L'ID est visible en barre d'adresse</span>
                  </li>
                  <li class="flex items-start">
                    <span class="mr-2">⚠️</span>
                    <span>Vu en historique du navigateur</span>
                  </li>
                  <li class="flex items-start">
                    <span class="mr-2">⚠️</span>
                    <span>
                      Visible par-dessus l'épaule (shoulder surfing)
                    </span>
                  </li>
                  <li class="flex items-start">
                    <span class="mr-2">⚠️</span>
                    <span
                      >Lien partagé par l'utilisateur = accès au compte</span
                    >
                  </li>
                  <li class="flex items-start">
                    <span class="mr-2">⚠️</span>
                    <span>Capturé par les proxies/FAI</span>
                  </li>
                </ul>
              </div>
            </div>

            <!-- Secure Example -->
            <div class="bg-green-950 p-4 rounded border border-green-900">
              <h3 class="text-sm font-bold text-green-300 mb-3">
                ✓ Approche Sécurisée
              </h3>
              <div class="space-y-3">
                <div
                  class="bg-zinc-950 p-3 rounded border border-green-900 text-xs font-mono break-all"
                >
                  <p class="text-green-400">🔒 page.php</p>
                </div>
                <ul class="space-y-2 text-green-300 text-sm">
                  <li class="flex items-start">
                    <span class="mr-2">✓</span>
                    <span>Session ID en cookie HttpOnly</span>
                  </li>
                  <li class="flex items-start">
                    <span class="mr-2">✓</span>
                    <span>Non visible en barre d'adresse</span>
                  </li>
                  <li class="flex items-start">
                    <span class="mr-2">✓</span>
                    <span>Non en historique</span>
                  </li>
                  <li class="flex items-start">
                    <span class="mr-2">✓</span>
                    <span>Impossible à voir par-dessus l'épaule</span>
                  </li>
                  <li class="flex items-start">
                    <span class="mr-2">✓</span>
                    <span>URL peut être partagée sans risque</span>
                  </li>
                </ul>
              </div>
            </div>
          </div>
        </div>

        <!-- Common Insecure Patterns -->
        <div
          class="bg-zinc-900 rounded-lg shadow-md p-6 border border-zinc-800"
        >
          <h2 class="text-xl font-bold text-zinc-100 mb-4">
            Patterns Non Sécurisés à Éviter
          </h2>

          <div class="space-y-3">
            <div
              v-for="pattern in insecurePatterns"
              :key="pattern.id"
              class="bg-red-950 p-4 rounded border border-red-900"
            >
              <p class="text-sm font-mono text-red-300 mb-2 break-all">
                {{ pattern.example }}
              </p>
              <p class="text-xs text-red-200">
                <strong>Problème :</strong> {{ pattern.problem }}
              </p>
            </div>
          </div>
        </div>

        <!-- HTTP Body (POST) Example -->
        <div
          class="bg-zinc-900 rounded-lg shadow-md p-6 border border-green-900"
        >
          <h2 class="text-xl font-bold text-green-400 mb-4">
            Exemple : Données POST (Sécurisées)
          </h2>
          <p class="text-zinc-400 mb-4">
            Avec POST, les données sont envoyées dans le corps de la requête (en
            HTTPS) :
          </p>

          <div class="space-y-4">
            <div class="bg-zinc-800 p-4 rounded border border-green-900">
              <p class="text-xs font-semibold text-green-400 mb-2">
                URL Visible au Navigateur :
              </p>
              <div
                class="bg-zinc-950 p-3 rounded border border-green-900 font-mono text-sm"
              >
                <span class="text-green-300"
                  >🔒 https://secure.example.com/login.php</span
                >
              </div>
            </div>

            <div class="text-center text-zinc-500">↓ HTTPS Chiffré ↓</div>

            <div class="bg-zinc-800 p-4 rounded border border-green-900">
              <p class="text-xs font-semibold text-green-400 mb-2">
                Corps de la Requête (Non Visible) :
              </p>
              <div
                class="bg-zinc-950 p-3 rounded border border-green-900 font-mono text-xs space-y-1"
              >
                <p>
                  <span class="text-green-400">POST</span>
                  <span class="text-zinc-400">/login.php HTTP/1.1</span>
                </p>
                <p>
                  <span class="text-green-400">Host:</span>
                  <span class="text-zinc-400">secure.example.com</span>
                </p>
                <p>
                  <span class="text-green-400">Content-Type:</span>
                  <span class="text-zinc-400"
                    >application/x-www-form-urlencoded</span
                  >
                </p>
                <p class="text-zinc-400">...</p>
                <p class="text-green-400">
                  [Données chiffrées : user, password, etc.]
                </p>
              </div>
            </div>
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
                Utiliser <strong class="text-zinc-100">POST</strong> pour tous
                les formulaires sensibles
              </span>
            </li>
            <li class="flex items-start">
              <span class="text-green-400 mr-3">✓</span>
              <span class="text-zinc-300">
                <strong class="text-zinc-100">Jamais GET</strong> pour
                identifiants, mots de passe, données bancaires
              </span>
            </li>
            <li class="flex items-start">
              <span class="text-green-400 mr-3">✓</span>
              <span class="text-zinc-300">
                Stocker les identifiants de session en
                <strong class="text-zinc-100">cookies HttpOnly</strong>, jamais
                en URL
              </span>
            </li>
            <li class="flex items-start">
              <span class="text-green-400 mr-3">✓</span>
              <span class="text-zinc-300">
                Toujours utiliser
                <strong class="text-zinc-100">HTTPS</strong> pour transmettre
                des données sensibles
              </span>
            </li>
            <li class="flex items-start">
              <span class="text-green-400 mr-3">✓</span>
              <span class="text-zinc-300">
                Tester avec les cookies
                <strong class="text-zinc-100">activés ET désactivés</strong>
              </span>
            </li>
            <li class="flex items-start">
              <span class="text-green-400 mr-3">✓</span>
              <span class="text-zinc-300">
                Ne jamais utiliser le format
                <strong class="text-zinc-100"
                  >http://user:pass@example.com/</strong
                >
              </span>
            </li>
            <li class="flex items-start">
              <span class="text-green-400 mr-3">✓</span>
              <span class="text-zinc-300">
                Auditer régulièrement les formulaires pour vérifier l'absence de
                données sensibles en URL
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

        <!-- Testing Procedures -->
        <div
          class="bg-zinc-900 rounded-lg shadow-md p-6 border border-zinc-800"
        >
          <h2 class="text-xl font-bold text-zinc-100 mb-4">
            Procédures de Vérification
          </h2>
          <div class="space-y-4">
            <div class="bg-zinc-800 p-4 rounded border border-zinc-700">
              <h3 class="text-sm font-bold text-zinc-100 mb-2">
                1. Inspecter les URLs
              </h3>
              <p class="text-sm text-zinc-400">
                Vérifiez la barre d'adresse lors de la connexion et de la saisie
                de données sensibles. Aucun paramètre avec identifiants ne doit
                être visible.
              </p>
            </div>

            <div class="bg-zinc-800 p-4 rounded border border-zinc-700">
              <h3 class="text-sm font-bold text-zinc-100 mb-2">
                2. Utiliser les Outils de Développement
              </h3>
              <p class="text-sm text-zinc-400">
                Ouvrez F12 → Network → envoyez le formulaire et vérifiez que
                POST est utilisé et les données ne sont pas en URL.
              </p>
            </div>

            <div class="bg-zinc-800 p-4 rounded border border-zinc-700">
              <h3 class="text-sm font-bold text-zinc-100 mb-2">
                3. Tester l'Historique du Navigateur
              </h3>
              <p class="text-sm text-zinc-400">
                Ouvrez l'historique (Ctrl+H) et vérifiez qu'aucune donnée
                sensible n'y apparaît.
              </p>
            </div>

            <div class="bg-zinc-800 p-4 rounded border border-zinc-700">
              <h3 class="text-sm font-bold text-zinc-100 mb-2">
                4. Tester les Cookies
              </h3>
              <p class="text-sm text-zinc-400">
                Désactivez les cookies dans les paramètres du navigateur et
                testez à nouveau. L'application ne doit pas basculer vers GET
                pour la session.
              </p>
            </div>

            <div class="bg-zinc-800 p-4 rounded border border-zinc-700">
              <h3 class="text-sm font-bold text-zinc-100 mb-2">
                5. Vérifier le Code Source du Formulaire
              </h3>
              <p class="text-sm text-zinc-400">
                Inspectez la balise form et vérifiez que method="POST" est
                utilisée et que l'action pointe vers HTTPS.
              </p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const attackScenarios = [
  {
    id: 1,
    title: 'Lien Partagé Accidentellement',
    description:
      "L'utilisateur partage l'URL contenant son identifiant de session sur les réseaux sociaux ou par email.",
    example: 'https://example.com/account?SESSIONID=abc123 → Accès au compte',
  },
  {
    id: 2,
    title: "Lecture par-dessus l'Épaule",
    description:
      "Une personne voit l'écran de l'utilisateur et lit l'URL contenant les identifiants.",
    example:
      "Voir page.php?user=john@example.com&password=P@ssw0rd dans la barre d'adresse",
  },
  {
    id: 3,
    title: 'Historique du Navigateur',
    description:
      "Un autre utilisateur du même ordinateur accède à l'historique et récupère les identifiants.",
    example:
      'Historique → page.php?user=john&password=password123 → Accès au compte',
  },
  {
    id: 4,
    title: 'Logs des Proxies/FAI',
    description:
      "Les FAI et proxies enregistrent toutes les requêtes, y compris les données en clair de l'URL.",
    example: 'Logs FAI : GET /login.php?user=xxx&password=yyy [IP] [timestamp]',
  },
  {
    id: 5,
    title: 'Services Tiers',
    description:
      "Les services d'analytics ou pixels de tracking reçoivent les données sensibles en URL.",
    example: 'Google Analytics reçoit : login.php?user=john&password=123',
  },
  {
    id: 6,
    title: 'Indexation par les Moteurs',
    description:
      'Les moteurs de recherche indexent les données sensibles visibles en URL.',
    example:
      "Google : 'john@example.com' dans les résultats de recherche en trouvant le lien en clair",
  },
]

const insecurePatterns = [
  {
    id: 1,
    example: 'login.php?user=john@example.com&password=P@ssw0rd',
    problem: 'Identifiants directement en URL',
  },
  {
    id: 2,
    example: 'page.php?credit_card=4111-1111-1111-1111&cvv=123',
    problem: 'Données bancaires en clair',
  },
  {
    id: 3,
    example: 'http://john:password@example.com/account',
    problem: 'Format http://user:pass@domaine (très dangereux)',
  },
  {
    id: 4,
    example: 'ftp://user:password@ftp.example.com/files',
    problem: 'Identifiants FTP en URL (similaire)',
  },
  {
    id: 5,
    example: 'page.php?SESSIONID=abc123def456&token=xyz789',
    problem: 'Identifiants de session en URL',
  },
  {
    id: 6,
    example: 'checkout.php?ssn=123-45-6789&dob=01/01/1990',
    problem: 'Données personnelles et sensibles en URL',
  },
]

const checklist = ref([
  {
    text: 'Utiliser POST pour tous les formulaires sensibles',
    checked: false,
  },
  {
    text: 'Ne jamais utiliser GET pour identifiants/mots de passe',
    checked: false,
  },
  {
    text: 'Utiliser des cookies HttpOnly pour les sessions',
    checked: false,
  },
  {
    text: 'Ne pas inclure les tokens/sessions en URL',
    checked: false,
  },
  {
    text: "Vérifier qu'aucune donnée sensible n'apparaît en barre d'adresse",
    checked: false,
  },
  {
    text: "Vérifier que l'historique ne contient pas de données sensibles",
    checked: false,
  },
  {
    text: 'Tester avec cookies activés et désactivés',
    checked: false,
  },
  {
    text: 'Utiliser HTTPS pour tout (pas seulement les données sensibles)',
    checked: false,
  },
  {
    text: 'Auditer les formulaires régulièrement',
    checked: false,
  },
  {
    text: 'Former les développeurs sur la sécurité des URL',
    checked: false,
  },
  {
    text: 'Mettre en place des tests de sécurité automatisés',
    checked: false,
  },
])
</script>
