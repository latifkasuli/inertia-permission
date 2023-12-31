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

defineProps(["permissions"]);
const form = useForm({});

const showConfirmDeletePermissionModal = ref(false);

const confirmDeletePermission = () => {
    showConfirmDeletePermissionModal.value = true;
};

const closeModal = () => {
    showConfirmDeletePermissionModal.value = false;
};

const deletePermission = (id) => {
    form.delete(route("permissions.destroy", id), {
        onSuccess: () => closeModal(),
    });
};
</script>

<template>
    <Head title="Permissions" />

    <AdminLayout>
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8 py-12">
            <div class="flex justify-between">
                <h1>Permissions Index Page</h1>

                <Link
                    :href="route('permissions.create')"
                    class="px-3 py-2 text-white font-semibold bg-indigo-500 hover:bg-indigo-700 rounded"
                    >New Permission
                </Link>
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
                            v-for="permission in permissions"
                            :key="permission.id"
                            class="border-b"
                        >
                            <table-data-cell>{{
                                permission.id
                            }}</table-data-cell>
                            <table-data-cell>{{
                                permission.name
                            }}</table-data-cell>
                            <table-data-cell class="space-x-4">
                                <Link
                                    :href="
                                        route('permissions.edit', permission.id)
                                    "
                                    class="text-green-400 hover:text-green-600"
                                    >Edit
                                </Link>
                                <button
                                    @click="confirmDeletePermission"
                                    class="text-red-400 hover:text-red-600"
                                >
                                    Delete
                                </button>
                                <Modal
                                    :show="showConfirmDeletePermissionModal"
                                    @close="closeModal"
                                    maxWidth="md"
                                >
                                    <div class="p-6">
                                        <h2
                                            class="text-lg font-semibold text-slate-800 dark:text-slate-200 text-center"
                                        >
                                            Are you sure to delete this
                                            permission
                                        </h2>
                                        <div
                                            class="mt-6 flex space-x-4 justify-end"
                                        >
                                            <DangerButton
                                                @click="
                                                    deletePermission(
                                                        permission.id
                                                    )
                                                "
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
