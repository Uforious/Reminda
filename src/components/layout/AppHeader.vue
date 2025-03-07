<template>
    <!-- Desktop Header -->
    <v-container v-if="!isMobile"  class="py-0 px-0" fluid max-width="100%">
        <v-toolbar flat class="d-flex justify-center">
            <v-img src="@/assets/logo.png" max-height="40" max-width="40" class="mr-3 ml-3" />
            <v-toolbar-title class="text-h5 font-weight-bold">{{ appName }}</v-toolbar-title>
        </v-toolbar>
    </v-container>

    <!-- Mobile App Bar -->
    <v-app-bar v-if="isMobile" app dense>
        <v-spacer />
        <v-img src="@/assets/logo.png" max-height="40" max-width="40" class="mr-2" />
        <v-toolbar-title>{{ appName }}</v-toolbar-title>
        <v-spacer />
    </v-app-bar>
</template>

<script setup>
import { ref, onMounted } from "vue";
import { useDisplay } from "vuetify";

const { mobile } = useDisplay();
const isMobile = ref(mobile);
const appName = "Reminder"

onMounted(() => {
    isMobile.value = window.innerWidth < 600;
    window.addEventListener("resize", () => {
        isMobile.value = window.innerWidth < 600;
    });
});
</script>