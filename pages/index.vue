<template>
    <div class="container py-20">
        <ais-instant-search-ssr>
            <div class="mb-10">
                <div class="flex justify-between items-end mb-6">
                    <h1 class="text-2xl text-gray-700 leading-tight">
                        Search
                    </h1>
                </div>
                <ais-search-box
                    placehodlder = "e.g Macbook Pro"
                />
            </div>
            <ais-state-results>
                <div slot-scope="{ query }">
                    <div v-if="query.length">
                        <ais-stats>
                            <h1 class="text-md mb-10 text-gray-700">

                            </h1>
                        </ais-stats>
                        <ais-hits>
                            <div slot="item" slot-scope="{ item }">
                                {{ item }}
                            </div>
                        </ais-hits>
                    </div>
                </div>
            </ais-state-results>
        </ais-instant-search-ssr> 
    </div>
</template>

<script>
    import {
        AisInstantSearchSsr,
        AisHits,
        AisSearchBox,
        AisStateResults,
        AisStats
    } from 'vue-instantsearch'
	export default {
        components : {
            AisInstantSearchSsr,
            AisHits,
            AisSearchBox,
            AisStateResults,
            AisStats
        },
        data () {
            return {
                instantSearchState : null
            }
        },
        provide () {
            return {
                $_ais: this.$instantsearch
            }
        },
        beforeMount () {
            this.$instantsearch.hydrate(this.instantSearchState)
        },
        asyncData({app}) {
            const instantsearch = app.$instantsearch
            return instantsearch
                .findResultsState({
                    hitsPerPage: 10 
                })
                .then (() => {
                    return {
                        instantSearchState : instantsearch.getState()
                    }
                })
        },
	}
</script>
