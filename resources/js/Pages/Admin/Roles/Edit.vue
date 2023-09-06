<script setup>
import AdminLayout from "@/Layouts/AdminLayout.vue";
import { Head, Link, useForm } from "@inertiajs/vue3";
import InputError from "@/Components/InputError.vue";
import InputLabel from "@/Components/InputLabel.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import TextInput from "@/Components/TextInput.vue";
import VueMultiselect from "vue-multiselect";
import Table from "@/Components/Table.vue";
import TableHeaderCell from "@/Components/TableHeaderCell.vue";
import TableRow from "@/Components/TableRow.vue";
import TableDataCell from "@/Components/TableDataCell.vue";
import { onMounted, watch } from "vue";

const props = defineProps({
    role: {
        type: Object,
        required: true,
    },
    permissions: Array,
});

const form = useForm({
    name: props.role.name,
    permissions: [],
});

onMounted(() => {
    form.permissions = props.role?.permissions;
});

watch(
    () => props.role,
    () => (form.permissions = props.role?.permissions)
);
</script>

<template>
    <Head title="Update Role" />

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
                class="mt-6 max-w-6xl mx-auto w-full sm:max-w-5xl px-6 py-4 bg-white dark:bg-gray-800 shadow-md sm:rounded-lg"
            >
                <h1 class="text-2xl p-4 dark:text-gray-400">Update Role</h1>
                <form
                    @submit.prevent="form.put(route('roles.update', role.id))"
                >
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
                            Update Role
                        </PrimaryButton>
                    </div>
                </form>
            </div>
            <div
                class="mt-6 max-w-6xl mx-auto w-full sm:max-w-5xl px-6 py-4 bg-white dark:bg-gray-800 shadow-md sm:rounded-lg"
            >
                <h1 class="text-2xl p-4 dark:text-gray-400">Permissions</h1>
                <Table>
                    <template #tablehead>
                        <TableRow>
                            <table-header-cell>#</table-header-cell>
                            <table-header-cell>Name</table-header-cell>
                            <table-header-cell>Action</table-header-cell>
                        </TableRow>
                    </template>
                    <template #default>
                        <TableRow
                            v-for="rolePermission in role.permissions"
                            :key="rolePermission.id"
                            class="border-b"
                        >
                            <table-data-cell>{{
                                rolePermission.id
                            }}</table-data-cell>
                            <table-data-cell>{{
                                rolePermission.name
                            }}</table-data-cell>
                            <table-data-cell class="space-x-4">
                                <Link
                                    :href="
                                        route('roles.permissions.destroy', [
                                            role.id,
                                            rolePermission.id,
                                        ])
                                    "
                                    method="DELETE"
                                    as="button"
                                    class="text-red-400 hover:text-red-600"
                                >
                                    Revoke
                                </Link>
                            </table-data-cell>
                        </TableRow>
                    </template>
                </Table>
            </div>
        </div>
    </AdminLayout>
</template>
<style src="vue-multiselect/dist/vue-multiselect.css"></style>
