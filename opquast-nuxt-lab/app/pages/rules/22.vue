<template>
  <div class="min-h-screen bg-zinc-950 p-6">
    <div class="max-w-6xl mx-auto">
      <!-- Header -->
      <div class="text-center mb-8">
        <h1 class="text-4xl font-bold text-zinc-100 mb-2">
          Système d'Authentification Unifiée
        </h1>
        <p class="text-zinc-400">
          Un identifiant unique pour tous vos services
        </p>
      </div>

      <!-- Login/Registration Card -->
      <div
        v-if="!isAuthenticated"
        class="max-w-md mx-auto bg-zinc-900 rounded-lg shadow-xl p-8 border border-zinc-800"
      >
        <div class="mb-6">
          <div class="flex border-b border-zinc-800 border-zinc-800">
            <button
              @click="authMode = 'login'"
              :class="[
                'flex-1 py-3 font-semibold transition-colors',
                authMode === 'login'
                  ? 'text-zinc-100 border-b border-zinc-800-2 border-zinc-100'
                  : 'text-zinc-400 hover:text-zinc-200',
              ]"
            >
              Connexion
            </button>
            <button
              @click="authMode = 'register'"
              :class="[
                'flex-1 py-3 font-semibold transition-colors',
                authMode === 'register'
                  ? 'text-zinc-100 border-b border-zinc-800-2 border-zinc-100'
                  : 'text-zinc-400 hover:text-zinc-200',
              ]"
            >
              Inscription
            </button>
          </div>
        </div>

        <form @submit.prevent="handleAuth" class="space-y-4">
          <div>
            <label class="block text-sm font-medium text-zinc-200 mb-1">
              Identifiant unique
            </label>
            <input
              v-model="credentials.username"
              type="text"
              required
              class="w-full px-4 py-2 border border-zinc-700 bg-zinc-800 rounded-lg text-zinc-100 focus:ring-2 focus:ring-zinc-500 focus:border-transparent"
              placeholder="votre.email@exemple.com"
            />
          </div>

          <div>
            <label class="block text-sm font-medium text-zinc-200 mb-1">
              Mot de passe
            </label>
            <input
              v-model="credentials.password"
              type="password"
              required
              class="w-full px-4 py-2 border border-zinc-700 bg-zinc-800 rounded-lg text-zinc-100 focus:ring-2 focus:ring-zinc-500 focus:border-transparent"
              placeholder="••••••••"
            />
          </div>

          <div v-if="authMode === 'register'">
            <label class="block text-sm font-medium text-zinc-200 mb-1">
              Confirmer le mot de passe
            </label>
            <input
              v-model="credentials.confirmPassword"
              type="password"
              required
              class="w-full px-4 py-2 border border-zinc-700 bg-zinc-800 rounded-lg text-zinc-100 focus:ring-2 focus:ring-zinc-500 focus:border-transparent"
              placeholder="••••••••"
            />
          </div>

          <!-- 2FA Setup (only during registration) -->
          <div
            v-if="authMode === 'register'"
            class="bg-zinc-800 p-4 rounded-lg"
          >
            <label class="flex items-center space-x-2">
              <input
                v-model="credentials.enable2FA"
                type="checkbox"
                class="w-4 h-4 text-zinc-100 rounded focus:ring-zinc-500"
              />
              <span class="text-sm font-medium text-zinc-200">
                Activer l'authentification à deux facteurs (2FA)
              </span>
            </label>
            <p class="text-xs text-zinc-400 mt-2">
              Recommandé pour une sécurité renforcée
            </p>
          </div>

          <!-- 2FA Code Input (during login if enabled) -->
          <div
            v-if="authMode === 'login' && show2FAInput"
            class="bg-zinc-800 p-4 rounded-lg"
          >
            <label class="block text-sm font-medium text-zinc-200 mb-1">
              Code de vérification (2FA)
            </label>
            <input
              v-model="credentials.twoFactorCode"
              type="text"
              required
              maxlength="6"
              class="w-full px-4 py-2 border border-zinc-700 bg-zinc-800 rounded-lg text-zinc-100 focus:ring-2 focus:ring-zinc-500 focus:border-transparent"
              placeholder="123456"
            />
            <p class="text-xs text-zinc-400 mt-2">
              Entrez le code à 6 chiffres de votre application
              d'authentification
            </p>
          </div>

          <button
            type="submit"
            class="w-full bg-zinc-700 text-zinc-100 py-3 rounded-lg font-semibold hover:bg-zinc-600 transition-colors"
          >
            {{ authMode === 'login' ? 'Se connecter' : "S'inscrire" }}
          </button>
        </form>

        <!-- Error Message -->
        <div
          v-if="errorMessage"
          class="mt-4 p-3 bg-zinc-800 border border-red-900 rounded-lg"
        >
          <p class="text-sm text-red-400">{{ errorMessage }}</p>
        </div>

        <!-- Success Message -->
        <div
          v-if="successMessage"
          class="mt-4 p-3 bg-zinc-800 border border-green-900 rounded-lg"
        >
          <p class="text-sm text-green-400">{{ successMessage }}</p>
        </div>

        <!-- Trust Message -->
        <div class="mt-6 p-4 bg-zinc-800 border border-zinc-700 rounded-lg">
          <p class="text-xs text-zinc-200 font-medium mb-2">
            Vos données sont protégées
          </p>
          <p class="text-xs text-zinc-400">
            Vos identifiants sont utilisés de manière sécurisée et partagés
            uniquement entre nos services autorisés.
          </p>
        </div>
      </div>

      <!-- Dashboard (after authentication) -->
      <div v-else>
        <!-- User Info Bar -->
        <div class="bg-zinc-900 rounded-lg shadow-md p-4 mb-6">
          <div class="flex items-center justify-between">
            <div class="flex items-center space-x-4">
              <div
                class="w-12 h-12 bg-zinc-700 rounded-full flex items-center justify-center text-zinc-100 font-bold text-xl"
              >
                {{ userInitial }}
              </div>
              <div>
                <p class="font-semibold text-zinc-100">
                  {{ currentUser.username }}
                </p>
                <p class="text-sm text-zinc-400">
                  Authentification
                  {{
                    currentUser.has2FA ? 'renforcée (2FA activée)' : 'standard'
                  }}
                </p>
              </div>
            </div>
            <button
              @click="logout"
              class="px-4 py-2 bg-red-900 text-zinc-100 rounded-lg hover:bg-red-800 transition-colors"
            >
              Se déconnecter
            </button>
          </div>
        </div>

        <!-- Services Grid -->
        <div class="grid md:grid-cols-3 gap-6">
          <div
            v-for="service in services"
            :key="service.id"
            class="bg-zinc-900 rounded-lg shadow-md hover:shadow-xl transition-shadow cursor-pointer"
            @click="accessService(service)"
          >
            <div class="p-6">
              <div class="flex items-center justify-between mb-4">
                <div
                  class="w-12 h-12 bg-zinc-700 rounded-lg flex items-center justify-center text-zinc-100 font-bold text-xl"
                >
                  {{ service.icon }}
                </div>
                <div
                  :class="[
                    'px-3 py-1 rounded-full text-xs font-semibold',
                    service.accessed
                      ? 'bg-green-900 text-green-300'
                      : 'bg-zinc-800 text-zinc-400',
                  ]"
                >
                  {{ service.accessed ? 'Accédé' : 'Non accédé' }}
                </div>
              </div>

              <h3 class="text-xl font-bold text-zinc-100 mb-2">
                {{ service.name }}
              </h3>
              <p class="text-sm text-zinc-400 mb-4">
                {{ service.description }}
              </p>

              <div class="flex items-center text-xs text-zinc-400 mb-3">
                <svg
                  class="w-4 h-4 mr-1"
                  fill="currentColor"
                  viewBox="0 0 20 20"
                >
                  <path
                    fill-rule="evenodd"
                    d="M5 9V7a5 5 0 0110 0v2a2 2 0 012 2v5a2 2 0 01-2 2H5a2 2 0 01-2-2v-5a2 2 0 012-2zm8-2v2H7V7a3 3 0 016 0z"
                    clip-rule="evenodd"
                  />
                </svg>
                Authentification unifiée
              </div>

              <button
                class="w-full bg-zinc-700 text-zinc-100 py-2 rounded-lg hover:bg-zinc-600 transition-colors font-semibold"
              >
                Accéder au service
              </button>
            </div>

            <div
              v-if="service.accessed"
              class="bg-zinc-800 px-6 py-3 border-t border-green-100"
            >
              <p class="text-xs text-green-300">
                Dernière connexion: {{ service.lastAccessed }}
              </p>
            </div>
          </div>
        </div>

        <!-- Benefits Section -->
        <div class="mt-8 bg-zinc-900 rounded-lg shadow-md p-6">
          <h2 class="text-2xl font-bold text-zinc-100 mb-4">
            Avantages de l'authentification unifiée
          </h2>
          <div class="grid md:grid-cols-3 gap-6">
            <div class="flex items-start space-x-3">
              <div
                class="w-10 h-10 bg-zinc-800 rounded-lg flex items-center justify-center text-zinc-100 font-bold"
              >
                1
              </div>
              <div>
                <h3 class="font-semibold text-zinc-100 mb-1">
                  Un seul identifiant
                </h3>
                <p class="text-sm text-zinc-400">
                  Mémorisez un seul jeu d'identifiants pour tous vos services
                </p>
              </div>
            </div>
            <div class="flex items-start space-x-3">
              <div
                class="w-10 h-10 bg-zinc-800 rounded-lg flex items-center justify-center text-zinc-100 font-bold"
              >
                2
              </div>
              <div>
                <h3 class="font-semibold text-zinc-100 mb-1">
                  Connexion rapide
                </h3>
                <p class="text-sm text-zinc-400">
                  Accédez instantanément à tous vos services sans ressaisir vos
                  identifiants
                </p>
              </div>
            </div>
            <div class="flex items-start space-x-3">
              <div
                class="w-10 h-10 bg-zinc-800 rounded-lg flex items-center justify-center text-zinc-100 font-bold"
              >
                3
              </div>
              <div>
                <h3 class="font-semibold text-zinc-100 mb-1">
                  Sécurité renforcée
                </h3>
                <p class="text-sm text-zinc-400">
                  Protection 2FA partagée sur tous vos services pour une
                  sécurité maximale
                </p>
              </div>
            </div>
          </div>
        </div>

        <!-- Activity Log -->
        <div class="mt-6 bg-zinc-900 rounded-lg shadow-md p-6">
          <h2 class="text-xl font-bold text-zinc-100 mb-4">
            Historique d'activité
          </h2>
          <div class="space-y-2">
            <div
              v-for="(log, index) in activityLog"
              :key="index"
              class="flex items-center justify-between p-3 bg-gray-50 rounded-lg"
            >
              <div class="flex items-center space-x-3">
                <div
                  class="w-8 h-8 bg-zinc-700 rounded flex items-center justify-center text-zinc-100 font-semibold text-sm"
                >
                  {{ log.icon }}
                </div>
                <div>
                  <p class="text-sm font-medium text-zinc-100">
                    {{ log.action }}
                  </p>
                  <p class="text-xs text-zinc-400">{{ log.timestamp }}</p>
                </div>
              </div>
              <div class="text-xs text-zinc-400">{{ log.service }}</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UnifiedAuthSystem',
  data() {
    return {
      isAuthenticated: false,
      authMode: 'login',
      show2FAInput: false,
      credentials: {
        username: '',
        password: '',
        confirmPassword: '',
        enable2FA: false,
        twoFactorCode: '',
      },
      currentUser: null,
      errorMessage: '',
      successMessage: '',
      services: [
        {
          id: 1,
          name: 'Messagerie',
          description: 'Accédez à votre boîte mail professionnelle',
          icon: 'M',
          accessed: false,
          lastAccessed: null,
        },
        {
          id: 2,
          name: 'Gestion de documents',
          description: 'Stockez et partagez vos fichiers en toute sécurité',
          icon: 'D',
          accessed: false,
          lastAccessed: null,
        },
        {
          id: 3,
          name: 'Calendrier',
          description: 'Organisez vos rendez-vous et réunions',
          icon: 'C',
          accessed: false,
          lastAccessed: null,
        },
        {
          id: 4,
          name: 'Gestion de projets',
          description: 'Collaborez efficacement avec votre équipe',
          icon: 'P',
          accessed: false,
          lastAccessed: null,
        },
        {
          id: 5,
          name: 'Ressources Humaines',
          description: 'Gérez vos congés et documents RH',
          icon: 'RH',
          accessed: false,
          lastAccessed: null,
        },
        {
          id: 6,
          name: 'Support Client',
          description: 'Accédez au système de tickets et FAQ',
          icon: 'S',
          accessed: false,
          lastAccessed: null,
        },
      ],
      activityLog: [],
      // Simulated user database
      users: [],
    }
  },
  computed: {
    userInitial() {
      return this.currentUser?.username?.charAt(0).toUpperCase() || 'U'
    },
  },
  methods: {
    handleAuth() {
      this.errorMessage = ''
      this.successMessage = ''

      if (this.authMode === 'register') {
        this.register()
      } else {
        this.login()
      }
    },
    register() {
      // Validation
      if (this.credentials.password !== this.credentials.confirmPassword) {
        this.errorMessage = 'Les mots de passe ne correspondent pas'
        return
      }

      if (this.credentials.password.length < 8) {
        this.errorMessage =
          'Le mot de passe doit contenir au moins 8 caractères'
        return
      }

      // Check if user already exists
      const existingUser = this.users.find(
        (u) => u.username === this.credentials.username,
      )
      if (existingUser) {
        this.errorMessage = 'Cet identifiant est déjà utilisé'
        return
      }

      // Create new user
      const newUser = {
        username: this.credentials.username,
        password: this.credentials.password, // In real app, this would be hashed
        has2FA: this.credentials.enable2FA,
        createdAt: new Date().toISOString(),
      }

      this.users.push(newUser)
      this.successMessage =
        'Compte créé avec succès ! Vous pouvez maintenant vous connecter.'

      // Reset form and switch to login
      setTimeout(() => {
        this.authMode = 'login'
        this.credentials.password = ''
        this.credentials.confirmPassword = ''
        this.successMessage = ''
      }, 2000)
    },
    login() {
      // Find user
      const user = this.users.find(
        (u) => u.username === this.credentials.username,
      )

      if (!user) {
        this.errorMessage = 'Identifiant ou mot de passe incorrect'
        return
      }

      if (user.password !== this.credentials.password) {
        this.errorMessage = 'Identifiant ou mot de passe incorrect'
        return
      }

      // Check 2FA
      if (user.has2FA && !this.show2FAInput) {
        this.show2FAInput = true
        this.successMessage = 'Veuillez entrer votre code 2FA'
        return
      }

      if (user.has2FA && this.show2FAInput) {
        // Simulate 2FA validation (in real app, validate against TOTP)
        if (this.credentials.twoFactorCode.length !== 6) {
          this.errorMessage = 'Code 2FA invalide'
          return
        }
      }

      // Successful login
      this.currentUser = { ...user }
      this.isAuthenticated = true
      this.show2FAInput = false
      this.addActivityLog('Connexion réussie', '✓', 'Système')

      // Reset credentials
      this.credentials = {
        username: '',
        password: '',
        confirmPassword: '',
        enable2FA: false,
        twoFactorCode: '',
      }
    },
    logout() {
      this.addActivityLog('Déconnexion', 'X', 'Système')
      this.isAuthenticated = false
      this.currentUser = null
      this.services.forEach((service) => {
        service.accessed = false
        service.lastAccessed = null
      })
      this.activityLog = []
    },
    accessService(service) {
      service.accessed = true
      service.lastAccessed = this.formatDateTime(new Date())
      this.addActivityLog(
        `Accès au service ${service.name}`,
        service.icon,
        service.name,
      )
    },
    addActivityLog(action, icon, service) {
      this.activityLog.unshift({
        action,
        icon,
        service,
        timestamp: this.formatDateTime(new Date()),
      })

      // Keep only last 10 entries
      if (this.activityLog.length > 10) {
        this.activityLog = this.activityLog.slice(0, 10)
      }
    },
    formatDateTime(date) {
      return date.toLocaleString('fr-FR', {
        day: '2-digit',
        month: '2-digit',
        year: 'numeric',
        hour: '2-digit',
        minute: '2-digit',
      })
    },
  },
  mounted() {
    // Create a demo user for testing
    this.users.push({
      username: 'demo@exemple.com',
      password: 'demo1234',
      has2FA: false,
      createdAt: new Date().toISOString(),
    })

    this.users.push({
      username: 'admin@exemple.com',
      password: 'admin1234',
      has2FA: true,
      createdAt: new Date().toISOString(),
    })
  },
}
</script>

<style scoped>
/* Additional custom styles if needed */
</style>
