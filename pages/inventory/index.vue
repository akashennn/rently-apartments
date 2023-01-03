<template>
    <div
        class="grid grid-cols-1 md:grid-cols-2 md:gap-2 md:m-2 lg:grid-cols-3 lg:m-0 mb-[64px] md:mb-[76px] lg:mb-[12px]">
        <div v-for="item in inventory" :key="item.id" class="card bg-base-100 shadow-xl rounded-none"
            @click="navigateTo(`/apartments/${item.id}`)">
            <img :src="item.image" :alt="item.name" class="object-cover h-[300px] w-full" />

            <div class="card-body">
                <h2 class="card-title">
                    {{ item.name }}
                </h2>
            </div>
        </div>
    </div>
</template>

<script setup>
// set layout
definePageMeta({
    layout: "custom",
})

//  get all the inventory data
const { data: inventory } = await useFetch("http://localhost:3004/inventory");
</script>