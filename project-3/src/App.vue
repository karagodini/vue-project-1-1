<template>
  <p style="position: absolute; top: 40px; z-index: 0;">
    Текст под бубликом который видно в дырке
  </p>
  <DonutButton :size="160" @click="handleClick">
    <template #prepend>
      <v-icon>mdi-star</v-icon>
    </template>
    Нажми меня
    <template #append>
      <v-icon>mdi-chevron-right</v-icon>
    </template>
  </DonutButton>
</template>

<script>
import DonutButton from './components/DonutButton.vue'

export default {
  components: { DonutButton },
  methods: {
    handleClick() {
      alert('Клик прошёл по кольцу!');
    }
  }
}
</script>
