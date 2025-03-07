<template>
  <v-app>
    <AppHeader />
    <MainContent class="mainContent">
      <slot />
    </MainContent>
    <AppFooter />
  </v-app>
</template>

<script setup>
import AppHeader from "@/components/layout/AppHeader.vue";
import AppFooter from "@/components/layout/AppFooter.vue";
import MainContent from "@/components/layout/MainContent.vue";

</script>

<style scoped>
.mainContent {
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #333;
  font-size: 2rem;
  text-align: center;
  background: linear-gradient(45deg,
      #FFD6E0, #FFEBB2, #B2F7EF, #C4FAF8, #FFB3BA, #FFDAC1);
  background-size: 400% 100%;
  animation: gradient 10s ease-in-out infinite;
}

@keyframes gradient {
  0% {
    background-position: 0% 50%;
  }

  100% {
    background-position: 100% 50%;
  }
}
</style>