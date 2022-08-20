<template>
  <div class="main">
    <!-- search box component -->
    <AnimeSearchBox :search="searchKey" :matchResult="matchedResult" @changeSearch="handleSearch" />
    <!-- anime list component -->
    <AnimeList v-if="!isLoaderOpen" :animeList="animeList" :page="page" @changePage="handlePageChange" />
    <!-- loader -->
    <div class="loader" v-if="isLoaderOpen"></div>
  </div>
</template>

<script>
import { onMounted, ref } from 'vue';
import axios from 'axios'

import AnimeSearchBox from '@/components/AnimeSearchBox.vue'
import AnimeList from '@/components/AnimeList.vue'

export default {
  name: 'AnimeSearch',

  components: {
    AnimeSearchBox,
    AnimeList
  },

  setup() {
    // ref vars
    const animeList = ref([])
    const page = ref(1)
    const limit = ref(15)
    const searchKey = ref('')
    const matchedResult = ref(0)
    const isLoaderOpen = ref(false)

    /**
     * Mounted Hook
     * *************************************
     */
    onMounted(() => fetchAnimes())

    /**
     * Methods
     * *************************************
     */

    // fetch animes
    const fetchAnimes = () => {
      // start the loader
      isLoaderOpen.value = true

      axios.get('https://api.jikan.moe/v4/characters', {
        params: {
          page: page.value,
          limit: limit.value,
          q: searchKey.value,
          order_by: 'favorite',
          sort: 'desc'
        }
      }).then(res => {
        // set the anime list
        animeList.value = res.data.data
        // set the matching result value
        matchedResult.value = res.data.pagination.items.total
      }).catch(err => {
        alert('Something went wrong!')
      }).finally(() => {
        // off the loader
        isLoaderOpen.value = false
      })
    }

    // handle anime search
    const handleSearch = (newSearch) => {
      searchKey.value = newSearch
      fetchAnimes()
    }

    // handle page search
    const handlePageChange = (newPage) => {
      page.value = newPage
      fetchAnimes()
    }

    return {
      // refs
      isLoaderOpen,
      searchKey,
      matchedResult,
      animeList,
      page,

      // methods
      handleSearch,
      handlePageChange
    }
  }
}

</script>

<style lang="scss" scoped>
.main {
  .loader {
    width: 60px;
    height: 60px;
    border: 5px solid #f3f3f3;
    border-top: 5px solid #333;
    border-radius: 50%;
    background: rgba($color: #000000, $alpha: 0.5);
    animation: loaderanimate 1s linear infinite;

    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 999;
  }

  @keyframes loaderanimate {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
}
</style>