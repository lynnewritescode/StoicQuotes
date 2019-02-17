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
        <figure
          v-if="info"
          aria-role="article"
          tab-index="0"
        >
          <blockquote
            class="quote"
            aria-live="assertive"
          >
            <span>{{ getCurrentQuote() }}</span>
          </blockquote>
        </figure>
        <figcaption
          v-if="info"
          tab-index="0"
        >
          <cite aria-live="assertive">- {{ getCurrentAuthor() }}</cite>
        </figcaption>

        <div class="call-to-action">
          <button
            class="refresh"
            tab-index="0"
            title="Generate a new quote"
            aria-describedby="refreshQuote"
            @click="randomQuote()"
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
      </main>
    </transition>
  </section>
</template>

<script>
import { setTimeout } from 'timers'
import axios from 'axios'

export default {
  data() {
    return {
      show: false,
      info: null,
      randomIndex: 0
    }
  },

  mounted() {
    const scope = this
    axios
      .get('https://randomstoicquotesapi.herokuapp.com/api/v1/quotes')
      .then(response => (this.info = response.data))
      .then(function() {
        scope.show = true
      })
      .then(this.randomQuote)
  },

  methods: {
    getCurrentQuote: function() {
      return this.info.data[this.randomIndex].attributes.text
    },

    getCurrentAuthor: function() {
      const scope = this
      const authorObj = this.info.included.filter(obj => {
        return (
          obj.id ===
          scope.info.data[scope.randomIndex].relationships.author.data.id
        )
      })[0]

      return authorObj.attributes.name
    },

    randomQuote: function() {
      const scope = this
      this.show = false
      this.randomIndex =
        Math.floor(Math.random() * (this.info.data.length - 1)) + 1

      setTimeout(function() {
        scope.show = true
      }, 0)
    }
  }
}
</script>

<style lang="scss">
.container {
  margin: 0 auto;
  display: flex;
  width: 100%;
  max-width: 1200px;
  justify-content: flex-start;
  align-items: center;
  text-align: center;
}

.title {
  font-family: Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 50px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle,
.quote {
  font-weight: 300;
  font-size: 32px;
  color: #526488;
  word-spacing: 5px;
  margin: 20px 0;
  padding-bottom: 15px;
}

.quote-wrapper {
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  min-height: 100vh;
  align-items: center;
  justify-content: center;
}

.quote {
  font-size: 3.6rem;
  line-height: 64px;
  display: block;
  margin-bottom: 10px;
  max-width: 800px;
  font-family: 'Playfair Display', Roboto, 'Helvetica Neue', Arial, sans-serif;
  quotes: '\201C' '\201D' '\2018' '\2019';

  &:before {
    content: open-quote;
    font-size: 4em;
    line-height: 0.1em;
    display: block;
  }
}

.call-to-action {
  display: block;
  margin-top: 40px;
}

.refresh {
  border: 0;
  background: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  appearance: none;

  &:focus {
    outline: none;
  }
}

.aria-helper {
  display: none;
}
</style>
