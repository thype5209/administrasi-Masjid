<template>
    <div>
        <div class="flex flex-wrap -mb-1 bg-white py-1 pl-3">
            <template v-for="(link, key) in links">
                <div v-if="link.url === null" :key="key"
                    class="mb-1 mr-1 px-4 py-3 text-gray-400 text-sm leading-4 border rounded" v-html="link.label" />
                <Link v-else :key="`link-${key}`"
                    class="mb-1 mr-1 px-4 py-3 focus:text-indigo-500 text-sm leading-4 active:border-blue-400 hover:bg-gray-200 border focus:border-indigo-500 rounded"
                    :class="{ 'bg-white border-blue-500 text-black': link.active }" preserve-state preserve-scroll :data="{search}" :href="link.url" v-html="link.label" />
            </template>
        </div>
    </div>
</template>

<script>
import { Head, useForm, Link } from '@inertiajs/vue3';


export default {
    components: {
        Link,
    },
    props: {
        links: Array,
        search: String,
        statusBayar: String,
        status: String,
    },
}
</script>
