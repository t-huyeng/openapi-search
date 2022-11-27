
<template>
  <div>
    <div text-gray:80>Where is the OpenAPI?</div>
    <div flex justify-center>
      <div relative border="~ rounded base" shadow font-200 m="t-5" w-200 min-w-full md:min-w-0>
        <input min-w-full ref="input" v-model="url" aria-label="Enter URL or keyword here.." w="80%"
          placeholder="Enter URL or keyword here.." type="text" autocomplete="off" p="x6 y4" bg-transparent border-none
          class="!outline-none" @keydown.enter="go" @input="go">
        <button v-if="url" absolute flex right-2 w-10 top-2 bottom-2 text-xl op30 hover:op90 aria-label="Clear search"
          @click="clear()">
          <span i-carbon-close ma block aria-hidden="true" />
        </button>
      </div>
    </div>

    <button font-500 m-3 text-sm uppercase btn :disabled="!url" @click="go">
      Search
    </button>
    <Settings :options="options" />
  </div>
  <div>
    <div v-if="loading">
      <!-- TODO loading animation  -->
    </div>
    <!-- show the result of the search -->
    <div v-else-if="show">
      <!-- show Result componete for all resulst in resultfuse -->
      <Result v-for="(item) in resultfuse" :result="item"></Result>

    </div>
    <div v-else>
      No OpenAPI found
    </div>

  </div>
</template>


<script setup lang="ts">
import Fuse from 'fuse.js'


const url = ref('https://verkehr.autobahn.de/o/autobahn')
const show = ref(true)
const loading = ref(false)
const { data } = await useFetch(() => `https://t-huyeng.github.io/bunddev-apis/`)
let resultfuse = ref({})
let tags = ref(["server url", "name"])

const options = {
  includeScore: true,
  minMatchCharLength: 3,
  threshold: 0.4,
  keys: ['serverURLs']
}

const fuse = new Fuse(data._rawValue, options)

const clear = () => {
  url.value = ''
  show.value = false
  resultfuse.value = {}
}

const go = () => {
  loading.value = true
  // show.value = !show.value
  if (url.value) {

    resultfuse = fuse.search(url.value)
    console.log(resultfuse)
    if (resultfuse.length > 0) {
      show.value = true
    } else {
      show.value = false
    }

  }
  loading.value = false

}
</script>