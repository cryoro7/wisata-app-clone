<template>
  <v-footer class="bg-grey-lighten-4">
    <v-container>
      <!-- Social Media and App Downloads -->
      <v-row class="py-4 border-bottom">
        <v-col cols="12" md="6" class="d-flex align-center">
          <span class="text-body-1 me-4">Follow Us</span>
          <v-btn
            v-for="(social, index) in socials"
            :key="index"
            :href="social.link"
            variant="text"
            icon
            color="black"
          >
            <v-icon>{{ social.icon }}</v-icon>
          </v-btn>
        </v-col>
        <v-col cols="12" md="6" class="d-flex justify-end align-center">
          <span class="text-body-1 me-4">Download App</span>
          <v-btn href="https://play.google.com" variant="text" class="me-2">
            <img
              src="/assets/img/android.png"
              alt="Get it on Google Play"
              height="40"
              width="135"
            />
          </v-btn>
          <v-btn href="https://apps.apple.com" variant="text">
            <img
              src="/assets/img/ios.png"
              alt="Download on the App Store"
              height="40"
              width="135"
            />
          </v-btn>
        </v-col>
      </v-row>

      <!-- Main Footer Content -->
      <v-row class="py-8">
        <!-- Licenses and Membership -->
        <v-col cols="12" md="3">
          <h3 class="text-h6 mb-4">Licenses and membership</h3>
          <div class="d-flex gap-4">
            <img src="/assets/img/payment.png" alt="IATA" class="w-100" />
          </div>
        </v-col>

        <!-- Payment Partners -->
        <v-col cols="12" md="3">
          <h3 class="text-h6 mb-4">Payment Partners</h3>
          <v-row dense>
            <v-col cols="12" class="mb-2">
              <img src="/assets/img/payment.png" alt="IATA" class="w-100" />
            </v-col>
          </v-row>
        </v-col>

        <!-- Products -->
        <v-col cols="12" md="3">
          <h3 class="text-h6 mb-4">Products</h3>
          <v-list density="compact" nav class="pa-0 bg-transparent">
            <v-list-item
              v-for="(product, index) in products"
              :key="index"
              :title="product.name"
              :href="product.link"
              class="px-0"
              density="compact"
            />
          </v-list>
        </v-col>

        <!-- Links -->
        <v-col cols="12" md="3">
          <h3 class="text-h6 mb-4">Links</h3>
          <v-list density="compact" nav class="pa-0 bg-transparent">
            <v-list-item
              v-for="(link, index) in links"
              :key="index"
              :title="link.name"
              :href="link.url"
              class="px-0"
              density="compact"
            />
          </v-list>
        </v-col>
      </v-row>

      <!-- Copyright -->
      <v-row class="pt-4 border-top">
        <v-col cols="12" class="d-flex justify-space-between align-center">
          <div class="d-flex align-center">
            <span class="text-body-2">&copy; Wisata App</span>
            <v-btn
              variant="text"
              density="compact"
              class="text-none ms-2"
              href="/terms"
            >
              Terms
            </v-btn>
          </div>
          <span class="text-body-2">v4.10</span>
        </v-col>
      </v-row>
    </v-container>
  </v-footer>
</template>

<script setup>
  const socials = [
    { icon: 'mdi-tiktok', link: 'https://tiktok.com' },
    { icon: 'mdi-instagram', link: 'https://instagram.com' },
    { icon: 'mdi-twitter', link: 'https://twitter.com' },
    { icon: 'mdi-youtube', link: 'https://youtube.com' },
    { icon: 'mdi-linkedin', link: 'https://linkedin.com' },
  ]

  const payments = [
    { name: 'Visa', logo: '/placeholder.svg?height=30&width=60' },
    { name: 'Mastercard', logo: '/placeholder.svg?height=30&width=60' },
    { name: 'JCB', logo: '/placeholder.svg?height=30&width=60' },
    { name: 'QRIS', logo: '/placeholder.svg?height=30&width=60' },
    { name: 'BCA', logo: '/placeholder.svg?height=30&width=60' },
    { name: 'CIMB', logo: '/placeholder.svg?height=30&width=60' },
    { name: 'Kredivo', logo: '/placeholder.svg?height=30&width=60' },
    { name: 'OVO', logo: '/placeholder.svg?height=30&width=60' },
    { name: 'Indosat', logo: '/placeholder.svg?height=30&width=60' },
  ]

  const products = [
    { name: 'Hotels', link: '/hotels' },
    { name: 'Dummy Hotel Voucher', link: '/hotel-voucher' },
    { name: 'Dummy Flight Ticket', link: '/flight-ticket' },
    { name: 'Wisata Diary', link: '/diary' },
  ]
</script>
<style scoped></style>
