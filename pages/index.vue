<template>
    <!-- navigation bar: desktop -->
    <div class="navbar bg-base-100 hidden lg:flex">
        <div class="flex-1">
            <a class="font-bold text-xl" @click="navigateTo('/')">Rently.</a>
        </div>

        <div class="flex-none">
            <ul class="menu menu-horizontal px-1">
                <li class="mr-2" @click="navigateTo('/')">
                    <a :class="{ active: $route.path === '/' }">Apartments</a>
                </li>

                <li @click="navigateTo('/inventory')">
                    <a :class="{ active: $route.path === '/inventory' }">Inventory</a>
                </li>
            </ul>
        </div>
    </div>

    <!-- apartments -->
    <div
        class="grid grid-cols-1 md:grid-cols-2 md:gap-2 md:m-2 lg:grid-cols-3 lg:m-0 mb-[64px] md:mb-[76px] lg:mb-[12px]">
        <div v-for="(apartment, index) in apartments" :key="apartment.id"
            class="card bg-base-100 shadow-xl rounded-none" @click="onCardClick(apartment.id)">
            <figure class="rounded-none">
                <img :src="apartment.image" :alt="apartment.title" class="object-cover h-[300px] w-full" />
            </figure>

            <div class="card-body">
                <h2 class="card-title">
                    {{ apartment.title }}

                    <div v-if="index === 0" class="badge badge-secondary">NEW</div>
                </h2>

                <p class="mb-2">{{ apartment.address }}</p>

                <div class="card-actions justify-end">
                    <div class="badge badge-outline">Floor {{ apartment.floor }}</div>
                    <div class="badge badge-outline">Unit {{ apartment.doorNumber }}</div>
                </div>
            </div>
        </div>
    </div>

    <!-- navigation bar: mobile and tablet -->
    <div class="btm-nav lg:hidden">
        <button :class="{ active: $route.path === '/' }" @click="navigateTo('/')">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"
                xmlns="http://www.w3.org/2000/svg">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6">
                </path>
            </svg>
        </button>

        <button :class="{ active: $route.path === '/inventory' }" @click="navigateTo('/inventory')">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"
                xmlns="http://www.w3.org/2000/svg">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                    d="M4 7v10c0 2.21 3.582 4 8 4s8-1.79 8-4V7M4 7c0 2.21 3.582 4 8 4s8-1.79 8-4M4 7c0-2.21 3.582-4 8-4s8 1.79 8 4m0 5c0 2.21-3.582 4-8 4s-8-1.79-8-4">
                </path>
            </svg>
        </button>
    </div>
</template>

<script setup>
//  get all the apartments data
const { data: apartments } = await useFetch("http://localhost:3004/apartments");

// navigate to apartment details page
const onCardClick = async (id) => {
    await navigateTo(`/apartments/${id}`)
}
</script>
