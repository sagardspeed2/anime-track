<template>
    <div class="search__box">
        <!-- search box title -->
        <div class="search__title">
            <h1>Search Anime Characters</h1>
        </div>

        <!-- search input box -->
        <div class="search__input">
            <input type="text" placeholder="Search anime ..." v-on="eListeners" />
        </div>

        <!-- total matched results message -->
        <div class="search__result__count">
            <p>
                Total <b>{{matchResult}}</b> matching anime characters found
            </p>
        </div>
    </div>
</template>

<script>
import _ from 'underscore'
import { computed } from 'vue'

export default {
    name: 'animeSearchBox',

    // props
    props: {
        matchResult: {
            type: Number,
            default: 0
        },

        search: {
            type: String,
            default: ''
        }
    },

    setup(props, {emit}) {
        /**
         * Computed props
         * *************************************
         */

        // input box listeners
        const eListeners = computed(() => {
            return {
                input: _.debounce((e) => {
                    emit('changeSearch', e.target.value)
                }, 1000)
            }
        })

        return {
            // computed
            eListeners
        }
    },
}
</script>

<style lang="scss" scoped>
.search__box {
    display: flex;
    justify-content: center;
    padding: 20px 0;
    flex-direction: column;
    align-items: center;
    gap: 20px;
    border-bottom: 2px solid rgba($color: #000000, $alpha: 0.5);

    .search__input {
        width: 50%;
        input {
            width: 100%;
            padding: 15px 20px;
            border-radius: 25px;
            border-color: rgba($color: #000000, $alpha: 0.5);
        }
    }
}
</style>