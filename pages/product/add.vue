<script setup>
const supabase = useSupabaseClient()
const categories = ref([])
const kategori = ref()
const files = ref()
const sizes = ref([])
const url_img = `https://qxiqnifqwadzkzpshvqq.supabase.co/storage/v1/object/public/cake/`
const form = ref({
    nama_kue: "",
    id_kategori: "",
    id_ukuran: "",
    harga: "",
    stok: null,
    foto_kue: null
})

const isLoading = ref(false)

const getCategory = async () => {
    const { data, error } = await supabase.from('kategori').select('*')
    if (data) categories.value = data
}

const getSizes = async () => {
    const { data, error } = await supabase.from('ukuran').select('*')
    if (data) sizes.value = data
}


async function saveToTable() {
    let file = files.value[0]
    const fileExt = file.name.split('.').pop()
    const fileName = `${Math.random()}.${fileExt}`
    const filePath = `${kategori.value}/${fileName}`
    isLoading.value = true

    const { data, error } = await supabase.storage.from('cake').upload(filePath, file)
    if (data) {
        console.log("upload success")
        const stockValue = form.value.stok === 'true' // Convert string to boolean
        const { data, error } = await supabase.from('produk')
            .insert([{
                nama_kue: form.value.nama_kue,
                id_kategori: form.value.id_kategori,
                id_ukuran: form.value.id_ukuran,
                harga: form.value.harga,
                stok: stockValue,
                foto_kue: url_img + kategori.value + '/' + fileName
            }])
        if (error) throw error
        if (data) {
            console.log("succes: saved to table")
        }
    }
    if (error) throw error
    else {
        isLoading.value = false;
        navigateTo("/product/") 
    }
}


onMounted(() => {
    getCategory()
    getSizes()
})



definePageMeta({
    layout: 'admin',
    middleware: 'auth',
})

</script>


<template>
    <div class="container-fluid pt-4 mb-5">

        <div class="row">
            <div class="col-md-1 mt-2">
                <nuxt-link to="/product" style="text-decoration: none; color: black;">
                    <i class="bi bi-arrow-left-circle fs-2"></i>
                </nuxt-link>
            </div>
            <div class="col-md-10">
                <h2 class="mt-3 head-tittle">Tambah Kue</h2>
            </div>
            <p v-if="isLoading" class="text-secondary">Loading...</p>
        </div>

        <div class="row mt-5">
            <form @submit.prevent="saveToTable" class="input-group justify-content-center">
                <div class="col-md-5 col-lg-3">
                    <div class="card add-img rounded-4 shadow">
                        <div class="card-body text-center align-items-center p-3">
                            <h6>Tambahkan foto kue disini</h6>
                            <input type="file" @change="(e) => files = e.target.files" accept="image/*">
                        </div>
                    </div>
                </div>
                <div class="col-md-6 col-lg-4">
                    <div class="card cake rounded-4 shadow">
                        <div class="card-body m-3">
                            <div class="mb-3">
                                <label for="exampleFormControlInput1" class="form-label">Nama Kue</label>
                                <input v-model="form.nama_kue" type="text" class="form-control"
                                id="exampleFormControlInput1">
                            </div>
                            <div class="mb-3">
                                <label for="kategori">Kategori File</label>
                                <select v-model="kategori" class="form-control form-select">
                                    <option value="birthday-cake">Birthday Cake</option>
                                    <option value="bolu-jadoel">Bolu Jadoel</option>
                                    <option value="dessert">Dessert</option>
                                    <option value="wedding-cake">Wedding Cake</option>
                                </select>
                            </div>
                            <div class="mb-3 kategori">
                                <label for="kategori">Kategori Kue</label>
                                <select v-model="form.id_kategori" class="form-control form-select" id="kategori">
                                    <option v-for="(category, i) in categories" :key="i" :value="category.id">{{
                                        category.nama }}</option>
                                </select>
                            </div>
                            <div class="mb-3 ukuran">
                                <label for="ukuran">Ukuran</label>
                                <select v-model="form.id_ukuran" class="form-control form-select" id="keperluan">
                                    <option v-for="(size, i) in sizes" :key="i" :value="size.id">{{ size.nama }}
                                    </option>
                                </select>
                            </div>
                            <div class="mb-3">
                                <label for="exampleFormControlInput1" class="form-label">Harga</label>
                                <input v-model="form.harga" type="text" class="form-control"
                                id="exampleFormControlInput1">
                            </div>
                            <div v-if="form.id_kategori == '4'" class="mb-3">
                                <label for="exampleFormControlInput1" class="form-label">Stok</label>
                                <div class="mb-3 d-flex align-items-center">
                                    <div class="form-check me-5">
                                        <input class="form-check-input" type="radio" name="stok" id="flexRadioDefault1" checked value="true" v-model="form.stok">
                                        <label class="form-check-label" for="flexRadioDefault1">Stok Tersedia</label>
                                    </div>
                                    <div class="form-check fc-second">
                                        <input class="form-check-input" type="radio" name="stok" id="flexRadioDefault2" value="false" v-model="form.stok">
                                        <label class="form-check-label" for="flexRadioDefault2">Stok Habis</label>
                                    </div>
                                </div>
                            </div>
                            <div class="text-center mt-3">
                                <button type="submit" class="btn rounded-5">Tambah</button>
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
    color: white;
    background-color: #C6AC7B;
}

.cake {
    margin-left: 30px;

}

.add-img {
    height: 350px;
}

.add-img .card-body {
    margin-top: 120px;
}

.kategori,
.ukuran {
    width: 100%;
}

@media only screen and (min-width: 600px) and (max-width: 890px) {
    .head-tittle {
        margin-left: 30px;
    }
}

@media only screen and (max-width: 600px) {
    .add-img {
        margin: 0px;;
    }

    .cake {
        margin-left: 0px;
        margin-top: 20px;

    }

    .add-img {
        height: 200px;
    }

    .add-img .card-body {
        margin-top: 50px;
    }

    h6,
    input,
    label,
    .btn {
        font-size: small;
    }

    i {
        margin-left: 0;
    }

}

</style>