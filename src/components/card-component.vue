<script setup lang="ts">
  defineProps({
    imgSrc:String,
    title:String
  })
</script>

<template>
  <section class="card">
    <div>
      <img :src="imgSrc" alt="img" height="200"/>
      <p>
        {{ title }}
      </p>
    </div>
  </section>
</template>