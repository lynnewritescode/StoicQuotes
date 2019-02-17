<template>
  <section>
    <figure
      aria-role="article"
      tab-index="0"
    >
      <blockquote
        v-if="quote"
        class="quote"
        aria-live="assertive"
      >
        <span>{{ quote }}</span>
      </blockquote>
    </figure>
    <figcaption
      v-if="author"
      tab-index="0"
    >
      <cite
        class="e-author"
        aria-live="assertive"
      >- {{ author }}</cite>
    </figcaption>
  </section>
</template>

<script>
export default {
  props: {
    quote: {
      type: String,
      default: null
    },
    author: {
      type: String,
      default: null
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/components/quote.scss';
</style>
