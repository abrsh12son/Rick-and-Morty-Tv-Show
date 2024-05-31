<template>
  
  <div class="my-24 mr-5  flex justify-end">
    <input
      type="text"
      v-model="search"
      name=""
      id=""
      class="indent-2 border-2 py-1 w-48  mr-2 border-slate-300 rounded-xl text-lg text-gray-800 shadow-sm shadow-slate-300 "
      placeholder="Search for episode"
    />
    <button class="bg-yellow-300 text-white px-2 py-1 text-lg rounded-xl">search</button>
  </div>
  <div><h1 class="text-center mb-10 text-4xl p-10  -mt-10">EPISODES</h1></div>

  <div class="text-xl shadow-lg ml-24 border-gray-900 rounded-2xl">
    <div class="flex flex-wrap mx-10 gap-10 ml-36 pb-5">
      <p v-if="error">Something went wrong...</p>
      <p v-if="loading" class="pl-16 py-2 text-lg">Loading...</p>
      <RouterLink 
        :to="`/episode/ ${episode.id}`"
        v-else
        v-for="episode in filteredEpisodes"
        :key="episode.id"
        class="p-2 "
      >
        <!--Card 1-->
        <div class="w-48 h-32  overflow-hidden  shadow-xl shadow-slate-600 text-center border-1 ring-4 ring-offset-8 ring-gray-400
        rounded-2xl border-solid border-gray-400  ">
           <div class="px-2 pt-1 font-bold">
            <div class="text-xl p-2 hover:underline hover:text-gray-950">{{ episode.name }}</div>
            <div class="text-xl p-2 hover:underline hover:text-gray-950">{{ episode.episode }}</div>

          </div>
        </div>
      </RouterLink>
       
    </div>
    <div class="flex justify-around py-6  ">
      <button @click="decrementCount" class="bg-gray-900 hover:bg-gray-800  text-white font-bold rounded-full w-14 h-14 p-1 text-center">
        
         {{ pre }}
      </button>
      <button
        @click="`${page.value--}`"
        class="bg-gray-900 hover:bg-gray-800  text-white font-bold rounded-full w-14 h-14 p-1 text-center"      >
        C-{{ page }}
      </button>
      <button
        type="button"
        @click="incrementCount"
        class="bg-gray-900 hover:bg-gray-800  text-white font-bold rounded-full w-14 h-14 p-1 text-center"      >
      {{ next }} >
       
      </button>
    </div>
  
  </div>
</template>

<script>
import { ref } from 'vue'
import gql from 'graphql-tag'
import { useQuery } from '@vue/apollo-composable'
import { RouterLink } from 'vue-router'

const CHARACTERS_QUERY = gql`
  query Characters($page: Int!) {
    episodes(page: $page) {
      results {
        id
        name
        episode
      }
    }
  }
`
export default {
  name: 'episode',
  setup() {
    const  search=ref('')
    const page = ref(1)
    // Define page variable in setup
    const pre = ref(0)
 
    const next = ref(2)
    const incrementCount = () => {
      if (page.value == 3) return
     
      page.value++
      pre.value++
      next.value++
    }
    const decrementCount = () => {
      if (page.value == 1) return
      

      page.value--
      pre.value--
      next.value--
    }
    const { result, loading, error } = useQuery(CHARACTERS_QUERY, () => ({
      page: page.value // Pass page value to the query
    }))

    
    return {
      result,
      loading,
      error,
      incrementCount,
      decrementCount,
      page,
      pre,
      next,
      search,
 
    }
  },
  computed: {
        filteredEpisodes() {
          return this.result.episodes.results.filter((eps) => {
            return eps.name.toLowerCase().includes(this.search.toLowerCase())
          })
        }
      }

}
</script>
