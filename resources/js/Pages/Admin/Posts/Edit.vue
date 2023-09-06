<script setup>
import AdminLayout from "@/Layouts/AdminLayout.vue";
import { Head, Link, useForm } from "@inertiajs/vue3";
import InputError from "@/Components/InputError.vue";
import InputLabel from "@/Components/InputLabel.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import TextInput from "@/Components/TextInput.vue";

const props = defineProps({
    post: {
        type: Object,
        required: true,
    },
});

const form = useForm({
    title: props.post?.title,
});
</script>

<template>
    <Head title="Update Post" />

    <AdminLayout>
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8 py-12">
            <div class="flex justify-between">
                <Link
                    :href="route('posts.index')"
                    class="px-3 py-2 text-white font-semibold bg-indigo-500 hover:bg-indigo-700 rounded"
                    >Back</Link
                >
            </div>
            <div
                class="mt-6 max-w-6xl mx-auto w-full sm:max-w-5xl px-6 py-4 bg-white dark:bg-gray-800 shadow-md sm:rounded-lg"
            >
                <h1 class="text-2xl p-4 dark:text-gray-400">Update Post</h1>
                <form
                    @submit.prevent="form.put(route('posts.update', post.id))"
                >
                    <div>
                        <InputLabel for="title" value="Post Title" />

                        <TextInput
                            id="title"
                            type="text"
                            class="mt-1 block w-full"
                            v-model="form.title"
                            required
                            autofocus
                            autocomplete="off"
                        />

                        <InputError class="mt-2" :message="form.errors.title" />
                    </div>

                    <div class="flex items-center justify-end mt-4">
                        <PrimaryButton
                            class="ml-4"
                            :class="{ 'opacity-25': form.processing }"
                            :disabled="form.processing"
                        >
                            Update Post
                        </PrimaryButton>
                    </div>
                </form>
            </div>
        </div>
    </AdminLayout>
</template>
