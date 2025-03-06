<template>
  <v-container class="fill-height">
    <v-responsive
      class="align-centerfill-height mx-auto"
      max-width="900"
    >
      <v-img
        class="mb-4"
        height="150"
        src="@/assets/logo.png"
      />

      <div class="text-center">
        <div class="text-body-2 font-weight-light mb-n1">Welcome to</div>

        <h1 class="text-h2 font-weight-bold">Reminda</h1>
      </div>

      <div class="py-4" />

      <v-row>
        <v-col cols="12">
          <v-card
            class="mx-auto"
            max-width="344"
            rounded="lg"
            variant="outlined"
          >
            <v-img
              height="400px"
              src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg"
              cover
            />

            <v-card-title>
              Top western road trips
            </v-card-title>
          
            <v-spacer/>
          </v-card>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="12">
          <v-card
            class="mx-auto"
            max-width="344"
          >
            <v-card-actions>
              <!-- <v-btn
                color="orange-lighten-2"
                text="Explore"
              /> -->

              <v-btn
                class="me-2 text-none"
                color="#4f545c"
                prepend-icon="mdi-export-variant"
                variant="flat"
              >
                Share
              </v-btn>
              <v-spacer/>
              <v-btn
                class="text-none"
                prepend-icon="mdi-check"
                variant="text"
                border
              >
                Interested
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>



    </v-responsive>
  </v-container>
</template>

<script setup>
  //
</script>
