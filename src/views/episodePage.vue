<template>
    <div>
       <div class=" text-center p=5 bg-white mt-24 justify-center rounded-lg p-2">
            <p class=" p-3 text-3xl text-center text-red-400 font-bold">
                {{ result.episode.name }}
            </p>
            <p class="text-2xl  text-blue-950 font-bold">
               episode : {{ result.episode.episode }}
           </p>
           <p class="text-2xl  text-blue-900 font-bold">
               air_date :{{ result.episode.air_date }}
           </p>
           <p class="text-2xl  text-blue-950 font-bold">
               created : {{ result.episode.created }}
           </p>
       </div>
        <RouterLink to="/character" class="text-3xl font-bold text-blue-400 ml-12">
           characters <small class="text-gray-600 text-sm ">(in {{ result.episode.name }})</small>
         </RouterLink>
         <div class="grid sm:grid-cols-1 md:grid-cols-2 lg:grid-cols-3 justify-center">
           <div  v-for="c in result.episode.characters" :key="c.id" class="p-10">  
              <router-link 
             :to="`/character/${c.id}` "

             class="max-w-sm  overflow-hidden shadow-lg  rounded-2xl ">
               <div class="relative">
                 <div 
                 :class="{
                   'bg-green-500': c.status === 'Alive',
                   'bg-red-500': c.status === 'Dead',
                   'bg-blue-700': c.status === 'unknown'
 
                 }"
                 class="inline-block  rounded-lg px-3 py-0 text-lg font-bold  mr-2 mb-2 absolute right-2 top-2 text-white">{{ c.status}}</div>
 
               <img class="w-full h-64 object-cover " :src="c.image" alt="Mountain">
               </div>
               <div class="px-6 py-4">
                 <router-link
                 :to="`/character/${c.id}` "
                 class="font-bold text-xl mb-2 hover:text-blue-500 hover:underline"> {{c.name}}</router-link>
                  
               </div>
 
               <div class="px-6 pt-4 pb-2">
                 <span class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">{{ c.status }}</span>
 
                 <span class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">{{ c.species }}</span>
                 <span class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">{{ c.gender }}</span>
               </div>
           </router-link>
           </div>
         </div>  
         <p class="text-blue-900 text-center font-extrabold text-2xl  p-3"> Project Links</p> 
         <div class="flex justify-center gap-10 mb-5 ">
       
          <a class="text-blue-700 hover:text-blue-400"
           href="https://github.com/abrsh12son">
            <svg xmlns="http://www.w3.org/2000/svg" width="40" height="40" viewBox="0 0 24 24" style="fill: blue;transform:"><path fill-rule="evenodd" clip-rule="evenodd" d="M12.026 2c-5.509 0-9.974 4.465-9.974 9.974 0 4.406 2.857 8.145 6.821 9.465.499.09.679-.217.679-.481 0-.237-.008-.865-.011-1.696-2.775.602-3.361-1.338-3.361-1.338-.452-1.152-1.107-1.459-1.107-1.459-.905-.619.069-.605.069-.605 1.002.07 1.527 1.028 1.527 1.028.89 1.524 2.336 1.084 2.902.829.091-.645.351-1.085.635-1.334-2.214-.251-4.542-1.107-4.542-4.93 0-1.087.389-1.979 1.024-2.675-.101-.253-.446-1.268.099-2.64 0 0 .837-.269 2.742 1.021a9.582 9.582 0 0 1 2.496-.336 9.554 9.554 0 0 1 2.496.336c1.906-1.291 2.742-1.021 2.742-1.021.545 1.372.203 2.387.099 2.64.64.696 1.024 1.587 1.024 2.675 0 3.833-2.33 4.675-4.552 4.922.355.308.675.916.675 1.846 0 1.334-.012 2.41-.012 2.737 0 .267.178.577.687.479C19.146 20.115 22 16.379 22 11.974 22 6.465 17.535 2 12.026 2z"></path></svg></a>
          <a class="text-blue-700  hover:text-red-500" 
          href="https://www.figma.com/design/dTu2rhPMkBKue4NhgTcxnm/Rick_Morty-Tv-Show?node-id=0-1&t=IcoHCrepAHPXslfx-0"><svg xmlns="http://www.w3.org/2000/svg"  width="40" height="40" viewBox="0 0 24 24" style="fill: red;transform:"><path d="M15.332 8.668a3.333 3.333 0 0 0 0-6.663H8.668a3.333 3.333 0 0 0 0 6.663 3.333 3.333 0 0 0 0 6.665 3.333 3.333 0 0 0 0 6.664A3.334 3.334 0 0 0 12 18.664V8.668h3.332z"></path><circle cx="15.332" cy="12" r="3.332"></circle></svg></a>
        </div>
    </div>
   <RouterView/>
</template>
   <script setup>
   import { defineProps } from 'vue';
   import gql from 'graphql-tag'
   import { useQuery } from '@vue/apollo-composable'
   import { RouterLink } from 'vue-router';
   
   const props = defineProps(['id']);
   
   const Episode_QUERY = gql`
     query GetLocation($id: ID!) {
       episode(id: $id) {
         id
         name  
         air_date
         created
         episode
         characters
         {
            id
            name
            status
            species
            gender
            image
         }
         
        
         
       }
     }
   `;
   
   const { result, loading, error } = useQuery(Episode_QUERY, { id: props.id });
   </script>