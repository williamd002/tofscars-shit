<template>
  <div class="nav">
    <div class="logo">
      <img src="/tofs_logo.png" @click="goHome" class="logo-image clickable">
    </div>
    <div v-if="authorized" class="nav-content">
      <button class="btn btn-secondary spacing-right" @click="logout">LOG OUT</button>
      <p class="username">{{userId}}</p>
    </div>
    <!--<div v-else>
      <a
        class="btn btn-secondary spacing-right"
        :href="loginUrl"
      >
        CONNECT TO SPOTIFY
      </a>
    </div>-->
  </div>
</template>

<script>
import querystring from 'querystring'
import config from '../config'

export default {
  data() {
    return {

    }
  },
  computed: {
    loginUrl() {
      return config.server + '/login?' + querystring.stringify({
        redirect: window.location.origin + window.location.pathname
      })
    },
    authorized() {
      return this.$store.getters.authorized
    },
    userId() {
      return this.$store.state.auth.userId
    }
  },
  methods: {
    logout() {
      this.$store.dispatch('logout')
    },
    goHome() {
      this.$router.push({ name: 'home' })
    }
  }
}
</script>

<style lang="scss" scoped>
.nav {
  display: flex;
  padding: 0.5em;
  justify-content: space-between;
  align-items: center;
}
.nav-content {
  display: flex;
  flex-direction: row;
  height: 100%;
  align-items: center;
  flex-wrap: wrap;
}

.username {
  margin-right: 0.5em;
}
.user-image {
  height: 100%;
  border-radius: 50%;
}
.logo {
  display: flex;
  align-items: center;
  margin-right: 0.5em;
  flex-wrap: wrap;
  margin: 3rem auto;
}
.logo-image {
  height: 8em;
  margin-right: 0.1em;
}
</style>
