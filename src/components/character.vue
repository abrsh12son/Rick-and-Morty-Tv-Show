<template>
  <div class="my-24 mr-5 flex justify-end">
    <input
      type="text"
      v-model="search"
      name=""
      id=""
      class=" indent-2 border-2 py-1 w-48  mr-2 border-slate-300 rounded-xl text-lg text-gray-800 shadow-sm shadow-slate-300 "    
        placeholder="Search for location"
    />
    <button class="bg-yellow-300 text-white px-2 py-1 text-lg rounded-xl">search</button>

  </div>
  <div>
    <div><h1 class=" mb-10 text-4xl p-3 text-center">CHARACTERS</h1></div>

    
  <div class="grid mx-64 sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-x-32 shadow-2xl shadow-slate-300  ">
    <div  v-for="char in filteredCharacters" :key="char.id" >
       <router-link 
      :to="`/character/${char.id}` "

      class="max-w-sm  overflow-hidden ml-64">
    <div class=" container w-48 mb-10 mx-auto text-wrap shadow-2xl shad shadow-slate-300 ring-4 ring-slate-300  items-center rounded-2xl">
        <img class="w-48 h-32 object-cover rounded-2xl" :src="char.image" alt="Mountain">
        <h1 class=" text-wrap text-center text-red-500 font-bold text-3xl mb-2 hover:text-blue-500 hover:underline" > {{char.name}}</h1>
     </div>
       
    </router-link>
    
    </div>
  </div>  
  
  <div class="flex justify-around py-6  ">
    <button @click="decrementCount" class="bg-blue-900 hover:bg-blue-500  text-white font-bold rounded-full w-14 h-14 p-1 text-center">
      
       {{ pre }}
    </button>
    <button
       
      class="bg-blue-900 hover:bg-blue-500   text-gray-400 font-bold  w-24 rounded-lg h-14 p-1 text-center"      >
       page-{{ page }}
    </button>
    <button
      type="button"
      @click="incrementCount"
      class="bg-blue-900 hover:bg-blue-500  text-white font-bold rounded-full w-14 h-14 text-center text-4xl  pb-3"      >
    >
     
    </button>
  </div>

  
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import gql from 'graphql-tag'
import { useQuery } from '@vue/apollo-composable'
const CHARACTERS_QUERY = gql`
  query Characters($page: Int!) {
    characters(page: $page) {
      results {
        id
        image
        name
      }
    }
  }
`

export default {
  name: 'character',
  setup() {
    const search = ref('')
    const page = ref(1)
    const pre = ref(0)
    const next = ref(2)

    const incrementCount = () => {
      if (page.value === 42) return
      page.value++
      pre.value++
      next.value++
    }

    const decrementCount = () => {
      if (page.value === 1) return
      page.value--
      pre.value--
      next.value--
    }

    const { result, loading, error } = useQuery(CHARACTERS_QUERY, () => ({
      page: page.value // Pass page value to the query
    }))

    const filteredCharacters = computed(() => {
      return result.value?.characters?.results.filter((eps) => {
        return eps.name.toLowerCase().includes(search.value.toLowerCase())
      }) || []
    })

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
      filteredCharacters
    }
  }
}
</script>
