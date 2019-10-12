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
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
        >
          <path
            fill="#f3f3f3"
            d="M17.65 6.35C16.2 4.9 14.21 4 12 4c-4.42 0-7.99 3.58-7.99 8s3.57 8 7.99 8c3.73 0 6.84-2.55 7.73-6h-2.08c-.82 2.33-3.04 4-5.65 4-3.31 0-6-2.69-6-6s2.69-6 6-6c1.66 0 3.14.69 4.22 1.78L13 11h7V4l-2.35 2.35z"
          />
          <path
            fill="none"
            d="M0 0h24v24H0z"
          />
        </svg>
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
import { setTimeout } from 'timers'
import quotes from '~/assets/quotes.json'
import QuoteComponent from '@/components/Quote.vue'

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
