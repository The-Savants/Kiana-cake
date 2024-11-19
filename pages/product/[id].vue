<script setup>
const supabase = useSupabaseClient()
const route = useRoute()
const cakeId = route.params.id
const categories = ref([])
const sizes = ref([])
const imageCake = ref()
const cake = ref({
    nama_kue: "",
    id_kategori: null,
    id_ukuran: null,
    harga: "",
    stok: null,
    foto_kue: null
})

const getCakeId = async () => {
    let { data, error } = await supabase.from('produk').select(`
        *,
        kategori (
            *
        )
    `)
        .eq('id', cakeId).single()
    if (data) {
        if (data.foto_kue) data.foto_kuePath = data.foto_kue.substr(data.foto_kue.indexOf(data.kategori.nama.toLowerCase())) 
        cake.value = data
    }
}

async function imgPicked(e) {
    const file = e.target.files[0]
    imageCake.value = file
    const { data: category, error } = await supabase.from('kategori').select('nama').eq('id', cake.value.kategori.id).single()
    if (error) throw error
    cake.value.foto_kuePath = `${category.nama.replace(' ', '-').toLowerCase()}/${file.name}`
}

const updateLoading = ref(false)
async function updateCake() {
    try {
        updateLoading.value = true
        if (imageCake.value) {
            let newFoto = cake.value.foto_kuePath
            let oldFoto = cake.value.foto_kue?.substr(cake.value.foto_kue?.indexOf('cake') + 5)
    
            if(newFoto == oldFoto) {
                const { error } = await supabase.storage.from('cake').update(newFoto, imageCake.value)
                if (error) throw error
            }
    
            else {
                const { error } = await supabase.storage.from('cake').upload(newFoto, imageCake.value)
                if (error) throw error
                if (oldFoto) {
                    const { error } = await supabase.storage.from('cake').remove(oldFoto)
                    if (error) throw error
                }
            }
        }
        const { data: { publicUrl: url } } = supabase.storage.from('cake').getPublicUrl(cake.value.foto_kuePath)
        const { error } = await supabase.from('produk').update({
            nama_kue: cake.value.nama_kue,
            id_kategori: cake.value.kategori.id,
            id_ukuran: cake.value.id_ukuran,
            harga: cake.value.harga,
            stok: cake.value.stok,
            foto_kue: url
        }).eq('id', cakeId)
        if (error) throw error
        navigateTo('/product')
    } catch (error) {
        console.error(error)
        return
    } finally {
        updateLoading.value = false
    }
}

const getCategory = async () => {
    const { data, error } = await supabase.from('kategori').select('*')
    if (data) categories.value = data
}

const getSize = async () => {
    const { data, error } = await supabase.from('ukuran').select('*')
    if (data) sizes.value = data
}

onMounted(() => {
    getCakeId()
    getCategory()
    getSize()
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
                <NuxtLink to="/product/" style="text-decoration: none; color: black;">
                    <i class="bi bi-arrow-left-circle fs-2"></i>
                </NuxtLink>
            </div>
            <div class="col-md-10">
                <h2 class="mt-3 head-tittle">Edit Kue</h2>
            </div>
        </div>

        <div class="row mt-5">
            <form @submit.prevent="updateCake" class="input-group justify-content-center">
                <div class="col-md-5 col-lg-3">
                    <div class="card cake rounded-4 shadow">
                        <div class="card-body text-center">
                            <img :src="cake.foto_kue" alt="img-cake" class="rounded-3">
                            <input type="file" :disabled="!cake.id_kategori" @change="imgPicked" accept="image/*">
                        </div>
                    </div>
                </div>
                <div class="col-md-6 col-lg-4">
                    <div class="card mb-5 form rounded-4 shadow">
                        <div class="card-body m-3">
                            <div class="mb-3">
                                <label for="exampleFormControlInput1" class="form-label">Nama Kue</label>
                                <input v-model="cake.nama_kue" type="text" class="form-control"
                                    id="exampleFormControlInput1">
                            </div>
                            <div class="mb-3">
                                <label for="kategori">Kategori</label>
                                <select v-model="cake.kategori" class="form-control form-select" id="keperluan">
                                    <option v-for="category in categories" :key="category.id" :value="category">
                                        {{ category.nama }}</option>
                                </select>
                            </div>
                            <div class="mb-3">
                                <label for="ukuran">Ukuran</label>
                                <select v-model="cake.id_ukuran" class="form-control form-select" id="keperluan">
                                    <option v-for="(size, i) in sizes" :key="i" :value="size.id">{{ size.nama }}
                                    </option>
                                </select>
                            </div>
                            <div class="mb-3">
                                <label for="exampleFormControlInput1" class="form-label">Harga</label>
                                <input v-model="cake.harga" type="text" class="form-control"
                                    id="exampleFormControlInput1">
                            </div>
                            <div v-if="cake.id_kategori == '4'" class="mb-3">
                                <label for="exampleFormControlInput1" class="form-label">Stok</label>
                                <div class="mb-3 d-flex align-items-center">
                                    <div class="form-check me-5">
                                        <input class="form-check-input" type="radio" name="stok" id="flexRadioDefault1" checked value="true" v-model="cake.stok">
                                        <label class="form-check-label" for="flexRadioDefault1">Stok Tersedia</label>
                                    </div>
                                    <div class="form-check fc-second">
                                        <input class="form-check-input" type="radio" name="stok" id="flexRadioDefault2" value="false" v-model="cake.stok">
                                        <label class="form-check-label" for="flexRadioDefault2">Stok Habis</label>
                                    </div>
                                </div>
                            </div>
                            <div class="text-center mt-4">
                                <button type="submit" :disabled="updateLoading" class="btn rounded-5">Edit</button>
                            </div>
                        </div>
                    </div>
                </div>
            </form>
        </div>

    </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Kavoon&family=Miltonian+Tattoo&family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Red+Rose:wght@300..700&display=swap');


h2,
h6,
input,
label,
.btn {
    font-family: "Poppins", sans-serif;
}


i {
    margin-left: 50px;
}

.btn {
    width: 120px;
    font-size: 20px;
    color: white;
    background-color: #C6AC7B;

}

.cake {
    height: 390px;
}

.form {
    margin-left: 30px;

}

h6 {
    margin-top: 125px;
}

img {
    width: 250px;
}

@media only screen and (min-width: 600px) and (max-width: 890px) {
    .head-tittle {
        margin-left: 30px;
    }
}


@media only screen and (max-width: 600px) {
    .card-body {
        margin: 0;
    }

    .form {
        margin-left: 0;
        margin-top: 20px;
    }

    label, input, select, .btn {
        font-size: small;
    }

    i {
        margin-left: 0;
    }
}

</style>
