<template>
   <div class="w-screen ml-0 flex gap-x-12  justify-round  bg-[url('/src/assets/bg2.png')]  bg-cover  bg-no-repeat 
     fixed z-10 pt-1 text-white text-2xl shadow-2xl shadow-slate-400 top-0">         
       <img src="/src/assets/logo3.png" alt="avater logo" width="60px" height="50px" class="bg-transparent  ml-5 mr-96
         ring-2 ring-slate-400 rounded-full  shadow-sm shadow-slate-500 ">
       <div class="flex gap-2 mt-2 justify-end  z-10 ">
         <div v-for="lin in links" :key="lin.id" class="text-white ">
            <RouterLink :to="`${lin.link}`" class="mx-3 text-2xl hover:italic hover:font-serif hover:text-yellow-300 hover:p-1">{{lin.home}}</RouterLink>
            
             </div>
             
       </div>
    </div>
 </template>

<script setup>
 import {RouterLink} from 'vue-router';
const links=[
   {
      home:'Home',
      link:'/'

   },
   {
      home:'Episode',
      link:'/episode'

   },
   {
      home:'Location',
      link:'/location'

   },
   {
      home:'Character',
      link:'/character'

   },
]


</script>

<style>
@media(max-width:500px)
{ 
    
}

</style>