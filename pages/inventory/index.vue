<template>
    <div
        class="grid grid-cols-1 md:grid-cols-2 md:gap-2 md:m-2 lg:grid-cols-3 lg:m-0 mb-[64px] md:mb-[76px] lg:mb-[12px]">
        <div v-for="item in inventory" :key="item.id" class="card bg-base-100 shadow-xl rounded-none"
            @click="navigateTo(`/inventory/${item.id}`)">
            <img :src="item.image" :alt="item.name" class="object-cover h-[300px] w-full" />

            <div class="card-body">
                <h2 class="card-title">
                    {{ item.name }}
                </h2>
            </div>
        </div>

        <!-- Add new inventory item card -->
        <div key="new-item" class="card bg-base-100 shadow-xl rounded-none" @click="navigateTo('/inventory/new-item')">
            <div class="flex flex-1 justify-center items-center">
                <img src="https://cdn-icons-png.flaticon.com/512/32/32339.png" alt="New Inventory Item Image"
                    class="flex object-contain h-[300px] w-full" />
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