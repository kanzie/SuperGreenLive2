<!--
      Copyright (C) 2021  SuperGreenLab <towelie@supergreenlab.com>
      Author: Constantin Clauzel <constantin.clauzel@gmail.com>

      This program is free software: you can redistribute it and/or modify
      it under the terms of the GNU General Public License as published by
      the Free Software Foundation, either version 3 of the License, or
      (at your option) any later version.

      This program is distributed in the hope that it will be useful,
      but WITHOUT ANY WARRANTY; without even the implied warranty of
      MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
      GNU General Public License for more details.

      You should have received a copy of the GNU General Public License
      along with this program.  If not, see <http://www.gnu.org/licenses/>.
 -->

<template>
  <section :id='$style.container'>
    <div :id='$style.body'>
      <div :id='$style.header'>
        <h1>PLANT ON THIS <span :class='$style.green'>TIMELAPSE</span>:</h1>
        <nuxt-link to='/camera' :id='$style.change'>readjust cam</nuxt-link>
        <a :href='storage' target='_blank' :id='$style.change'>storage.zip</a>
      </div>
      <div :id='$style.plantInfos'>
        <Plant :plant='plant' />
      </div>
      <div :id='$style.capture'>
        <div :id='$style.loading'>
          <div><Loading label='Capturing pic..' /></div>
        </div>
        <div v-for='src in srcs' v-if='src' :key='src' :style='{"background-image": `url(${src})`}'></div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from 'axios'
import Loading from '~/components/loading.vue'

const RPI_URL=process.env.RPI_URL

export default {
  data() {
    return {
      n: 0,
      srcs: [null, `${RPI_URL}/capture`],
      storage: `${RPI_URL}/storage.zip`,
    }
  },
  mounted() {
    axios.post(`${RPI_URL}/motion/stop`) // in case the page was reloaded and motion never stopped
    this.interval = setInterval(() => {
      this.$data.srcs = [
        this.$data.srcs[1],
        `${RPI_URL}/capture?rand=${new Date().getTime()}`
      ]
    }, 120000)
  },
  destroyed() {
    clearInterval(this.interval)
  },
  methods: {
    nextHandler() {
      this.$router.push("/")
    },
  },
  computed: {
    plant() {
      return this.$store.state.plant.plant
    },
  },
}
</script>

<style module lang=stylus>

#container
  display: flex
  justify-content: center
  height: 100vh

#body
  display: flex
  flex-direction: column
  margin-top: 70pt
  padding: 0 20pt
  width: 100%
  max-width: 600pt
  @media only screen and (max-width: 1000pt)
    margin-top: 60pt

#header
  display: flex
  justify-content: space-between
  align-items: center

#header > h1
  margin: 20pt 0
  color: #454545
  @media only screen and (max-width: 1000pt)
    font-size: 1.2em
    margin: 10pt 0

#change
  font-weight: 600
  color: #3bb30b
  text-decoration: none

.green
  color: #3bb30b

#capture
  position: relative
  height: 100%
  margin: 30pt 0
  @media only screen and (max-width: 1000pt)
    margin: 15pt 0

#capture > div
  position: absolute
  top: 0
  right: 0
  bottom: 0
  left: 0
  background-position: center
  background-size: contain
  background-repeat: no-repeat

#loading
  display: flex
  align-items: center
  justify-content: center

#loading > div
  position: relative
  width: 100pt
  height: 100pt

#plantInfos
  @media only screen and (max-width: 1000pt)
    font-size: 0.8em

</style>
