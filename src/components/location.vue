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
    <button class="bg-yellow-300 text-white px-2 py-1  text-lg rounded-2xl">search</button>
  </div>
  <div><h1 class="ml-10 text-4xl mb-10 text-center">LOCATIONS</h1></div>

  <div class=" rounded-2xl shadow-xl shadow-slate-300 ml-24  pt-10">
    <div class="flex flex-wrap mx-10 gap-10 ml-36 pb-5">

      <p v-if="error">Something went wrong...</p>

      <p v-if="loading" class="pl-16 py-2 text-lg">Loading...</p>
      <router-link 
        :to="`/location/ ${location.id}`"
        v-else
        v-for="location in filteredLOcations"
        :key="location.id"
        class="p-2 " >
        <!--Card 1-->
        <div class="w-48 h-32  overflow-hidden  shadow-xl shadow-slate-600 text-center border-1 ring-4 ring-offset-8 ring-blue-200
        rounded-2xl border-solid border-gray-400  ">
           <div class="px-2 pt-1 ">
            <div class=" text-xl p-2 hover:underline hover:text-gray-950">{{ location.name }}</div>
            <div class=" text-blue-600 text-xl pl-2 hover:underline hover:text-gray-950">{{ location.type }}</div>
          </div>
        </div>
      </router-link>

       
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
        class="bg-blue-900 hover:bg-blue-500  text-white font-bold rounded-full w-14 h-14 p-1 text-center"      >
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
    locations(page: $page) {
      results {
        id
        name
         type
         dimension
      }
    }
  }
`
export default {
  name: 'location',
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
    filteredLOcations() {
          return this.result.locations.results.filter((eps) => {
            return eps.name.toLowerCase().includes(this.search.toLowerCase())
          })
        }
      }

}
</script>
