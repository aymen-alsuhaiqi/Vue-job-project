<script setup>
import hero from "@/components/Hero.vue";
import homecards from '@/components/Homecards.vue'
import Listjobs from '@/components/Listjobs.vue';
</script>

<template>
    
  <hero title="Welcome Home"/>
  <homecards/>
  <Listjobs :limit="3" :show-button="true"/>
  <h1 class="text-2xl">aymen</h1>
</template>