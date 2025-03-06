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

      <div v-if="selectedItem" class="card">
        <!-- <img :src="selectedItem.image" :alt="selectedItem.text" class="image" /> -->
        <h1 class="text-h2 font-weight-bold">{{ selectedItem.text }}</h1>
      </div>

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
                @click="shareToSocialMedia"
              >
                Share
              </v-btn>
              <v-spacer/>
              <v-btn
                class="text-none"
                prepend-icon="mdi-check"
                variant="text"
                border
                @click="selectRandomItem"
              >
                Select Post
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>

    </v-responsive>
  </v-container>
</template>


<!-- <template>
  <div class="app-container">
    <div v-if="selectedItem" class="card">
      <img :src="selectedItem.image" :alt="selectedItem.text" class="image" />
      <p class="text">{{ selectedItem.text }}</p>
    </div>
    <button @click="selectRandomItem">Select Random</button>
    <button @click="shareToSocialMedia">Share</button>
  </div>
</template> -->


<script setup>
import { ref, onMounted } from "vue";

const items = ref([]);
const selectedItem = ref(null);

const fetchItems = async () => {
  try {
    const response = await fetch("/data/items.json");
    if (!response.ok) {
      throw new Error("Failed to fetch JSON");
    }
    const data = await response.json();
    console.log("Items loaded:", data); // Debugging
    items.value = data;
    selectRandomItem();
  } catch (error) {
    console.error("Error loading JSON data:", error);
  }
};

const selectRandomItem = () => {
  if (items.value.length === 0) {
    console.warn("No items available");
    return;
  }
  const randomIndex = Math.floor(Math.random() * items.value.length);
  selectedItem.value = items.value[randomIndex];
  console.log("Selected Item:", selectedItem.value); // Debugging line
};

const shareToSocialMedia = () => {
  if (!selectedItem.value) return;
  const text = encodeURIComponent(selectedItem.value.text);
  const url = encodeURIComponent(selectedItem.value.image);
  
  const options = {
    facebook: `https://www.facebook.com/sharer/sharer.php?u=${url}`,
    twitter: `https://twitter.com/intent/tweet?text=${text}&url=${url}`,
    whatsapp: `https://api.whatsapp.com/send?text=${text}%20${url}`
  };
  
  const choice = prompt("Share on: Facebook, Twitter, or WhatsApp?").toLowerCase();
  if (options[choice]) {
    window.open(options[choice], "_blank");
  } else {
    alert("Invalid choice");
  }
};

onMounted(fetchItems);
</script>

<style>

</style>
