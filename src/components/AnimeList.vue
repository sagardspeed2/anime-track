<template>
    <!-- anime list -->
    <div class="anime__list" v-if="animeList.length">
        <!-- anime details -->
        <div class="anime__items">
            <AnimeItem v-for="(anime, index) in animeList" :key="index" :anime="anime"/>
        </div>

        <div class="action_btns">
            <!-- prev btn -->
            <button v-if="isPrevBtnShow" @click="movePage('prev')">
                Previous
            </button>

            <!-- next btn -->
            <button @click="movePage('next')">
                Next
            </button>
        </div>
    </div>

    <!-- no result message -->
    <div class="anime__list no__result" v-else>
        No results found
    </div>
</template>

<script>
import { computed } from 'vue'

import AnimeItem from '@/components/AnimeItem.vue'

export default {
    name: 'animeList',

    // props
    props: {
        animeList: {
            type: Array,
            default: []
        },
        page: {
            type: Number,
            default: 0
        }
    },
    
    components: {
        AnimeItem,
    },

    setup(props, {emit}) {
        /**
         * Computed props
         * *************************************
         */

        // check prev btn is visible
        const isPrevBtnShow = computed(() => {
            return props.page > 1
        })

        /**
         * Methods
         * *************************************
         */

        // change page
        const movePage = (type) => {
            const newPage = type === 'next' ? props.page + 1 : props.page - 1

            emit("changePage", newPage)
        }

        return {
            // computed
            isPrevBtnShow,

            // methods
            movePage
        }
    }
}
</script>

<style lang="scss" scoped>
.anime__list {
    margin-bottom: 5%;
    & .anime__items {
        display: flex;
        width: 80%;
        margin: auto;
        flex-direction: column;
    }

    .action_btns {
        text-align: center;
        margin-top: 15px;

        button {
            padding: 10px 15px;
            background: #000;
            color: #fff;
            border: none;
            outline: none;
            border-radius: 10px;
            margin: 0 5px;
            cursor: pointer;
        }
    }

    &.no__result {
        display: flex;
        justify-content: center;
        margin-top: 5%;
    }
}
</style>