<script setup lang="ts">

import HomeItem from "@/components/HomeItem.vue";
import HomeItemElements from "@/components/HomeItemElements.vue"
</script>

<template>
  <main>
    <HomeItem>
      <template #heading>
        Elementy rozwijane i zwijane.
      </template>
    </HomeItem>
    <HomeItemElements></HomeItemElements>
  </main>
</template>
