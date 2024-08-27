<script setup>
import axios from 'axios';
import PulsLoader from 'vue-spinner/src/PulseLoader.vue'
import { ref, reactive, onMounted } from 'vue';
import { useRoute, RouterLink, useRouter } from 'vue-router';
import { useToast } from 'vue-toastification';
import NotFoundView from './NotFoundView.vue';

const route = useRoute();
const router = useRouter();
const toast = useToast();
const jobId = route.params.id

const job = ref([])
const isLoading = ref(false)

const deleteJob = async () => {
    try{
        const confirm = window.confirm(`Are you sure you want to delete this job ${jobId}`)
        if(confirm){
            await axios.delete(`/api/Jobs/${jobId}`)
            toast.success('Job deleted successfully', {
                duration: 3000})
            router.push('/jobs')
            
            }

    }catch(error){
        toast.error('Failed to delete job', {
            duration: 3000
        })
        console.error(error)
    }
}

onMounted(async () => {
    try {
        const response = await axios.get(`/api/Jobs?id=${jobId}`)
        console.log(response.data[0])
        job.value = response.data[0]
        if(response.data.length > 0)
        {isLoading.value = true}
    } catch (error) {
        console.error(error)
    }
})

</script>

<template>
    <section>
        <div class="container bg-green-100 m-auto py-6 px-6">
            <RouterLink to="/jobs" class="text-green-500 hover:text-green-600 flex items-center">
                <i class="pi pi-arrow-circle-left mr-2"></i> Back to Job Listings
            </RouterLink>
        </div>
    </section>
    <section class="bg-green-50">
        <div v-if="isLoading" class="container m-auto py-10 px-6">
            <div class="grid grid-cols-1 md:grid-cols-70/30 w-full gap-6">
                <main>
                    <div class="bg-white p-6 rounded-lg shadow-md text-center md:text-left">
                        <div class="text-gray-500 mb-4">{{ job.type }}</div>
                        <h1 class="text-3xl font-bold mb-4">{{ job.title }}</h1>
                        <div class="text-gray-500 mb-4 flex align-middle justify-center md:justify-start">
                            <i class="pi pi-map-marker text-orange-500 mr-2"></i>
                            <p class="text-orange-700">{{ job.location }}</p>
                        </div>
                    </div>

                    <div class="bg-white p-6 rounded-lg shadow-md mt-6">
                        <h3 class="text-green-800 text-lg font-bold mb-6">
                            Job Description
                        </h3>

                        <p class="mb-4">
                            {{ job.description }}
                        </p>

                        <h3 class="text-green-800 text-lg font-bold mb-2">Salary</h3>

                        <p class="mb-4">{{ job.salary }} / Year</p>
                    </div>
                </main>

                <!-- Sidebar -->
                <aside>
                    <!-- Company Info -->
                    <div v-if="job.company" class="bg-white p-6 rounded-lg shadow-md">
                        <h3 class="text-xl font-bold mb-6">Company Info</h3>

                        <h2 class="text-2xl">{{ job.company.name }}</h2>

                        <p class="my-2">
                            {{ job.company.description }}
                        </p>

                        <hr class="my-4" />

                        <h3 class="text-xl">Contact Email:</h3>

                        <p class="my-2 bg-green-100 p-2 font-bold">
                            {{ job.company.contactEmail }}
                        </p>

                        <h3 class="text-xl">Contact Phone:</h3>

                        <p class="my-2 bg-green-100 p-2 font-bold">{{ job.company.contactPhone }}</p>
                    </div>

                    <!-- Manage -->
                    <div class="bg-white p-6 rounded-lg shadow-md mt-6">
                        <h3 class="text-xl font-bold mb-6">Manage Job</h3>
                        <a href="add-job.html"
                            class="bg-green-500 hover:bg-green-600 text-white text-center font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block">Edit
                            Job</a>
                        <button
                        @click="deleteJob"
                            class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block">
                            Delete Job
                        </button>
                        
                    </div>
                </aside>
            </div>
        </div>
        <div v-else class="text-center">
            <PulsLoader />
            <NotFoundView error="400 Bad Request" msg="Sorry, the data is doesn't exists or server is shutdown" />
        </div>
    </section>
</template>