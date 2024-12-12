<template>
  <v-container class="d-flex justify-center align-center" style="height: 100vh">
    <v-card v-if="!isSignIn" outlined class="w-33 pa-5">
      <v-row class="mb-4">
        <v-col class="text-center">
          <img
            src="../../assets/img/logo-wisata-app.png"
            alt="Wisata App Logo"
            height="50"
          />
        </v-col>
      </v-row>
      <v-row v-if="alert" class="mb-4">
        <v-alert
          text="Berhasil Login!"
          title="Alert title"
          type="success"
        ></v-alert>
      </v-row>

      <v-row class="text-center mb-4">
        <v-col>
          <h3>Create your account</h3>
        </v-col>
      </v-row>

      <v-row no-gutters>
        <v-col cols="6">
          <v-btn
            block
            :color="status === 'personal' ? 'primary' : undefined"
            :variant="status === 'personal' ? 'flat' : 'text'"
            class="rounded-0 px-4 py-4"
            height="48"
            @click="accountType('personal')"
          >
            <v-icon start size="20">mdi-account</v-icon>
            Personal
          </v-btn>
        </v-col>
        <v-col cols="6">
          <v-btn
            block
            :color="status === 'organization' ? 'primary' : undefined"
            :variant="status === 'organization' ? 'flat' : 'text'"
            class="rounded-0 px-4 py-4"
            height="48"
            @click="accountType('organization')"
          >
            <v-icon start size="20">mdi-domain</v-icon>
            Organization
          </v-btn>
        </v-col>
      </v-row>
      <v-card-text>
        <div v-if="status === 'personal'">
          <p>For every traveler:</p>
          <ul>
            <li>Access to exclusive, member-only deals</li>
            <li>Earn cashbacks and rewards on every booking</li>
          </ul>
        </div>
        <div v-if="status === 'organization'">
          <p>For startups and businesses:</p>
          <ul>
            <li>Manage your company's travel plan in one place</li>
            <li>Access to exclusive, member-only deals</li>
            <li>Earn cashbacks and rewards on every bookings</li>
          </ul>
        </div>

        <v-form class="mt-4" ref="formUpsert">
          <v-text-field
            v-if="status === 'organization'"
            label="Organization name"
            v-model="formData.organization"
            outlined
            required
            :rules="formRules"
          ></v-text-field>
          <v-text-field
            label="Full name"
            v-model="formData.name"
            outlined
            required
            :rules="formRules"
          ></v-text-field>
          <v-text-field
            label="WhatsApp number (optional)"
            v-model="formData.whatsapp"
            outlined
            :rules="formRules"
            required
          ></v-text-field>
          <v-text-field
            label="Email"
            v-model="formData.email"
            outlined
            required
            :rules="formRulesEmail"
          ></v-text-field>
          <v-text-field
            label="Password"
            v-model="formData.password"
            type="password"
            outlined
            :rules="formRules"
            required
          ></v-text-field>

          <div
            class="d-flex align-center justify-space-between bg-grey-lighten-4 pa-md-2 mb-3"
          >
            <span>Tired of password?</span>
            <a href="#">Try passwordless ✨</a>
          </div>

          <v-checkbox
            v-model="formData.acceptTnC"
            :label="'I agree to the Terms and Conditions'"
          ></v-checkbox>

          <v-btn
            :disabled="!formData.acceptTnC"
            block
            color="primary"
            class="mt-4"
            @click="handleSubmit"
            >Sign up</v-btn
          >

          <div v-if="status === 'personal'" class="divider-with-text">
            <div class="line"></div>
            <span>OR CONTINUE WITH</span>
            <div class="line"></div>
          </div>
          <div
            v-if="status === 'personal'"
            class="d-flex align-center justify-space-between"
          >
            <v-btn class="w-50">
              <v-icon start>mdi-google</v-icon>
              Google
            </v-btn>
            <v-btn class="w-50">
              <v-icon start>mdi-apple</v-icon>
              Apple
            </v-btn>
          </div>

          <div class="text-center mt-3">
            Already have an account? <a @click="login" href="#">Sign in</a>
          </div>
        </v-form>
      </v-card-text>
    </v-card>
    <v-card v-else outlined class="w-33">
      <v-row class="mb-4">
        <v-col class="text-center">
          <img
            src="../../assets/img/logo-wisata-app.png"
            alt="Wisata App Logo"
            height="50"
          />
        </v-col>
      </v-row>
      <v-row v-if="alert" class="mb-4">
        <v-alert
          text="Berhasil Login!"
          title="Alert title"
          type="success"
        ></v-alert>
      </v-row>

      <v-row class="text-center mb-4">
        <v-col>
          <h3>Sign in to Wisata App</h3>
        </v-col>
      </v-row>
      <v-card-text>
        <v-form class="mt-4" ref="formUpsert">
          <v-text-field
            label="Username or Email"
            v-model="formData.name"
            outlined
            required
            :rules="formRules"
          ></v-text-field>

          <v-btn block color="primary" class="mt-4" href="/">Continue</v-btn>

          <div class="divider-with-text">
            <div class="line"></div>
            <span>OR CONTINUE WITH</span>
            <div class="line"></div>
          </div>

          <v-row class="mt-5">
            <v-btn class="w-100">
              <v-icon start>mdi-google</v-icon>
              Google
            </v-btn>
          </v-row>
          <v-row class="mt-5">
            <v-btn class="w-100">
              <v-icon start>mdi-apple</v-icon>
              Apple
            </v-btn>
          </v-row>
          <v-row class="mt-5 mb-5">
            <v-btn class="w-100">
              <v-icon start>mdi-whatsapp</v-icon>
              WhatsApp
            </v-btn>
          </v-row>
        </v-form>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script setup>
  import { ref, reactive } from 'vue'

  const router = useRouter()
  const status = ref('personal')
  const showPassword = ref(false)
  const isSignIn = ref(false)
  const alert = ref(false)
  const formData = reactive({
    name: '',
    whatsapp: '',
    email: '',
    password: '',
    acceptTnC: false,
  })
  const formUpsert = ref(null)
  const formRules = ref([(v) => !!v || 'Field Wajib Diisi'])
  const formRulesEmail = ref([
    (v) => !!v || 'Email tidak boleh kosong',
    (v) => (v && /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(v)) || 'Format email salah',
  ])

  function accountType(params) {
    console.log('🚀 ~ accountType ~ param:', params)
    status.value = params
  }

  function login() {
    isSignIn.value = true
  }

  async function handleSubmit() {
    const { valid } = await formUpsert.value.validate()
    alert.value = true
    setTimeout(() => {
      alert.value = false
      router.push({
        path: `/`,
      })
    }, 2000)
  }
</script>

<style scoped>
  ul {
    padding-left: 1rem;
  }

  ul li {
    list-style: disc;
  }
  .divider-with-text {
    display: flex;
    align-items: center;
    margin-top: 16px;
    margin-bottom: 16px;
    color: gray;
    font-size: 0.9rem;
    text-transform: uppercase;
  }

  .divider-with-text .line {
    flex-grow: 1;
    height: 1px;
    background-color: #ccc;
    margin: 0 8px;
  }
</style>
