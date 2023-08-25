<script setup lang="ts">
import { useQuery } from '@tanstack/vue-query';
import { nextTick, onMounted, ref } from 'vue';

const id = ref(0);

const {data, refetch} = useQuery({
  queryKey: ['multiply', id] as const,
  queryFn: async ({queryKey}) => {
    console.log("Current value of ref:", id.value);
    console.log("Current value of query key:", queryKey[1]); // Will always be one step behind
    
    return Promise.resolve(id.value * 2)
  },
  enabled: false
});
onMounted(() => refetch());

async function increment() {
  console.log('Increment clicked');
  id.value++;
  // await nextTick(); // Awaiting the next tick before refetching fixes the issue
  refetch();
}
</script>

<template>
  <div>Id times two: {{ data }}</div>
  <button @click="increment">Increment id</button>
</template>

