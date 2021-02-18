<template>
  <div class="home">
    <h1>Game:</h1>
    <remember-game-component/>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import RememberGameComponent from '@/components/RememberGameComponent.vue';

export default defineComponent({
  name: 'RememberGameView',
  components: {
    RememberGameComponent,
  },
});
</script>
