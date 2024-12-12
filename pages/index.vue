<template>
  <NuxtLayout key="home" name="main">
    <header class="d-print-none bg-white py-4" style="height: 64px">
      <div class="container">
        <v-row justify="between" align="center" align-content="space-between">
          <NuxtLink
            to="/"
            class="d-inline-block align-self-center brand-logo w-50"
            style="line-height: 0"
          >
            <div
              class="d-inline-block"
              style="line-height: 0; margin: 0px 0px 0px -6px"
            >
              <img
                src="/assets/img/logo-wisata-app.png"
                alt="app-logo"
                width="185"
                height="42"
              />
            </div>
          </NuxtLink>
          <div class="w-50 d-flex align-end flex-column">
            <v-btn
              class="text-none text-body-2 font-weight-bold"
              href="/auth"
              variant="flat"
              color="primary"
            >
              Sign in
            </v-btn>
          </div>
        </v-row>
      </div>
    </header>
    <div class="container">
      <div class="py-8 my-0 my-sm-8">
        <div class="d-flex justify-center">
          <v-col md="10" lg="12" class="text-center">
            <h1
              class="text-h3 text-sm-h2 font-weight-bold px-sm-0 px-md-8 mb-3"
            >
              The best place to book<br />hotel deals
            </h1>
            <p class="text-h5 text-sm-h6 mb-3">
              Save time and money with our
              <span
                class="d-inline-block d-md-inline rainbow-text font-weight-bold"
                >incredible member-only prices</span
              >
            </p>
            <div class="pt-3 d-flex justify-center align-center">
              <v-btn
                href="/auth"
                color="primary"
                elevation="0"
                size="large"
                width="200px"
                class="text-none text-subtitle-2 text-bold"
              >
                Get Started
              </v-btn>
              <v-btn
                href="https://project-exterior-technical-test-app.up.railway.app/app"
                variant="outlined"
                color="primary"
                elevation="0"
                size="large"
                width="150px"
                class="text-none text-subtitle-2 text-bold ml-3"
              >
                Download App
              </v-btn>
            </div>
          </v-col>
        </div>
      </div>
      <div class="py-4 my-0 mb-4">
        <v-container class="text-center">
          <v-btn outlined color="primary" rounded class="my-button up-down">
            Try it for yourself
            <v-icon class="arrow-icon">mdi-chevron-down</v-icon>
          </v-btn>
        </v-container>
      </div>
      <div class="text-center">
        <h4>Unlock great deals with up to 50% off your next trip!</h4>
      </div>
      <v-sheet
        class="mt-5 d-flex mx-n4 mx-sm-0 px-4 px-md-6 pt-2 rounded-b-lg bg-white"
        elevation="1"
      >
        <v-col
          md="12"
          sm="12"
          lg="4"
          class="autocomplete-filter flex-md-grow-1 mb-4 mb-lg-0 col-md-auto pa-1"
        >
          <v-text-field
            v-model="searchQuery"
            label="Where are you going?"
            variant="outlined"
            rounded="lg"
            :focused="isFocused"
            clearable
            :color="searchQuery.length > 0 || isFocused ? 'primary' : 'default'"
            :hide-details="true"
            :error="error"
            @click:clear="onClear"
            :error-messages="errorMessages"
            @focus="onFocus"
            @blur="onBlur"
            @update:modelValue="onSearch"
          >
            <template #prepend-inner>
              <v-icon :color="isFocused ? 'primary' : 'default'"
                >mdi-map-marker-outline</v-icon
              >
            </template>
            <template #clear>
              <v-icon
                @click="onClear"
                :color="isFocused ? 'primary' : 'default'"
                >mdi-close-circle</v-icon
              >
            </template>
            <v-menu
              v-model="openMenu"
              height="420px"
              :open-on-click="openMenu"
              target="parent"
            >
              <v-list class="px-2 py-0 pt-2 w-search-autocomplete">
                <div
                  v-if="loading"
                  class="d-flex flex-row align-center justify-center"
                >
                  <AppLoader size="40" />
                </div>
                <div v-if="emptyState" class="pa-2" style="min-height: 64px">
                  <p class="mb-0 text-body-2 text--primary">
                    No results found for
                    <span class="font-weight-medium">{{ searchQuery }}</span>
                  </p>
                  <p class="mb-0 text-body-2 text-secondary">
                    Try other keywords
                  </p>
                </div>
                <v-list-item
                  v-if="!emptyState"
                  v-for="(item, index) in localSuggestions"
                  :key="index"
                  @click="selectItem(item)"
                  class="mb-0 pa-2 rounded-sm"
                  style="cursor: pointer"
                >
                  <div class="location-autocomplete-option">
                    <div class="mb-1">
                      <v-chip
                        :color="getChipColor(item.location_type)"
                        size="x-small"
                        rounded="0"
                        class="ma-0 px-2 py-0 d-inline-flex flex-row flex-nowrap justify-start align-center"
                        style="height: 20px !important"
                      >
                        <v-icon
                          :size="14"
                          class="ma-0"
                          :style="{ marginTop: '-1px' }"
                        >
                          {{ getIcon(item.location_type) }}
                        </v-icon>
                        <span class="ml-2 text-overline font-weight-medium">
                          {{ formatLocationType(item.location_type) }}
                        </span>
                      </v-chip>
                    </div>
                    <p
                      class="mb-0 text-h7 text-truncate text-none"
                      v-html="item.name"
                    ></p>
                    <p
                      class="mb-0 text-caption text--secondary text-truncate"
                      v-html="item.highlight_data.name_suffix"
                    ></p>
                  </div>
                </v-list-item>
              </v-list>
            </v-menu>
          </v-text-field>
        </v-col>
        <v-col
          md="12"
          sm="12"
          lg="3"
          class="date-filter flex-shrink-1 mb-4 mb-lg-0 col-md-auto pa-1"
        >
          <FiltersDateCheckin
        /></v-col>
        <v-col
          md="12"
          sm="12"
          lg="3"
          class="date-filter flex-shrink-1 mb-4 mb-lg-0 col-md-auto pa-1"
        >
          <FiltersGuest />
        </v-col>
        <v-col lg="2" md="12" sm="12" class="col-md-auto pa-1">
          <v-btn
            block
            height="100%"
            rounded="lg"
            size="large"
            variant="flat"
            color="primary"
            href="/stay/fairmont-jakarta-9000248394?checkin=2025-03-04&checkout=2025-03-08&guest_per_room=2&number_of_room=1"
            style="height: 56px"
          >
            <template #prepend>
              <v-icon size="20">mdi-magnify</v-icon>
            </template>
            <span class="text-none text-subtitle-1 font-weight-medium"
              >Search</span
            >
          </v-btn>
        </v-col>
      </v-sheet>
      <v-container class="py-16">
        <!-- Hotels Section -->
        <v-row class="mb-16">
          <v-col cols="12" md="6">
            <div class="d-flex align-start mb-4">
              <v-icon color="primary" size="32" class="me-4">mdi-hotel</v-icon>
              <div>
                <h2 class="text-h4 font-weight-bold mb-4">
                  Huge selections of hotels
                </h2>
                <p class="text-body-1">
                  We partner with various hotel chains all across the world.
                  Discover 700,000+ domestic and international hotels, including
                  apartments and villas.
                </p>
              </div>
            </div>
          </v-col>
          <v-col cols="12" md="6">
            <v-row>
              <v-col>
                <img
                  src="/assets/img/huge.png"
                  alt="huge"
                  class="w-100 grey lighten-2"
                />
              </v-col>
            </v-row>
            <div class="text-center mt-4">...and many more!</div>
          </v-col>
        </v-row>

        <!-- Deals Section -->
        <v-row class="mb-16">
          <v-col cols="12" md="6">
            <div class="d-flex align-start mb-4">
              <v-icon color="primary" size="32" class="me-4"
                >mdi-tag-multiple</v-icon
              >
              <div>
                <h2 class="text-h4 font-weight-bold mb-4">Unbeatable deals</h2>
                <p class="text-body-1">
                  Save up to 50% or on average 30% compared to other OTA. We use
                  state-of-the-art Machine Learning and Artificial Intelligence
                  to find you the best deals.
                </p>
              </div>
            </div>
          </v-col>
          <v-col cols="12" md="6" class="d-flex justify-center">
            <img
              src="/assets/img/deals.png"
              alt="Deals Illustration"
              class="w-100"
            />
          </v-col>
        </v-row>

        <!-- Payment Section -->
        <v-row>
          <v-col cols="12" md="6">
            <div class="d-flex align-start mb-4">
              <v-icon color="primary" size="32" class="me-4"
                >mdi-credit-card-multiple</v-icon
              >
              <div>
                <h2 class="text-h4 font-weight-bold mb-4">
                  Various payment options
                </h2>
                <p class="text-body-1">
                  Be flexible with choices of payment. Use eWallet, Credit Card,
                  QRIS, Bank Transfer, or Pay Later. All payment methods are
                  100% security guaranteed.
                </p>
              </div>
            </div>
          </v-col>
          <v-col cols="12" md="6">
            <img src="/assets/img/payment.png" class="grey lighten-2 w-100" />
          </v-col>
        </v-row>

        <!-- call center -->
        <v-row>
          <v-col cols="12" md="6">
            <div class="d-flex align-start mb-4">
              <v-icon color="primary" size="32" class="me-4"
                >mdi-phone</v-icon
              >
              <div>
                <h2 class="text-h4 font-weight-bold mb-4">
                  Various payment options
                </h2>
                <p class="text-body-1">
                  Be flexible with choices of payment. Use eWallet, Credit Card,
                  QRIS, Bank Transfer, or Pay Later. All payment methods are
                  100% security guaranteed.
                </p>
              </div>
            </div>
          </v-col>
          <v-col cols="12" md="6">
            <img src="/assets/img/support.png" class="grey lighten-2 w-100" />
          </v-col>
        </v-row>

        <!-- achievements -->
        <v-row class="mb-16">
          <v-col cols="12" md="6">
            <div class="d-flex align-start mb-4">
              <v-icon color="primary" size="32" class="me-4">mdi-trophy-outline</v-icon>
              <div>
                <h2 class="text-h4 font-weight-bold mb-4">
                  Our achievements
                </h2>
                <p class="text-body-1">
                  We partner with various hotel chains all across the world.
                  Discover 700,000+ domestic and international hotels, including
                  apartments and villas.
                </p>
              </div>
            </div>
          </v-col>
          <v-col cols="12" md="6">
            <v-row>
              <v-col>
                <img
                  src="/assets/img/achievement.png"
                  alt="huge"
                  class="w-100 grey lighten-2"
                />
              </v-col>
            </v-row>
            <div class="text-center mt-4">...and many more!</div>
          </v-col>
        </v-row>
      </v-container>
      <AppFooter />
    </div>
  </NuxtLayout>
</template>
<script setup>
  useHead({
    title: 'Wisata App: Book Member Only Hotel Deals',
  })
  const filterStore = useFiltersStore()
  const route = useRoute()
  const propertyContentStore = usePropertyContentStore()
  const {
    searchQuery,
    suggestions,
    loading,
    error,
    errorMessages,
    fetchSuggestions,
  } = useLocationSearch()

  const slug = route.params.slug
  const openMenu = ref(false)
  const isFocused = ref(true)
  const onFocus = () => {
    isFocused.value = true
    if (searchQuery.value) {
      openMenu.value = true
      onSearch(searchQuery.value)
    }
  }
  const onClear = () => {
    searchQuery.value = ''
  }

  const onBlur = () => {
    isFocused.value = false
  }

  const onSearch = (query) => {
    if (query && query.length >= 3) {
      fetchSuggestions(query.replace(/\s+/g, '+'))
      openMenu.value = true
    } else {
      openMenu.value = false
    }
  }

  const selectItem = (item) => {
    searchQuery.value = `${item.name}, ${item.name_suffix}`
    openMenu.value = false
    filterStore.setFilters({
      checkin: filterStore.checkin,
      checkout: filterStore.checkout,
      guest_per_room: filterStore.guest_per_room,
      number_of_room: filterStore.number_of_room,
      slug: item.slug,
    })
  }

  const getChipColor = (locationType) => {
    if (locationType === 'property') {
      return 'primary'
    } else if (locationType === 'area') {
      return 'success'
    }
    return 'success'
  }

  const getIcon = (locationType) => {
    if (locationType === 'property') {
      return 'mdi-bed-outline'
    } else if (locationType === 'area') {
      return 'mdi-map-marker-outline'
    }
    return 'mdi-domain'
  }

  const formatLocationType = (locationType) => {
    return locationType.replace(/_/g, ' ')
  }
  const localSuggestions = computed(() => {
    return searchQuery.value && searchQuery.value.length >= 3
      ? suggestions.value
      : []
  })

  const emptyState = computed(() => {
    return (
      !loading.value &&
      !error.value &&
      searchQuery.value &&
      suggestions.value.length === 0
    )
  })

  const propertyId = extractPropertyId(slug)
  const propertyData = computed(
    () => propertyContentStore.properties[propertyId] || {},
  )
  watchEffect(() => {
    console.log('🚀 ~ watchEffect ~ propertyData:', propertyData)
    // if (propertyData.value?.name && propertyData.value?.name_suffix) {
    //   searchQuery.value = `${propertyData.value.name}, ${propertyData.value.name_suffix}`
    // }
  })
</script>
<style scoped>
  body {
    font-family:
      Google Sans,
      BlinkMacSystemFont,
      sans-serif !important;
  }
  .rainbow-text {
    -webkit-text-fill-color: transparent;
    background: linear-gradient(
      90deg,
      #6a5acd,
      violet 20%,
      pink 30%,
      #b22222 40%,
      tomato 50%,
      #ff8c00 55%,
      green 65%,
      #1e90ff 80%,
      #add8e6
    );
    -webkit-background-clip: text;
    background-clip: text;
  }
  .my-button {
    /* Base styles for the button */
    /* ... */
  }
  .my-button:before {
    position: absolute;
    top: 0;
    left: -500px;
    content: '';
    width: 100px;
    height: 40px;
    background: rgba(255, 255, 255, 0.6);
    transform: skew(-50deg);
    /* transition: 1s; */
    animation: shine 3s ease infinite;
  }
  @keyframes shine {
    from {
      left: -500px;
    }
    to {
      left: 655px;
    }
  }

  .up-down {
    animation: up-down linear 4s;
    animation-iteration-count: infinite;
    transform-origin: 50% 50%;
    -webkit-animation: up-down linear 4s;
    -webkit-animation-iteration-count: infinite;
    -webkit-transform-origin: 50% 50%;
    -moz-animation: up-down linear 4s;
    -moz-animation-iteration-count: infinite;
    -moz-transform-origin: 50% 50%;
    -o-animation: up-down linear 4s;
    -o-animation-iteration-count: infinite;
    -o-transform-origin: 50% 50%;
    -ms-animation: up-down linear 4s;
    -ms-animation-iteration-count: infinite;
    -ms-transform-origin: 50% 50%;
  }

  @keyframes up-down {
    0% {
      transform: translate(1px, 20px);
    }
    24% {
      transform: translate(1px, 30px);
    }
    50% {
      transform: translate(1px, 12px);
    }
    74% {
      transform: translate(1px, 22px);
    }
    100% {
      transform: translate(1px, 22px);
    }
  }

  @-moz-keyframes up-down {
    0% {
      -moz-transform: translate(1px, 20px);
    }
    24% {
      -moz-transform: translate(1px, 30px);
    }
    50% {
      -moz-transform: translate(1px, 12px);
    }
    74% {
      -moz-transform: translate(1px, 22px);
    }
    100% {
      -moz-transform: translate(1px, 22px);
    }
  }

  @-webkit-keyframes up-down {
    0% {
      -webkit-transform: translate(1px, 20px);
    }
    24% {
      -webkit-transform: translate(1px, 30px);
    }
    50% {
      -webkit-transform: translate(1px, 12px);
    }
    74% {
      -webkit-transform: translate(1px, 22px);
    }
    100% {
      -webkit-transform: translate(1px, 22px);
    }
  }

  @-o-keyframes up-down {
    0% {
      -o-transform: translate(1px, 20px);
    }
    24% {
      -o-transform: translate(1px, 30px);
    }
    50% {
      -o-transform: translate(1px, 12px);
    }
    74% {
      -o-transform: translate(1px, 22px);
    }
    100% {
      -o-transform: translate(1px, 22px);
    }
  }

  @-ms-keyframes up-down {
    0% {
      -ms-transform: translate(1px, 20px);
    }
    24% {
      -ms-transform: translate(1px, 30px);
    }
    50% {
      -ms-transform: translate(1px, 12px);
    }
    74% {
      -ms-transform: translate(1px, 22px);
    }
    100% {
      -ms-transform: translate(1px, 22px);
    }
  }
</style>
