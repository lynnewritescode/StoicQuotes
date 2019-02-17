<template>
  <section
    class="container"
    tab-index="0"
  >
    <transition name="fade">
      <main
        v-if="show"
        class="quote-wrapper"
        tab-index="0"
        aria-live="assertive"
        aria-atomic="true"
      >
        <QuoteComponent
          :quote="quote"
          :author="author"
        />
      </main>
    </transition>
    <div class="call-to-action">
      <button
        class="refresh"
        :class="{'active': isActive}"
        tab-index="0"
        title="Generate a new quote"
        aria-describedby="refreshQuote"
        @click="getQuote()"
      >
        <unicon
          name="redo"
          fill="#526488"
        />
      </button>
      <div
        id="refreshQuote"
        class="aria-helper"
      >
        This will refresh the quote displayed on the page.
      </div>
    </div>
  </section>
</template>

<script>
import quotes from '~/assets/quotes.json'
import QuoteComponent from '@/components/Quote.vue'
import { setTimeout } from 'timers'

export default {
  components: {
    QuoteComponent
  },

  data() {
    return {
      show: false,
      quote: null,
      author: null,
      isActive: false
    }
  },

  mounted() {
    this.getQuote()
  },

  methods: {
    getQuote: function() {
      const scope = this
      this.show = false
      this.isActive = true
      const quote = quotes[Math.floor(Math.random() * quotes.length)]
      this.quote = quote.quote
      this.author = quote.author
      setTimeout(function() {
        scope.show = true
        scope.isActive = false
      }, 500)
    }
  }
}
</script>

<style lang="scss" scoped>
@import '@/assets/scss/pages/index.scss';
</style>
