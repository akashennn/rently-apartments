<template>
    <div class="grid grid-cols-1 md:grid-cols-2 md:gap-2 md:m-2 lg:grid-cols-3 lg:gap-4 lg:m-4">
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
</template>

<script setup>
//  get all the apartments data
const { data: apartments } = await useFetch("http://localhost:3004/apartments");

// navigate to apartment details page
const onCardClick = async (id) => {
    await navigateTo(`/apartments/${id}`)
}
</script>
