<script setup lang="ts">

import DataTransferForm from "@/components/DataTransferForm.vue";
</script>

<template>
  <header>
    <div class="wrapper">
      <a href="https://www.fast-forward-it.de" target="_blank"><img class="logo" src="./assets/logo_weiss.png" alt="Fast Forward IT Logo" /></a>
      <h1>Liferay Object Entries Transfer</h1>
    </div>
  </header>

  <main>
    <data-transfer-form></data-transfer-form>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
  width: 300px;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
