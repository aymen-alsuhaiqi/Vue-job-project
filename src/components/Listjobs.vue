<script setup>
import { onMounted, ref } from 'vue';
import Joblisting from './Joblisting.vue';
import axios from 'axios';
import PulseLoder from 'vue-spinner/src/PulseLoader.vue'

defineProps({
    limit:Number,
    showButton:{
        type:Boolean,
        default: false,
    }

})

const jobs = ref([])
const isLoading = ref(false)
onMounted(async ()=>{
    try {
        const response = await axios.get('/api/Jobs')
        jobs.value = response.data
    }
    catch (error) {
        console.error(error);
    }
    finally{
        isLoading.value = true;
    }
})

</script>

<template>
    <section class="bg-green-50 px-4 py-10">
        <div class="container-xl lg:container m-auto">
            <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">
                Browse Jobs
            </h2>
            <div v-if="!isLoading" class="text-center">
                <PulseLoder/>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <Joblisting v-for="i in jobs.slice(0,limit || jobs.length)" :job="i" :key="i.id" />
            </div>
        </div>
    </section>
    <section v-if="showButton" class="m-auto max-w-lg my-10 px-6">
      <a
        href="jobs.html"
        class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700"
        >View All Jobs</a
      >
    </section>
</template>