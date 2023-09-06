<script setup>
import AdminLayout from "@/Layouts/AdminLayout.vue";
import { Head, Link, useForm } from "@inertiajs/vue3";
import Table from "@/Components/Table.vue";
import TableHeaderCell from "@/Components/TableHeaderCell.vue";
import TableRow from "@/Components/TableRow.vue";
import TableDataCell from "@/Components/TableDataCell.vue";
import { ref } from "vue";
import Modal from "@/Components/Modal.vue";
import DangerButton from "@/Components/DangerButton.vue";
import SecondaryButton from "@/Components/SecondaryButton.vue";

defineProps(["roles"]);
const form = useForm({});

const showConfirmDeleteRoleModal = ref(false);

const confirmDeleteRole = () => {
    showConfirmDeleteRoleModal.value = true;
};

const closeModal = () => {
    showConfirmDeleteRoleModal.value = false;
};

const deleteRole = (id) => {
    form.delete(route("roles.destroy", id), {
        onSuccess: () => closeModal(),
    });
};
</script>

<template>
    <Head title="All Roles" />

    <AdminLayout>
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8 py-12">
            <div class="flex justify-between">
                <h1>Roles Index Page</h1>

                <Link
                    :href="route('roles.create')"
                    class="px-3 py-2 text-white font-semibold bg-indigo-500 hover:bg-indigo-700 rounded"
                    >New Role</Link
                >
            </div>
            <div class="mt-6">
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
                            v-for="role in roles"
                            :key="role.id"
                            class="border-b"
                        >
                            <table-data-cell>{{ role.id }}</table-data-cell>
                            <table-data-cell>{{ role.name }}</table-data-cell>
                            <table-data-cell class="space-x-4">
                                <Link
                                    :href="route('roles.edit', role.id)"
                                    class="text-green-400 hover:text-green-600"
                                    >Edit
                                </Link>
                                <button
                                    @click="confirmDeleteRole"
                                    class="text-red-400 hover:text-red-600"
                                >
                                    Delete
                                </button>
                                <Modal
                                    :show="showConfirmDeleteRoleModal"
                                    @close="closeModal"
                                    maxWidth="md"
                                >
                                    <div class="p-6">
                                        <h2
                                            class="text-lg font-semibold text-slate-800 dark:text-slate-200 text-center"
                                        >
                                            Are you sure to delete this role
                                        </h2>
                                        <div
                                            class="mt-6 flex space-x-4 justify-end"
                                        >
                                            <DangerButton
                                                @click="deleteRole(role.id)"
                                                >Yes</DangerButton
                                            >
                                            <SecondaryButton @click="closeModal"
                                                >Cancel</SecondaryButton
                                            >
                                        </div>
                                    </div>
                                </Modal>
                            </table-data-cell>
                        </TableRow>
                    </template>
                </Table>
            </div>
        </div>
    </AdminLayout>
</template>
