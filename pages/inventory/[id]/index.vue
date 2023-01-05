<template>
    <div class="flex flex-1 justify-center bg-[#f7f7fa]">
        <div class="w-[1024px] h-screen bg-white">
            <button class="btn btn-circle btn-outline absolute top-3 left-3 bg-white" @click="onBack">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                    stroke="currentColor" class="w-6 h-6">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M9 15L3 9m0 0l6-6M3 9h12a6 6 0 010 12h-3" />
                </svg>
            </button>

            <div class="flex flex-1 justify-center">
                <img class="h-[300px] md:h-full lg:h-[500px] object-cover" :src="inventoryItem.image || defaultImageURL"
                    :alt="`${inventoryItem.name} Image` || 'Default Image'" />
            </div>

            <div class="p-3 flex flex-1 flex-col">
                <div>
                    <label class="label">
                        <span class="label-text">Name</span>
                    </label>

                    <input type="text" placeholder="Enter name" v-model="inventoryItem.name"
                        class="input input-sm input-bordered w-full" :disabled="isReadyOnly" />
                </div>

                <div>
                    <label class="label">
                        <span class="label-text">Image URL</span>
                    </label>

                    <input type="text" placeholder="Enter name" v-model="inventoryItem.image"
                        class="input input-sm input-bordered w-full" :disabled="isReadyOnly" />
                </div>

                <div v-if="isReadyOnly" class="flex flex-1 flex-col mt-3 justify-center">
                    <button class="btn btn-sm btn-primary flex-1 mb-2" @click="onEdit">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                            stroke="currentColor" class="w-4 h-4 mr-2">
                            <path stroke-linecap="round" stroke-linejoin="round"
                                d="M16.862 4.487l1.687-1.688a1.875 1.875 0 112.652 2.652L10.582 16.07a4.5 4.5 0 01-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 011.13-1.897l8.932-8.931zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0115.75 21H5.25A2.25 2.25 0 013 18.75V8.25A2.25 2.25 0 015.25 6H10" />
                        </svg>
                        Edit
                    </button>

                    <button class="btn btn-sm btn-secondary flex-1" @click="onDelete">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                            stroke="currentColor" class="w-4 h-4 mr-2">
                            <path stroke-linecap="round" stroke-linejoin="round"
                                d="M14.74 9l-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 01-2.244 2.077H8.084a2.25 2.25 0 01-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 00-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 013.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 00-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 00-7.5 0" />
                        </svg>
                        Delete
                    </button>
                </div>

                <div v-else class="flex flex-1 mt-3 justify-center">
                    <button class="btn btn-sm btn-secondary mr-2 flex-1" @click="onCancel">Cancel</button>
                    <button class="btn btn-sm btn-primary flex-[3]" @click="onSave">Save Changes</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
const route = useRoute()

const defaultImageURL = 'https://upload.wikimedia.org/wikipedia/commons/thumb/6/65/No-Image-Placeholder.svg/1665px-No-Image-Placeholder.svg.png';
const isReadyOnly = useState('isEditView', () => true)

// fetch inventory item data
const { data: inventoryItem } = await useFetch(`http://localhost:3004/inventory/${route.params.id}`);

const onBack = () => {
    navigateTo('/inventory')
}

const onEdit = () => {
    isReadyOnly.value = false;
}

const onCancel = () => {
    isReadyOnly.value = true;
}

const onSave = async () => {
    try {
        await $fetch(`http://localhost:3004/inventory/${route.params.id}`, {
            method: 'PUT',
            body: {
                "id": inventoryItem.value.id,
                "name": inventoryItem.value.name,
                "image": inventoryItem.value.image
            }
        });

        isReadyOnly.value = true;
    } catch (error) {
        console.log(error)
    }
}

const onDelete = async () => {
    try {
        await $fetch(`http://localhost:3004/inventory/${route.params.id}`, {
            method: 'DELETE'
        });

        isReadyOnly.value = true;

        navigateTo('/inventory');
    } catch (error) {
        console.log(error)
    }
}
</script>