<template>
    <div>
        <!-- add setting icon and open settings on click -->
        <div class="flex justify-end" text="xl gray4" m-5 flex justify-center gap-3>
            <a i-carbon-settings class="cursor-pointer" @click="openSettings" />
        </div>

        <!-- show options.keys if show is true -->
        <div v-if="show" m-5 flex justify-center gap-3>
            Searching for keyword in:
            <div v-for="key in options.keys" :key="key" m="r-2" p="x-2 y-1" border="~ rounded gray-200 dark:gray-700"
                outline="none active:none" text="center" bg="transparent" cursor="pointer" hover="op80">
                {{ key }}
            </div>
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
}
</script>
