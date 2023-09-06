<script setup>
import AdminLayout from "@/Layouts/AdminLayout.vue";
import { Head, Link, useForm } from "@inertiajs/vue3";
import InputError from "@/Components/InputError.vue";
import InputLabel from "@/Components/InputLabel.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import TextInput from "@/Components/TextInput.vue";
import VueMultiselect from "vue-multiselect";

const form = useForm({
    name: "",
    permissions: [],
});

defineProps(["permissions"]);
</script>

<template>
    <Head title="Create New Role" />

    <AdminLayout>
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8 py-12">
            <div class="flex justify-between">
                <Link
                    :href="route('roles.index')"
                    class="px-3 py-2 text-white font-semibold bg-indigo-500 hover:bg-indigo-700 rounded"
                    >Back</Link
                >
            </div>
            <div
                class="mt-6 max-w-6xl mx-auto w-full sm:max-w-6xl px-6 py-4 bg-white dark:bg-gray-800 shadow-md sm:rounded-lg"
            >
                <form @submit.prevent="form.post(route('roles.store'))">
                    <div>
                        <InputLabel for="name" value="Name" />

                        <TextInput
                            id="name"
                            type="text"
                            class="mt-1 block w-full"
                            v-model="form.name"
                            required
                            autofocus
                            autocomplete="off"
                        />

                        <InputError class="mt-2" :message="form.errors.name" />
                    </div>

                    <div class="mt-4">
                        <InputLabel for="permissions" value="Permissions" />
                        <VueMultiselect
                            v-model="form.permissions"
                            :options="permissions"
                            :multiple="true"
                            :close-on-select="true"
                            placeholder="Select Permissions"
                            label="name"
                            track-by="id"
                        />
                        <InputError
                            class="mt-2"
                            :message="form.errors.permissions"
                        />
                    </div>

                    <div class="flex items-center justify-end mt-4">
                        <PrimaryButton
                            class="ml-4"
                            :class="{ 'opacity-25': form.processing }"
                            :disabled="form.processing"
                        >
                            Save Role
                        </PrimaryButton>
                    </div>
                </form>
            </div>
        </div>
    </AdminLayout>
</template>
<style src="vue-multiselect/dist/vue-multiselect.css"></style>
