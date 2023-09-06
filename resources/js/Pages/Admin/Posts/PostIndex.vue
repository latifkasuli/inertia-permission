<script setup>
import AdminLayout from "@/Layouts/AdminLayout.vue";
import { Head, Link, useForm } from "@inertiajs/vue3";
import { usePermission } from "@/Composables/permissions";
import Table from "@/Components/Table.vue";
import TableHeaderCell from "@/Components/TableHeaderCell.vue";
import TableRow from "@/Components/TableRow.vue";
import TableDataCell from "@/Components/TableDataCell.vue";
import { ref } from "vue";
import Modal from "@/Components/Modal.vue";
import DangerButton from "@/Components/DangerButton.vue";
import SecondaryButton from "@/Components/SecondaryButton.vue";

defineProps({
    posts: {
        type: Object,
    },
});
const form = useForm({});

const showConfirmDeletePostModal = ref(false);
const { hasPermission } = usePermission();

const confirmDeletePost = () => {
    showConfirmDeletePostModal.value = true;
};

const closeModal = () => {
    showConfirmDeletePostModal.value = false;
};

const deletePost = (id) => {
    form.delete(route("posts.destroy", id), {
        onSuccess: () => closeModal(),
    });
};
</script>

<template>
    <Head title="All Posts" />

    <AdminLayout>
        <div class="max-w-7xl mx-auto sm:px-6 lg:px-8 py-12">
            <div class="flex justify-between">
                <h1>Posts Index Page</h1>
                <template v-if="hasPermission('create post')">
                    <Link
                        :href="route('posts.create')"
                        class="px-3 py-2 text-white font-semibold bg-indigo-500 hover:bg-indigo-700 rounded"
                        >New Post</Link
                    >
                </template>
            </div>
            <div class="mt-6">
                <Table>
                    <template #tablehead>
                        <TableRow>
                            <table-header-cell>#</table-header-cell>
                            <table-header-cell>Title</table-header-cell>
                            <table-header-cell>Action</table-header-cell>
                        </TableRow>
                    </template>
                    <template #default>
                        <TableRow
                            v-for="post in posts"
                            :key="post.id"
                            class="border-b"
                        >
                            <table-data-cell>{{ post.id }}</table-data-cell>
                            <table-data-cell>{{ post.title }}</table-data-cell>
                            <table-data-cell class="space-x-4">
                                <template v-if="hasPermission('create post')">
                                    <Link
                                        :href="route('posts.edit', post.id)"
                                        class="text-green-400 hover:text-green-600"
                                        >Edit
                                    </Link>
                                </template>

                                <template v-if="hasPermission('create post')">
                                    <button
                                        @click="confirmDeletePost"
                                        class="text-red-400 hover:text-red-600"
                                    >
                                        Delete
                                    </button>
                                    <Modal
                                        :show="showConfirmDeletePostModal"
                                        @close="closeModal"
                                        maxWidth="md"
                                    >
                                        <div class="p-6">
                                            <h2
                                                class="text-lg font-semibold text-slate-800 dark:text-slate-200 text-center"
                                            >
                                                Are you sure to delete this post
                                            </h2>
                                            <div
                                                class="mt-6 flex space-x-4 justify-end"
                                            >
                                                <DangerButton
                                                    @click="deletePost(post.id)"
                                                    >Yes</DangerButton
                                                >
                                                <SecondaryButton
                                                    @click="closeModal"
                                                    >Cancel</SecondaryButton
                                                >
                                            </div>
                                        </div>
                                    </Modal>
                                </template>
                            </table-data-cell>
                        </TableRow>
                    </template>
                </Table>
            </div>
        </div>
    </AdminLayout>
</template>
