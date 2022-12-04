
<template>
  <div>
    <div text-gray:80>Where is the OpenAPI?</div>
    <div flex justify-center>
      <div relative border="~ rounded base" shadow font-200 m="t-5" w-200 min-w-full md:min-w-0>
        <input min-w-full ref="input" v-model="url" aria-label="Enter serverurl here.." w="80%"
          placeholder="Enter serverurl here.." type="text" autocomplete="off" p="x6 y4" bg-transparent border-none
          class="!outline-none" @keydown.enter="go" @input="go">
        <button v-if="url" absolute flex right-2 w-10 top-2 bottom-2 text-xl op90 hover:op90 hover:color-red
          aria-label="Clear search" @click="clear()">
          <span i-carbon-close ma block aria-hidden="true" />
        </button>
      </div>
    </div>

    <button font-500 m-3 text-m uppercase btn :disabled="!url" @click="go">
      Search
    </button>
    <Settings :options="options" />
    <button font-500 m-3 text-sm uppercase bg-bluegray hover-bg-yellow color-black btn @click="random">
      Show random API
    </button>
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


const url = ref('')
const show = ref(true)
const loading = ref(false)
const { data } = await useFetch(() => `https://t-huyeng.github.io/bunddev-apis/`)
let resultfuse = ref({})
let tags = ref(["server url", "name"])

let options = {
  includeScore: true,
  minMatchCharLength: 3,
  threshold: 0.4,
  keys: ['serverURLs']
}

let fuse = new Fuse(data._rawValue, options)

const clear = () => {
  url.value = ''
  show.value = false
  resultfuse.value = {}
}
const random = () => {
  try {
    url.value = data._rawValue[Math.floor(Math.random() * data._rawValue.length)].serverURLs[0]
    go()
  }
  catch {
    return
  }
}

const go = () => {
  loading.value = true
  // show.value = !show.value
  if (url.value) {
    // remove trailing whitespaces
    let searchurl = url.value.trim()
    resultfuse = fuse.search(searchurl)
    if (resultfuse.length > 0) {
      show.value = true
    } else {
      show.value = false
    }

  }
  loading.value = false

}
</script>