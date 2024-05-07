<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head, useForm } from '@inertiajs/vue3';

const props = defineProps({
    authors: { type: Object },
    countries: { type: Object }
})
</script>

<template>
    <div>

        <Head title="Authors" />
        <AuthenticatedLayout>
            <template #header>
                Authors
            </template>

            <div class="w-full overflow-hidden rounded-lg border shadow-md">
                <div class="w-full overflow-x-auto bg-white">
                    <table class="w-full whitespace-nowrap">
                        <thead>
                            <tr
                                class="text-xs font-semibold tracking-wide text-left text-gray-500 uppercase border-b dark:border-gray-700 bg-gray-50 dark:text-gray-400 dark:bg-gray-800">
                                <th class="px-4 py-3">#Header</th>
                                <th class="px-4 py-3">Name</th>
                                <th class="px-4 py-3">Last Name</th>
                                <th class="px-4 py-3">Country</th>
                                <th class="px-4 py-3">Detail</th>
                                <th class="px-4 py-3">Edit</th>
                                <th class="px-4 py-3">Delete</th>
                            </tr>
                        </thead>
                        <tbody class="bg-white divide-y dark:divide-gray-700 dark:bg-gray-200">
                            <tr v-for="a, i in authors" :key="a.id" class="text-gray-700">
                                <td class="px-4 py-3 text-sm">{{ (i + 1) }}</td>
                                <td class="px-4 py-3 text-sm">{{ a.name }}</td>
                                <td class="px-4 py-3 text-sm">{{ a.last_name }}</td>
                                <td class="px-4 py-3 text-sm">{{ a.countries.name }}</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </AuthenticatedLayout>
    </div>
</template>



<style lang="scss" scoped></style>
