<script setup>
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import { Head, useForm } from "@inertiajs/vue3";
import DangerButton from "@/Components/DangerButton.vue";
import NavLink from "@/Components/NavLink.vue";
import DarkButton from "@/Components/DarkButton.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import InputGroup from "@/Components/InputGroup.vue";
import InputError from "@/Components/InputError.vue";
import { ref, reactive } from "vue";
import select2 from 'select2';
select2();
import Select2 from 'vue3-select2-component';

const props = defineProps({
    authors: { type: Object },
    book: { type: Object },
    authorsOfBook: { type: Object },
});

const form = useForm({
    id: "",
    isbn: "",
    description: "",
    title: "",
    publisher: "",
    release_date: "",
    pages: "",
    image: "",
    authors: [],
});

const title_form = ref(props.book == null ? "Create book" : "Edit book");
const req = ref("required");
const srcImg = ref("../../storage/img/book-view.png");
const msj = ref("");
const classMsj = ref("hidden");
const options = ref([]);
props.authors.map((row) => (
    options.value.push({
        'id': row.id,
        'text': row.last_name + '' + row.name
    })
))

if (props.book != null) {
    form.isbn = props.book.isbn;
    form.title = props.book.title;
    form.description = props.book.description;
    form.release_date = props.book.release_date;
    form.publisher = props.book.publisher;
    form.pages = props.book.pages;
    form.image = props.book.image;
    form.id = props.book.id;
    srcImg.value = '../../storage' + props.book.image;
    props.authorsOfBook.map((row) => (
        form.authors.push(row.id)
    ))
}


const save () => {
    if (props.book == null) {
        form.post(route('books.store'), {
            onSuccess: () => {
                ok('Book created')
            }
        });
    }
    else {
        form.post(route('updateBook'));
    }
}

const ok = (m) => {
    form.reset();
    msj.value = m;
    classMsj.value = "block";
    setTimeout(() => {
        classMsj.value = "hidden";
    }, 8000);
};

const showImg (e) => {
    form.image = e.target.files[0];
    srcImg.value = URL.createObjectURL(e.target.files[0]);
}
</script>

<template>

    <Head title="Book" />
    <AuthenticatedLayout>
        <template #header>
            <div class="inline-flex overflow-hidden mb-4 w-full">
                {{ title_form }}
                <NavLink :href="route('books.index')">
                    <DarkButton>
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                            stroke="currentColor" class="w-6 h-6">
                            <path stroke-linecap="round" stroke-linejoin="round"
                                d="M12 6.042A8.967 8.967 0 0 0 6 3.75c-1.052 0-2.062.18-3 .512v14.25A8.987 8.987 0 0 1 6 18c2.305 0 4.408.867 6 2.292m0-14.25a8.966 8.966 0 0 1 6-2.292c1.052 0 2.062.18 3 .512v14.25A8.987 8.987 0 0 0 18 18a8.967 8.967 0 0 0-6 2.292m0-14.25v14.25" />
                        </svg>
                    </DarkButton>
                </NavLink>
            </div>
        </template>
        <div class="flex justify-center items-center w-12 bg-green-400">
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                stroke="currentColor" class="w-6 h-6">
                <path stroke-linecap="round" stroke-linejoin="round"
                    d="M9 12.75 11.25 15 15 9.75M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
            </svg>
        </div>
        <div class="grid gap-6 bg-white mb-8 md:grid-cols-2 border rounded-lg">
            <div class="min-w-0 p-4 rounded-lg shadow-xs">
                <form class="mt-6 mb-6 space-y-6 max-w-xl" @submit.prevent="save">
                    <InputGroup :text="'ISBN'" :required="'required'" v-model="form.isbn">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                            stroke="currentColor" class="w-6 h-6">
                            <path stroke-linecap="round" stroke-linejoin="round"
                                d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5" />
                        </svg>
                    </InputGroup>
                    <InputError class="mt-1" :message="form.errors.isbn"></InputError>
                    <InputGroup :text="'Description'" :required="'required'" v-model="form.description">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                            stroke="currentColor" class="w-6 h-6">
                            <path stroke-linecap="round" stroke-linejoin="round"
                                d="M7.5 8.25h9m-9 3H12m-9.75 1.51c0 1.6 1.123 2.994 2.707 3.227 1.129.166 2.27.293 3.423.379.35.026.67.21.865.501L12 21l2.755-4.133a1.14 1.14 0 0 1 .865-.501 48.172 48.172 0 0 0 3.423-.379c1.584-.233 2.707-1.626 2.707-3.228V6.741c0-1.602-1.123-2.995-2.707-3.228A48.394 48.394 0 0 0 12 3c-2.392 0-4.744.175-7.043.513C3.373 3.746 2.25 5.14 2.25 6.741v6.018Z" />
                        </svg>
                    </InputGroup>
                    <InputError class="mt-1" :message="form.errors.description"></InputError>
                    <InputGroup :text="'Title'" :required="'required'" v-model="form.title">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                            stroke="currentColor" class="w-6 h-6">
                            <path stroke-linecap="round" stroke-linejoin="round"
                                d="M12 6.042A8.967 8.967 0 0 0 6 3.75c-1.052 0-2.062.18-3 .512v14.25A8.987 8.987 0 0 1 6 18c2.305 0 4.408.867 6 2.292m0-14.25a8.966 8.966 0 0 1 6-2.292c1.052 0 2.062.18 3 .512v14.25A8.987 8.987 0 0 0 18 18a8.967 8.967 0 0 0-6 2.292m0-14.25v14.25" />
                        </svg>
                    </InputGroup>
                    <InputError class="mt-1" :message="form.errors.title"></InputError>
                    <InputGroup :text="'Publisher'" :required="'required'" v-model="form.publisher">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                            stroke="currentColor" class="w-6 h-6">
                            <path stroke-linecap="round" stroke-linejoin="round"
                                d="M12 21v-8.25M15.75 21v-8.25M8.25 21v-8.25M3 9l9-6 9 6m-1.5 12V10.332A48.36 48.36 0 0 0 12 9.75c-2.551 0-5.056.2-7.5.582V21M3 21h18M12 6.75h.008v.008H12V6.75Z" />
                        </svg>
                    </InputGroup>
                    <InputError class="mt-1" :message="form.errors.publisher"></InputError>
                    <InputGroup :text="'Release day'" :required="'required'" v-model="form.release_date" :type="'date'">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                            stroke="currentColor" class="w-6 h-6">
                            <path stroke-linecap="round" stroke-linejoin="round"
                                d="M12 21v-8.25M15.75 21v-8.25M8.25 21v-8.25M3 9l9-6 9 6m-1.5 12V10.332A48.36 48.36 0 0 0 12 9.75c-2.551 0-5.056.2-7.5.582V21M3 21h18M12 6.75h.008v.008H12V6.75Z" />
                        </svg>
                    </InputGroup>
                    <InputError class="mt-1" :message="form.errors.release_date"></InputError>
                    <InputGroup :text="'Pages'" :required="'required'" v-model="form.pages" :type="'number'">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                            stroke="currentColor" class="w-6 h-6">
                            <path stroke-linecap="round" stroke-linejoin="round"
                                d="M5.25 8.25h15m-16.5 7.5h15m-1.8-13.5-3.9 19.5m-2.1-19.5-3.9 19.5" />
                        </svg>
                    </InputGroup>
                    <InputError class="mt-1" :message="form.errors.pages"></InputError>
                    <InputGroup v-if="props.book == null" :required="'required'" :text="'Image'" :type="'file'"
                        :accept="'image/*'" @change="showImg($event)">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                            stroke="currentColor" class="w-6 h-6">
                            <path stroke-linecap="round" stroke-linejoin="round"
                                d="m2.25 15.75 5.159-5.159a2.25 2.25 0 0 1 3.182 0l5.159 5.159m-1.5-1.5 1.409-1.409a2.25 2.25 0 0 1 3.182 0l2.909 2.909m-18 3.75h16.5a1.5 1.5 0 0 0 1.5-1.5V6a1.5 1.5 0 0 0-1.5-1.5H3.75A1.5 1.5 0 0 0 2.25 6v12a1.5 1.5 0 0 0 1.5 1.5Zm10.5-11.25h.008v.008h-.008V8.25Zm.375 0a.375.375 0 1 1-.75 0 .375.375 0 0 1 .75 0Z" />
                        </svg>
                    </InputGroup>
                    <InputGroup v-else :text="'Image'" :required="'required'" :type="'file'" :accept="'image/*'"
                        @change="showImg($event)">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                            stroke="currentColor" class="w-6 h-6">
                            <path stroke-linecap="round" stroke-linejoin="round"
                                d="m2.25 15.75 5.159-5.159a2.25 2.25 0 0 1 3.182 0l5.159 5.159m-1.5-1.5 1.409-1.409a2.25 2.25 0 0 1 3.182 0l2.909 2.909m-18 3.75h16.5a1.5 1.5 0 0 0 1.5-1.5V6a1.5 1.5 0 0 0-1.5-1.5H3.75A1.5 1.5 0 0 0 2.25 6v12a1.5 1.5 0 0 0 1.5 1.5Zm10.5-11.25h.008v.008h-.008V8.25Zm.375 0a.375.375 0 1 1-.75 0 .375.375 0 0 1 .75 0Z" />
                        </svg>
                    </InputGroup>
                    <InputError class="mt-1" :message="form.errors.image"></InputError>
                    <span class="mt-2">Add Authors</span>
                    <Select2 v-model="form.authors" :options="options" :settings="{ multiple: true, width: '100%' }"
                        @change="form.authors = $event.target.value" />
                    <InputError class="mt-1" :message="form.errors.authors"></InputError>
                    <PrimaryButton>Save</PrimaryButton>
                </form>
            </div>
            <div class="min-w-0 p-4 rounded-lg shadow-xs">
                <img :src="srcImg" width="500" />
            </div>
        </div>
    </AuthenticatedLayout>
</template>
