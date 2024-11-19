<script setup>
const supabase = useSupabaseClient()
const keyword = ref('')
const cake = ref()
const selectCake = ref({})
const categories = ref([])

function toggleDelete(cake) {
    selectCake.value = cake
}

const { data: cakes, refresh } = useLazyAsyncData('cakes', async () => {
    const { data, error } = await supabase.from('produk').select(`*, kategori(*), ukuran(*)`).order("created_at", { ascending: false })
    if (error) throw error
    return (data)
})

const getCategory = async () => {
    const { data, error } = await supabase.from('kategori').select('*')
    if (data) categories.value = data
}

const cakeFiltered = computed(() => {
    return cakes.value?.filter((c) => {
        return (
            c.nama_kue?.toLowerCase().includes(keyword.value.toLowerCase()) ||
            c.kategori?.nama.toLowerCase().includes(keyword.value.toLowerCase())
        )
    })
})

const deleteCake = async (id) => {
    console.log(id)
    const { error } = await supabase.from('produk').delete().eq('id', id)
    if (error) throw error
    else
        refresh()
}

onMounted(() => {
    getCategory()
    refresh()
})

definePageMeta({
    layout: 'admin',
    middleware: 'auth'
})

</script>

<template>
    <div class="container-fluid pt-4 mb-5">

        <div class="row">
            <div class="col-md-1 mt-2">
                <nuxt-link to="/dashboard" style="text-decoration: none; color: black;">
                    <i class="bi bi-arrow-left-circle fs-2"></i>
                </nuxt-link>
            </div>
            <div class="col-md-10">
                <h2 class="mt-3 head-tittle">Product</h2>
            </div>
        </div>

        <!--Search Categories and Add-->
        <div class="p-5">
            <div class="row p-2">
                <div class="col-md-5 search">
                    <form @submit.prevent="getCake" class="input-group flex-nowrap">
                        <input v-model="keyword" type="text" class="form-control rounded-5" placeholder="Cari nama kue"
                            aria-label="Search" aria-describedby="search-addon" />
                    </form>
                </div>
                <div class="col-md-2 categories">
                    <select v-model="keyword" class="form-control form-select rounded-5" id="category">
                        <option value="">Category</option>
                        <option v-for="(category, i) in categories" :key="category.id" :value="category.nama">{{
                            category.nama }}</option>
                    </select>
                </div>
                <div class="col-md-5 text-end btn-add">
                    <nuxt-link to="../product/add">
                        <button class="btn rounded-5">Tambah Kue</button>
                    </nuxt-link>
                </div>
            </div>
        </div>

        <!--Product-->
        <div class="p-3">
            <div class="card p-3 rounded shadow">
                <div class="card-body">
                    <div class="table-responsive">
                        <table class="table table-striped">
                            <thead>
                                <tr>
                                    <th>No</th>
                                    <th>Foto Produk</th>
                                    <th>Nama Produk</th>
                                    <th>Kategori</th>
                                    <th>Ukuran</th>
                                    <th>Harga</th>
                                    <th></th>
                                    <th></th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr  v-for="(cake, i) in cakeFiltered" :key="i">
                                    <td>{{ i + 1 }}</td>
                                    <td><img :src="cake.foto_kue" class="rounded-3"></td>
                                    <td>{{ cake.nama_kue }}</td>
                                    <td>{{ cake.kategori?.nama }}</td>
                                    <td>{{ cake.ukuran?.nama }}</td>
                                    <td>{{ cake.harga }}</td>
                                    <td><nuxt-link :to="`/product/${cake.id}`" style="color: black;">
                                        <i class="bi bi-pencil fs-4 fw-bold edit"></i>
                                    </nuxt-link></td>
                                    <td><i class="bi bi-trash3 fs-4 fw-bold delete" data-bs-toggle="modal"
                                        data-bs-target="#deleteModal" @click="toggleDelete(cake)"></i></td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>

        <!--Modal Delete-->
        <div class="modal fade center" id="deleteModal" tabindex="-1" aria-labelledby="deleteModalLabel"
            aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered modal-md p-4 rounded-5">
                <div class="modal-content">
                    <div class="modal-header">
                        <div class="modal-title fs-5" id="exampleModalLabel"></div>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <div class="modal-body text-center">
                        <h5>Apakah anda yakin akan menghapus produk ini? {{ selectCake.nama_kue }}</h5>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn edit rounded-5" data-bs-dismiss="modal">Batal</button>
                        <button type="button" class="btn delete rounded-5" data-bs-dismiss="modal"
                            @click="deleteCake(selectCake.id)">Hapus</button>
                    </div>
                </div>
            </div>
        </div>


    </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Junge&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Kavoon&family=Miltonian+Tattoo&family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Red+Rose:wght@300..700&display=swap');


h2 {
    font-family: "Junge", cursive;
}

h3,
h5,
h6,
.form-control,
.btn,
th, td {
    font-family: "Poppins", sans-serif;
}

i {
    margin-left: 50px;
}

.form-control {
    font-size: 14px;
    height: 45px;
    border: 1px solid #C6AC7B;
}

.custom {
    background-color: #C6AC7B;
    color: white;
    height: 20px;
}

.btn {
    width: 175px;
    margin-right: 10px;
    font-size: 17px;
    color: white;
    background-color: #C6AC7B;
}

.cake {
    padding: 50px;
}

.categories img {
    width: 100px;
}

.card {
    border: 1px solid #C6AC7B;
}

.add {
    width: 200px;
    font-size: 20px;
}

hr {
    margin-top: 25px;
    border: 1px solid #C6AC7B;

}

img {
    width: 90px;

}

.delete {
    margin-left: 40px;
}

.edit {
    margin-left: 50px;
}

.modal {
    height: 300px;
}

.modal-header,
.modal-content,
.modal-footer {
    border: none;
}

.modal-dialog {
    background-color: white;
}

.modal-footer .btn {
    width: 100px;
}

.modal-footer .delete {
    color: white;
    background-color: red !important;
}

.header h5 {
    font-weight: 700;
}


@media only screen and (min-width: 600px) and (max-width: 890px) {
    .head-tittle {
        margin-left: 30px;
    }
}


@media only screen and (max-width: 600px) {
    .categories, .btn-add {
        margin-top: 10px;
    }
    
    h5 {
        font-size: small;
    }

    i {
        font-size: 0.2rem;
        margin-left: 0;
    }

    .delete {
        margin-left: 20px;
    }

    .edit {
        margin-left: 50px;
    }
}

</style>