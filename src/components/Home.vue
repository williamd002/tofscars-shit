<template>
  <div class="home">
    <!--
    <form @submit.prevent="connect" class="search-container spacing-y">
      <input
        class="search-bar"
        placeholder="Enter queue link..."
        v-model="queueLink"
      >
      <button
        class="btn btn-standard spacing-x"
        type="submit"
      >
        Connect
      </button>
    </form>

    <h2 v-if="badQueueLink" class="error-text">
      Cannot queue songs yet...
    </h2>
     
    <p class="spacing-y">...or...</p>
    -->

    <div>
      <button
        v-if="authorized"
        class="btn btn-primary btn-big spacing-x"
        @click="newQueue"
      >
        Create new queue
      </button>
    <!--<div v-else>
        <a
          class="btn btn-primary btn-big spacing-x"
          :href="loginUrl"
        >
          Connect to Spotify
        </a>
        <p class="spacing-y">...to create queue</p>
      </div>-->
    </div>

    <div class="description">
      <h1>Spotify Queuer - TOFS Edition</h1>
      <button
        class="btn btn-big spacing-x"
        style="background: darkred;margin-top: 15px; color: white;"
        @click="enterParty"
      >
        Enter the party
      </button>
      <!--<ol>
        <li>Connect to Spotify.</li>
        <li>Create new queue.</li>
        <li>Share the queue link with your friends.</li>
        <li>Everybody adds tracks.</li>
      </ol>
      <ul class="perks">
        <li>
          The queue is fair! Everybody has an
          equal chance to get their song next.
        </li>
        <li>Only the host of the queue needs a Spotify account!</li>
      </ul>

      <h4>New features:</h4>
      <ul>
        <li>Now fading to the next song is supported.</li>
        <li>Displays next song and the upcoming songs in the queue.</li>
      </ul>

      <p>
        To report bugs, or if you have suggestions of new features,
        either go to the
        <a
         target="_blank"
         href="https://github.com/mattiasahlsen/spotify-queue"
        >
        github page
        </a><br>
        or email me at
        <a target="_blank" @click.prevent="openMail">
          mattias.ahlsen@gmail.com
        </a>
      </p>-->
    </div>

  </div>
</template>

<script>
import querystring from 'querystring'
import config from '../config'
import { showErr } from '../lib'

export default {
  data() {
    return {
      queueLink: '',
      badQueueLink: false,
    }
  },
  computed: {
    authorized() {
      return this.$store.getters.authorized
    },
    loginUrl() {
      return config.server + '/login?' + querystring.stringify({
        redirect: window.location.origin + window.location.pathname
      })
    },
  },
  methods: {/* 
    openMail() {
      window.location.href =
      "mailto:mattias.ahlsen@gmail.com?subject=Subject&body=Message%20goes%20here";
    }, */
    connect() {
      const link = this.queueLink.includes('http') ?
        this.queueLink : 'http://' + this.queueLink

      let url
      try {
        url = new URL(link)
      } catch (err) {
        this.badQueueLink = true
        return
      }

      if (
        url.origin !== window.location.origin ||
        url.pathname === '/'
      ) {
        this.badQueueLink = true
        return
      }
      this.badQueueLink = false
      if (url.pathname !== window.location.pathname) {
        this.$router.push(url.pathname)
      }
    },
    newQueue() {
      return this.$store.dispatch('newQueue').catch(showErr)
    },
    enterParty() {
        this.$router.push('/play')
    },
  }
}
</script>

<style scoped lang="scss">
.home {
  text-align: center;
}
.description {
  text-align: center;
  margin-top: 2em;
}
.perks {
  font-size: 1.2em;
}
</style>
