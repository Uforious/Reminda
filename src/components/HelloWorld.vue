<script setup>
import { ref, onMounted } from "vue";
import html2canvas from "html2canvas";

const dialShare = ref(false);
const pageUrl = "https://techformist.com";

const items = ref([]);
const selectedItem = ref(null);
const cardToShare = ref(null);
const capturedImageUrl = ref(null);

// Fetch items from JSON
const fetchItems = async () => {
  try {
    const response = await fetch("/data/items.json");
    if (!response.ok) throw new Error("Failed to fetch JSON");

    const data = await response.json();
    items.value = data;
    selectRandomItem();
  } catch (error) {
    console.error("Error loading JSON data:", error);
  }
};

// Select a random item
const selectRandomItem = () => {
  if (items.value.length === 0) return;
  selectedItem.value = items.value[Math.floor(Math.random() * items.value.length)];
};

// Capture the card content as an image
const captureCardImage = async () => {
  if (!cardToShare.value) return;

  const canvas = await html2canvas(cardToShare.value, {
    scale: window.devicePixelRatio,
    useCORS: true,
  });

  capturedImageUrl.value = canvas.toDataURL("image/png");

  // Automatically download the image (for Instagram, etc.)
  const link = document.createElement("a");
  link.href = capturedImageUrl.value;
  link.download = "shared-post.png";
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
};

// Platform sizes for image sharing
const platformSizes = {
  facebook: { width: 1200, height: 630 },
  twitter: { width: 1200, height: 675 },
  whatsapp: { width: 1080, height: 1080 },
  instagram: { width: 1080, height: 1080 },
  linkedin: { width: 1200, height: 627 },
};

// Platform share URLs
const platformUrls = {
  facebook: (text, url) => `https://www.facebook.com/sharer/sharer.php?u=${url}&quote=${text}`,
  twitter: (text, url) => `https://twitter.com/intent/tweet?text=${text}&url=${url}`,
  whatsapp: (text, url) => `https://api.whatsapp.com/send?text=${text}%20${url}`,
  linkedin: (text, url) => `https://www.linkedin.com/sharing/share-offsite/?url=${url}`,
};

// Handle social media sharing
const shareToSocialMedia = () => {
  if (!selectedItem.value) return;

  const text = encodeURIComponent(selectedItem.value.text);
  const url = encodeURIComponent(pageUrl);

  const choice = prompt("Share on: Facebook, Twitter, WhatsApp, LinkedIn, or Instagram?").toLowerCase();

  if (platformUrls[choice]) {
    window.open(platformUrls[choice](text, url), "_blank");
  } else if (choice === "instagram") {
    captureCardImage();
    alert("Image saved! Open Instagram and upload it manually.");
  } else {
    alert("Invalid choice");
  }
};

onMounted(fetchItems);
</script>

<template>
  <v-container class="fill-height">
    <v-responsive class="align-center mx-auto" max-width="900" style="height: 100vh;">
      <v-img class="mb-4" height="10vh" src="@/assets/logo.png" />

      <v-row v-if="selectedItem">
        <v-col cols="12">
          <v-card ref="cardToShare" class="mx-auto" max-width="344" rounded="lg" variant="outlined">
            <v-img height="40vh" :src="selectedItem.image" :alt="selectedItem.text" cover />
            <v-card-title class="text-wrap text-center">
              {{ selectedItem.text }}
            </v-card-title>
          </v-card>
        </v-col>
      </v-row>

      <v-row>
        <v-col cols="12">
          <v-card class="mx-auto" max-width="344">
            <v-card-actions>
              <v-btn class="text-none" prepend-icon="mdi-shuffle" variant="text" @click="selectRandomItem">
                Shuffle Post
              </v-btn>
              <v-spacer />
              <!-- <v-btn class="me-2 text-none" color="#4f545c" prepend-icon="mdi-export-variant" variant="flat"
                @click="shareToSocialMedia">
                Share
              </v-btn> -->

              <v-speed-dial v-model="dialShare" absolute right bottom direction="left" open-on-hover>
                <template v-slot:activator>
                  <v-btn fab bottom small color="#4f545c" class="me-2 text-none" prepend-icon="mdi-export-variant" variant="flat">
                    <!-- <v-icon v-if="dialShare">mdi-close</v-icon>
                    <v-icon v-else>mdi-share-variant</v-icon> -->
                    Share
                  </v-btn>
                </template>

                <v-btn dark fab bottom color="blue darken-7" small
                  :href="`https://www.linkedin.com/shareArticle?mini=true&url=${pageUrl}`" target="_blank">
                  <v-icon>mdi-linkedin</v-icon>
                </v-btn>

                <v-btn dark fab bottom color="blue" small
                  :href="`https://www.facebook.com/sharer/sharer.php?u=${pageUrl}`" target="_blank">
                  <v-icon>mdi-facebook</v-icon>
                </v-btn>

                <v-btn dark fab bottom color="green" small
                  :href="`https://wa.me/?text=Checkout%20this%20page.%20${pageUrl}`" target="_blank">
                  <v-icon>mdi-whatsapp</v-icon>
                </v-btn>

                <v-btn dark fab bottom color="tertiary" small
                  :href="`mailto:?subject=Check this out!&body=Check this out! ${pageUrl}`" target="_blank">
                  <v-icon>mdi-email</v-icon>
                </v-btn>
              </v-speed-dial>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>


    </v-responsive>
  </v-container>
</template>

<style scoped></style>
