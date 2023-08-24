<script setup>
import AdminLayout from "@/Layouts/AdminLayout.vue";
import { Head, Link, useForm } from "@inertiajs/vue3";
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import TextInput from '@/Components/TextInput.vue';

const props = defineProps({
    permission: {
        type: Object,
        required: true
    }
});

const form = useForm({
    name: props.permission.name,
})
</script>

<template>
    <Head title="Update Permission" />

    <AdminLayout>
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8 py-12">
            <div class="flex justify-between">

                <Link :href="route('permissions.index')"
                    class="px-3 py-2 text-white font-semibold bg-indigo-500 hover:bg-indigo-700 rounded">Back</Link>
            </div>
            <div
                class="mt-6 max-w-md mx-auto w-full sm:max-w-md px-6 py-4 bg-white dark:bg-gray-800 shadow-md overflow-hidden sm:rounded-lg">
                <form @submit.prevent="$event => form.put(route('permissions.update',permission.id))">
                    <div>
                        <InputLabel for="name" value="Permission Name" />

                        <TextInput id="name" type="text" class="mt-1 block w-full" v-model="form.name" required autofocus
                            autocomplete="off" />

                        <InputError class="mt-2" :message="form.errors.name" />
                    </div>

                    <div class="flex items-center justify-end mt-4">
                        <PrimaryButton class="ml-4" :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                            Update
                        </PrimaryButton>
                    </div>
                </form>
            </div>
        </div>
    </AdminLayout>
</template>
