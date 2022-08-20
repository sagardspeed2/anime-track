<template>
    <!-- anime info -->
    <div class="anime__item">
        <div class="anime__info">
            <!-- anime image -->
            <div class="anime__image">
                <img :src="animeImage" :alt="animeInfo.name">
            </div>

            <!-- anime details -->
            <div class="anime__detail">
                <div class="detail__header">
                    <!-- anime name -->
                    <div class="anime__name">
                        {{animeInfo.name}}
                    </div>

                    <!-- anime likes -->
                    <div class="anime__likes">
                        <span class="hearts">&hearts;</span> {{animeInfo.favorites}}
                    </div>
                </div>

                <!-- anime nicknames -->
                <div class="anime__tags">
                    <div class="anime__tag" v-for="(tag, tag_i) in animeInfo.nicknames" :key="tag_i">
                        {{tag}}
                    </div>
                </div>
            </div>
        </div>

        <!-- anime link -->
        <div class="anime__link">
            <a :href="animeInfo.url" target="_blank">&#x21d2;</a>
        </div>
    </div>
</template>

<script>
import { computed, ref, watch } from 'vue';

export default {
    name: "animeItem",

    // props
    props: {
        anime: {
            type: Object,
            default: null
        }
    },

    setup(props) {
        // ref vars
        const animeInfo = ref(props.anime)
        
        /**
         * Computed props
         * *************************************
         */

        // get anime image
        const animeImage = computed(() => {
            return animeInfo.value.images.webp.image_url
        })

        /**
         * watch hooks
         * *************************************
         */
        watch(() => props.anime, () => {
            animeInfo.value = props.anime
        })

        return {
            // refs
            animeInfo, 
            
            // computed
            animeImage
        }
    }
}
</script>

<style lang="scss" scoped>
.anime {
    &__item {
        width: 100%;
        display: flex;
        border: 1px solid rgba($color: #000000, $alpha: 0.5);
        align-items: center;
        padding: 10px;
        border-radius: 10px;
        margin: 10px 0;

        .anime {
            &__info {
                flex: 1;
                display: flex;
                border-right: 2px solid rgba($color: #000000, $alpha: 0.5);

                .anime__image {
                    img {
                        width: 50px;
                        height: 60px;
                        border-radius: 10px;
                    }
                }

                .anime__detail {
                    width: 100%;
                    display: flex;
                    justify-content: space-around;
                    flex-direction: column;
                    margin: 0 10px;

                    .detail__header {
                        display: flex;

                        .anime__name {
                            flex: 1;
                        }

                        .hearts {
                            color: red
                        }
                    }

                    .anime__tags {
                        display: flex;

                        .anime__tag {
                            background: rgba($color: #000000, $alpha: 1.0);
                            color: #fff;
                            margin-right: 5px;
                            padding: 5px;
                            border-radius: 5px;
                        }
                    }
                }
            }

            &__link {
                padding-left: 10px;

                a {
                    text-decoration: none;
                    color: #000;
                    font-size: 26px;
                    font-weight: bolder;
                }
            }
        }
    }
}
</style>