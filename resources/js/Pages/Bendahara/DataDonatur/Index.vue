<script setup>
import AuthenticatedLayout from '@/Layouts/AuthenticatedLayout.vue';
import { Head, Link, useForm } from '@inertiajs/vue3';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import { defineProps, ref ,watch} from 'vue';
import Dropdown from '@/Components/Dropdown.vue';
import DropdownLink from '@/Components/DropdownLink.vue';
import Modal from '@/Components/Modal.vue';
import Pagination from '@/Components/Pagination.vue';
import FlashMessage from '@/Components/FlashMessage.vue';

const props = defineProps({
    data_donatur: {
        type: Object,
        default: () => ({}),
    },
    search: String,
    total_saldo: Number,
})
// Search Form
const SearchForm = useForm({});
const search = ref(props.search);

watch(search, (value)=>{
    SearchForm.get(route('DataDonatur.index', {
        search: value,
    }));
})

const rupiah = (number) => {
    return new Intl.NumberFormat("id-ID", {
        style: "currency",
        currency: "IDR"
    }).format(number);
}
// Modal
const modalDelete = ref(false);
function showModaldelete(param) {
    modalDelete.value = true;
    FormDelete.slug = param;
}
function closeModal() {
    modalDelete.value = false;
    FormDelete.slug = null;
}

// Delete
const FormDelete = useForm({
    slug: null,
});
function deleteJadwal() {
    FormDelete.delete(route('DataDonatur.delete'),{
        onSuccess:()=>{
            modalDelete.value=false;
            FormDelete.reset()
        }
    })
}
console.log(props.data_donatur)
</script>

<template>
    <Head title="donatur" />

    <AuthenticatedLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">Data Donatur</h2>
            <FlashMessage />
        </template>

        <div class="py-4">
            <section class="bg-transparent px-3 sm:px-5">
                <div class="mx-auto max-w-screen-xl px-4 lg:px-12">
                    <!-- Start coding here -->
                    <div class="bg-white relative shadow-md sm:rounded-lg overflow-hidden">
                        <div
                            class="flex flex-col md:flex-row items-center justify-between space-y-3 md:space-y-0 md:space-x-4 p-4">
                            <div class="w-full">
                                <h6>Total : <span>{{ rupiah(total_saldo) }}</span></h6>
                            </div>
                            <div class="w-full md:w-1/2">
                                <form class="flex items-center">
                                    <label for="simple-search" class="sr-only">Search</label>
                                    <div class="relative w-full">
                                        <div class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
                                            <svg aria-hidden="true" class="w-5 h-5 text-gray-500" fill="currentColor"
                                                viewbox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                                                <path fill-rule="evenodd"
                                                    d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z"
                                                    clip-rule="evenodd" />
                                            </svg>
                                        </div>
                                        <input type="text" id="simple-search" v-model="search" autofocus
                                            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-primary-500 focus:border-primary-500 block w-full pl-10 p-2 "
                                            placeholder="Search" required="">
                                    </div>
                                </form>
                            </div>
                            <div
                                class="w-full md:w-auto flex flex-col md:flex-row space-y-2 md:space-y-0 items-stretch md:items-center justify-end md:space-x-3 flex-shrink-0">
                                <Link :href="route('DataDonatur.create')">
                                <PrimaryButton>Tambah</PrimaryButton>
                                </Link>
                                <div class="flex items-center space-x-3 w-full md:w-auto">

                                </div>
                            </div>
                        </div>
                        <div class="overflow-x-auto px-2">
                            <table class="w-full text-sm text-left text-gray-500">
                                <thead class="text-xs text-gray-700 uppercase bg-gray-50 ">
                                    <tr>
                                        <th scope="col" class="border text-center">No.</th>

                                        <th scope="col" class="px-4 py-3 border">Bukti</th>
                                        <th scope="col" class="px-4 py-3 border">Nama</th>
                                        <th scope="col" class="px-4 py-3 border">Jenis</th>
                                        <th scope="col" class="px-4 py-3 border">Jumlah</th>
                                        <th scope="col" class="px-4 py-3 border">Tanggal</th>
                                        <th scope="col" class="px-4 py-3 border">Keterangan</th>
                                        <th scope="col" class="px-4 py-3 border">
                                            <span class="sr-only">Actions</span>
                                        </th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="(item,index) in data_donatur.data" class="border-b ">
                                        <th class="border text-center">{{ (data_donatur.current_page - 1) * data_donatur.per_page + index+1 }}.</th>

                                        <td class="px-4 py-3 border w-32">
                                            <img :src="item.path_bukti" :alt="item.jenis" srcset="">
                                        </td>

                                        <th scope="row"
                                            class="px-4 py-3 border font-medium text-gray-900 whitespace-nowrap text-start ">
                                            {{ item.nama_donatur }}</th>
                                        <td class="px-4 py-3 border">{{ item.jenis }}</td>
                                        <td class="px-4 py-3 border">{{ rupiah(item.jumlah) }}</td>
                                        <td class="px-4 py-3 border">{{ item.tanggal }}</td>
                                        <td class="px-4 py-3 border">
                                            <p v-html="item.ket"></p>
                                        </td>
                                        <td class="px-4 py-3 border ">
                                            <Dropdown align="top" width="48">
                                                <template #trigger>
                                                    <span class="inline-flex rounded-md">
                                                        <button type="button"
                                                            class="inline-flex items-center px-3 py-2 border border-transparent text-sm leading-4 font-medium rounded-md text-white bg-red-500 hover:text-gray-700 focus:outline-none transition ease-in-out duration-150">
                                                            <svg class="w-5 h-5" aria-hidden="true" fill="currentColor"
                                                                viewbox="0 0 20 20" xmlns="http://www.w3.org/2000/svg">
                                                                <path
                                                                    d="M6 10a2 2 0 11-4 0 2 2 0 014 0zM12 10a2 2 0 11-4 0 2 2 0 014 0zM16 12a2 2 0 100-4 2 2 0 000 4z" />
                                                            </svg>
                                                        </button>
                                                    </span>
                                                </template>

                                                <template #content>
                                                    <DropdownLink class="bg-green-500 hover:bg-green-600 active:bg-green-400 text-white"
                                                        :href="route('DataDonatur.edit', { nama: item.nama, slug: item.id, alamat: item.alamat, })">
                                                        Edit </DropdownLink>
                                                    <PrimaryButton type="button" class="!bg-error px-1 py-1 text-xs text-white hover:bg-red-600 active:bg-red-400 w-full block"
                                                        @click="showModaldelete(item.id)">
                                                        Hapus
                                                    </PrimaryButton>
                                                </template>
                                            </Dropdown>
                                        </td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                        <Pagination :links="data_donatur.links" />
                    </div>
                </div>
            </section>
            <Modal :show="modalDelete" :maxWidth="'md'">
                <div class="max-w-full h-full flex justify-center items-center ">
                    <div class="block bg-white rounded-lg py-5">
                        <h3 class="mb-4">Apakah Anda Yakin?</h3>
                        <div class="flex justify-around">
                            <PrimaryButton type="button" @click="deleteJadwal()" class="bg-blue-500 hover:bg-blue-600 active:bg-blue-800">Ya
                            </PrimaryButton>
                            <PrimaryButton type="button" @click="closeModal()"
                                class="bg-error hover:bg-red-600 active:bg-red-800">Batal</PrimaryButton>
                        </div>
                    </div>
                </div>
            </Modal>
        </div>
    </AuthenticatedLayout>
</template>
