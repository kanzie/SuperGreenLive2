<!--
      Copyright (C) 2021  SuperGreenLab <towelie@supergreenlab.com>
      Author: Constantin Clauzel <constantin.clauzel@gmail.com>

      This program is free software: you can redistribute it and/or modify
      it under the terms of the GNU General Public License as published by
      the Free Software Foundation, either version 3 of the License, or
      (at your option) any later version.

      This program is distributed in the hope that it will be useful,
      but WITHOUT ANY WARRANTY without even the implied warranty of
      MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
      GNU General Public License for more details.

      You should have received a copy of the GNU General Public License
      along with this program.  If not, see <http://www.gnu.org/licenses/>.
 -->

<template>
  <section :id="$style.container">
    <form @submit='loginHandler'>
      <div :id='$style.body'>
        <div :id='$style.title'>S<span :id='$style.green'>G</span>L LOGIN</div>
        <input type='text' placeholder='Login' v-model='login' @change=''/>
        <input type='password' placeholder='Password' v-model='password' />
        <span :id='$style.error' v-if='error'>Wrong login/password</span>
        <div :id='$style.button'>
          <button @click='loginHandler'>LOGIN</button>
        </div>
      </div>
    </form>
  </section>
</template>

<script>
export default {
  data() {
    return {
      login: '',
      password: '',
    }
  },
  watch: {
    loggedIn(val) {
      if (this.$store.state.plant.plant == null) {
        this.$router.replace('/plant')
      } else {
        this.$router.replace('/')
      }
    },
  },
  methods: {
    loginHandler(e) {
      e.preventDefault()
      e.stopPropagation()
      const { login, password } = this.$data
      this.$store.dispatch('auth/login', { login, password })
      return false
    },
  },
  computed: {
    loggedIn() {
      return this.$store.getters['auth/loggedIn']
    },
    error() {
      return this.$store.getters['auth/error']
    },
  },
}
</script>

<style module lang=stylus>
#container
  display: flex
  height: 100vh
  justify-content: center
  align-items: center

#body
  display: flex
  flex-direction: column

#body > input
  margin: 3pt 0
  padding: 3pt 6pt

#green
  color: #3bb30b

#title
  color: #454545
  font-weight: bold

#button
  display: flex
  justify-content: flex-end
  align-items: flex-end
  padding: 15pt 0 0 0

#button > button
  border: none
  color: white
  border-radius: 2.5px
  background-color: #3bb30b
  padding: 2pt 20pt
  cursor: pointer

#button > button:hover
  background-color: #4bc31b

#button > button:active
  background-color: #2ba300

#error
  color: red

</style>
