<template>
    <div class="flex justify-evenly">
       <div class="bg-white px-4 py-2 w-52 rounded-lg border-slate-200 border-2 h-60 mt-12 shadow-md">
         <div class="w-full h-[30%]  justify-center items-center flex">
          <i class="fa-solid fa-xl mr-3 fa-boxes-stacked text-slate-500"></i> 
            <h1 class="text-xl text-slate-500">Category</h1>
         </div>
         <div class="w-full h-[70%]  flex justify-center items-center pt-2">
            <div class="w-20 h-20 rounded-full border-2 shadow-sm flex justify-center items-center  bg-cyan-400">
                <div class="text-xl text-white">{{ this.data.category }}</div>
            </div>
         </div>
       </div>

       <div class="bg-white px-4 py-2 w-52 rounded-lg border-slate-200 border-2 h-60 mt-12 shadow-md">
         <div class="w-full h-[30%]  justify-center items-center flex">
          <i class="fa-solid fa-xl mr-3  fa-pen-nib text-slate-500"></i> 
            <h1 class="text-xl text-slate-500">Author</h1>
         </div>
         <div class="w-full h-[70%]  flex justify-center items-center pt-2">
            <div class="w-20 h-20 rounded-full border-2 shadow-sm flex justify-center items-center  bg-cyan-400">
                <div class="text-xl text-white">{{ this.data.author }}</div>
            </div>
         </div>
       </div>
       <div class="bg-white px-4 py-2 w-52 rounded-lg border-slate-200 border-2 h-60 mt-12 shadow-md">
         <div class="w-full h-[30%]  justify-center items-center flex">
          <i class="fa-brands fa-xl mr-3 fa-product-hunt text-slate-500"></i> 
            <h1 class="text-xl text-slate-500">Book</h1>
         </div>
         <div class="w-full h-[70%]  flex justify-center items-center pt-2">
            <div class="w-20 h-20 rounded-full border-2 shadow-sm flex justify-center items-center  bg-cyan-400">
                <div class="text-xl text-white">{{ this.data.book }}</div>
            </div>
         </div>
       </div>

       <div class="bg-white px-4 py-2 w-52 rounded-lg border-slate-200 border-2 h-60 mt-12 shadow-md">
         <div class="w-full h-[30%]  justify-center items-center flex">
          <i class="fa-solid fa-xl mr-3 fa-users text-slate-500"></i> 
            <h1 class="text-xl text-slate-500">User</h1>
         </div>
         <div class="w-full h-[70%]  flex justify-center items-center pt-2">
            <div class="w-20 h-20 rounded-full border-2 shadow-sm flex justify-center items-center  bg-cyan-400">
                <div class="text-xl text-white">{{ this.data.user }}</div>
            </div>
         </div>
       </div>

    </div>
  </template>
  
  <script lang="ts">
  import ApiServices from '../../../services/ApiServices';
  
 
  export default {
    data(){
      return {
        auth:false,
        role: '',
        data : []
      }
    },
    mounted(){
          this.setAuthUser()
          this.getData()
    },
    methods:{
      getData(){
        ApiServices.get('dashboard').then((res)=>{
          this.data = res.data
        }).catch(err=>console.log(err))
      },
      setAuthUser (){
            ApiServices.get('user').then((res)=>{
              let roles = res.data.data.role
              let test = roles.filter(element => {
                 return element.name == 'admin' 
               });
               
              if (test.length == 0) {
                this.$router.push({name: 'client'})
              }
                
            }).catch((err)=>console.log(err))
             
        },
    },
  };
  </script>