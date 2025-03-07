<script setup>
import { ref, onMounted } from "vue";
import AppLayout from "@/layouts/AppLayout.vue";
import html2canvas from "html2canvas";
import { useDisplay } from "vuetify";

const items = ref([]);
const selectedItem = ref(null);
const cardToShare = ref(null);
const capturedImageUrl = ref(null);
const dialShare = ref(false);
const pageUrl = "https://techformist.com";

const activeNav = 'top';


const { mobile } = useDisplay();
const isMobile = ref(mobile);

// Fetch items from JSON
const fetchItems = async () => {
  try {
    const response = await fetch("/data/items.json");
    if (!response.ok) {
      throw new Error("Failed to fetch JSON");
    }
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
  const randomIndex = Math.floor(Math.random() * items.value.length);
  selectedItem.value = items.value[randomIndex];
};

const downloadImage = () => {
  if (items.value.length === 0) return;
  const randomIndex = Math.floor(Math.random() * items.value.length);
  selectedItem.value = items.value[randomIndex];
};

// const convertToImage = () => {
//   if (!selectedItem.value) return;

//   html2canvas(cardToShare.value, { scale: window.devicePixelRatio }).then((canvas) => {
//     const imageUrl = canvas.toDataURL();
//     alert("Image captured! You can now share it.");
//     // Implement sharing logic here
//   });
// };


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
const platforms = {
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
const shareToSocialMedia = (choice) => {
  if (!selectedItem.value) return;

  const text = encodeURIComponent(selectedItem.value.text);
  const url = encodeURIComponent(pageUrl);

  if (platformUrls[choice]) {
    window.open(platformUrls[choice](text, url), "_blank");
  } else if (choice === "instagram") {
    captureCardImage();
    alert("Image saved! Open Instagram and upload it manually.");
  } else {
    alert("Invalid choice");
  }
};

onMounted(() => {
  isMobile.value = window.innerWidth < 600;
  window.addEventListener("resize", () => {
    isMobile.value = window.innerWidth < 600;
  });
});

onMounted(fetchItems);
</script>

<template>
  <AppLayout>
    <v-container class="fill-height container">
      <v-responsive class="align-center mx-auto">

        <v-row v-if="selectedItem">
          <v-col cols="12">
            <v-card ref="cardToShare" class="mx-auto" max-width="350" rounded="lg" variant="outlined">
              <v-img height="40vh" :src="selectedItem.image" :alt="selectedItem.text" cover />
              <v-card-title class="text-wrap text-center">
                {{ selectedItem.text }}
              </v-card-title>
            </v-card>
          </v-col>
        </v-row>

        <v-row v-if="!isMobile">
          <v-col cols="12">
            <v-card class="mx-auto" max-width="350">
              <v-card-actions>
                <v-btn class="me-2 text-none" color="#4f545c" prepend-icon="mdi-shuffle" variant="flat"
                  @click="selectRandomItem">
                  Shuffle
                </v-btn>

                <v-btn class="me-2 text-none" color="#4f545c" prepend-icon="mdi-download" variant="flat"
                  @click="downloadImage">
                  Download
                </v-btn>

                <v-speed-dial v-model="dialShare" absolute right bottom direction="left" open-on-hover>
                  <template v-slot:activator>
                    <v-btn fab bottom small color="#4f545c" class="me-2 text-none" prepend-icon="mdi-export-variant"
                      variant="flat">
                      <!-- <v-icon v-if="dialShare">mdi-close</v-icon>
                    <v-icon v-else>mdi-share-variant</v-icon> -->
                      Share
                    </v-btn>
                  </template>

                  <v-btn dark fab bottom color="blue darken-7" small
                    :href="`https://www.linkedin.com/shareArticle?mini=true&url=${pageUrl}`" target="_blank">
                    <v-icon>mdi-linkedin</v-icon>
                  </v-btn>

                  <!-- <v-btn dark fab bottom color="blue" small
                  :href="`https://www.facebook.com/sharer/sharer.php?u=${pageUrl}`" target="_blank">
                  <v-icon>mdi-facebook</v-icon>
                </v-btn> -->

                  <!-- <v-btn dark fab bottom color="green" small
                  :href="`https://wa.me/?text=Checkout%20this%20page.%20${pageUrl}`" target="_blank">
                  <v-icon>mdi-whatsapp</v-icon>
                </v-btn> -->

                  <!-- <v-btn dark fab bottom color="tertiary" small
                  :href="`mailto:?subject=Check this out!&body=Check this out! ${pageUrl}`" target="_blank">
                  <v-icon>mdi-email</v-icon>
                </v-btn> -->
                </v-speed-dial>
              </v-card-actions>
            </v-card>
          </v-col>
        </v-row>
      </v-responsive>

      <!-- Mobile Bottom Navigation -->
      <v-bottom-navigation v-if="isMobile" app fixed grow v-model="activeNav">
      <!-- <v-bottom-navigation v-if="isMobile" app> -->
        <v-btn @click="selectRandomItem" value="shuffle">
          <v-icon>mdi-shuffle</v-icon>
          <span>Shuffle</span>
        </v-btn>
        <v-btn @click="shareToSocialMedia" value="share">
          <v-icon>mdi-export-variant</v-icon>
          <span>Share</span>
        </v-btn>
        <v-btn @click="downloadImage" value="download">
          <v-icon>mdi-download</v-icon>
          <span>Download</span>
        </v-btn>
      </v-bottom-navigation>
    </v-container>
  </AppLayout>
</template>



<style>
@media (min-width: 960px) {
    .v-container {
        max-width: 100%;
    }
}
</style>
