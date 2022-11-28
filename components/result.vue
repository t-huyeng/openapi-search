<template>
    <div flex justify-center>

        <div border="~ rounded gray-200 dark:gray-700" outline="none active:none" m="b-4" justify-center w-screen-sm
            hover-bg-blend-overlay bg-opacity-80
            v-bind:class="getScore == 'Match' ? 'bg-green-900 hover-bg-green-900' : 'hover-bg-gray-800'">
            <!-- show fuse score of result -->
            <div text-s text-right m-r-2 v-bind:class="getScore == 'Match' ? 'text-green:80' : 'text-gray:50'">
                {{ getScore }}
            </div>
            <!-- show server urls -->
            <div text="xs gray4">
                Server URL<template v-if="result.item.serverURLs.length > 1">s</template>:
            </div>
            <template v-for="server in result.item.serverURLs">
                <div text="xs gray4" m-1 flex justify-center gap-3>
                    <template v-if="result.item.serverURLs.length > 1">-</template>{{ server }}
                </div>
            </template>
            <div class="result__title">
                <h5 text-xs> {{ result.item.office }}</h5>
                <h2 class="text-2xl font-bold">{{ result.item.name }}</h2>
            </div>
            <div text="xl gray4" m-5 flex justify-center gap-3>
                <a i-carbon-logo-github :href="result.item.githubURL" target="_blank" />
                <a i-carbon-book :href="result.item.documentationURL" target="_blank" />
                <a i-carbon-api-1 :href="result.item.rawOpenAPI" target="_blank" />

            </div>
            <div class="result__description">
                <p class="text-sm text-gray:90" font-italic>{{ result.item.description }}</p>
            </div>
        </div>
    </div>

</template>

<script setup lang="ts">
export interface Props {
    result: {
        item: {
            name: string;
            office: string;
            description: string;
            documentationURL: string;
            githubURL: string;
            rawOpenAPI: string;
            pypiURL: string;
            serverURLs: [];
        },
        score: number;
        refIndex: number;
    }
}
const props = defineProps<Props>()
const color = ref('green-200')
const getScore = computed(() => {
    // if score is smaller 0.01 show "Match"
    if (props.result.score < 0.005) {
        return "Match"
    }
    // if score is smaller 0.1 show "Good Match"
    if (props.result.score < 0.1) {
        return "Good Match"
    }
    else {
        return "Similar URL"
    }
})


</script>

<style scoped>
.result__title {
    padding: 1rem;
}

.result__description {
    padding: 1rem;
}
</style>