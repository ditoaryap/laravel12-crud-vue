<script setup lang="ts">
import AppLayout from '@/layouts/AppLayout.vue';
import { type BreadcrumbItem } from '@/types';
import { Head, Link, useForm } from '@inertiajs/vue3';
import InputError from '@/components/InputError.vue';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import { Button } from '@/components/ui/button';

const breadcrumbs: BreadcrumbItem[] = [
    {
        title: 'Posts Edit',
        href: '/posts/create',
    },
];



const props = defineProps({
    post: {
        type: Object,
        default: () => ({ title: '', body: '' })
    }
});

const form = useForm({
    title: props.post.title || "",
    body: props.post.body || "",
});

function submit() {
    form.put(route(`posts.store`), {
        preserveScroll: true,

        onSuccess: () => form.reset()
    });
}

</script>

<template>
    <Head title="Posts Edit" />

    <AppLayout :breadcrumbs="breadcrumbs">
        <div class="flex h-full flex-1 flex-col gap-4 rounded-xl p-4">

            <div>
                <Link href="/posts" class="cursor-pointer px-3 py-2 text-xs font-medium text-white bg-blue-700 rounded-lg hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">
                    Back
                </Link>
            </div>

            <form @submit.prevent="submit">
                <div class="grid gap-2">
                    <Label for="title">Title:</Label>
                    <Input
                        id="title"
                        type="text"
                        class="mt-1 block w-full"
                        v-model="form.title"
                        placeholder="Title"
                    />
                    <InputError class="mt-2" :message="form.errors.title" />
                </div>

                <div class="grid gap-2 mt-3">
                    <Label for="body">Body:</Label>
                    <Input
                        id="body"
                        type="textarea"
                        class="mt-1 block w-full file:text-foreground placeholder:text-muted-foreground selection:bg-primary selection:text-primary-foreground dark:bg-input/30 border-input h-15 min-w-0 rounded-md border bg-transparent px-3 shadow-xs transition-[color,box-shadow] outline-none file:inline-flex file:h-7 file:border-0 file:bg-transparent file:text-sm file:font-medium disabled:pointer-events-none disabled:cursor-not-allowed disabled:opacity-50 md:text-sm"
                        v-model="form.body"
                        placeholder="Body"
                    />
                    <InputError class="mt-2" :message="form.errors.body" />
                </div>

                <div class="flex items-center gap-4 mt-3">
                    <Button>Update</Button>
                </div>


            </form>

        </div>
    </AppLayout>
</template>
