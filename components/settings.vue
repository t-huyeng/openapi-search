<template>
    <div>
        <!-- add setting icon and open settings on click -->
        <div class="flex justify-end" text="xl gray4" m-5 flex justify-center gap-3>
            <a i-carbon-settings class="cursor-pointer" @click="openSettings" />
            <a i-carbon-help class="cursor-pointer" @click="openHelp" />
        </div>

        <!-- show options.keys if show is true -->
        <div v-if="show" m-5 flex justify-center gap-3>
            Searching for keyword in:
            <div v-for="key in options.keys" :key="key" m="r-2" p="x-2 y-1" border="~ rounded gray-200 dark:gray-700"
                outline="none active:none" text="center" bg="transparent" cursor="pointer" hover="op80">
                {{ key }}
            </div>
        </div>
        <div v-if="showHelp" m-5 justify-center font-oblique>
            Have you just found an API and are not sure if it is already documented? <br />
            Just paste the server URL of the API into the input field and see if a result appears.
            If there are no result go ahead and create a new issue on the <a color-blue:400
                href="https://github.com/bundesAPI/sofortmassnahmen/issues" hover-bg-gray7>GitHub repository</a>.
        </div>

    </div>


</template>
<script setup lang="ts">
export interface Props {
    options: {
        includeScore: boolean,
        minMatchCharLength: number,
        threshold: number,
        keys: [],
    }
}
defineProps<Props>()
const show = ref(false)

const openSettings = () => {
    show.value = !show.value
    if (show.value) {
        showHelp.value = false
    }
}

const showHelp = ref(false)

const openHelp = () => {
    showHelp.value = !showHelp.value
    if (showHelp.value) {
        show.value = false
    }
}
</script>
