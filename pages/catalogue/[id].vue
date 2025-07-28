<script setup>
const supabase = useSupabaseClient()
const route = useRoute()
const cake = ref(null)
const categories = ref([])
const sizes = ref([])


const getCakeId = async () => {
    const id = Number(route.params.id)
    if (isNaN(id)) {
        console.error('Invalid ID:', route.params.id)
        return
  }  const { data, error } = await supabase.from('produk').select(`
  *,
  kategori(*),
  ukuran(*)
`)
  .eq('id_kue', id).single()

    console.log('DATA:', data)
    console.log('ERROR:', error)

  if (data) {
    cake.value = data
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

onMounted (() => {
    getCakeId()
    getCategory()
    getSize()
})
</script>

<template>
    <div class="container-fluid pt-4 mb-5">

        <div class="row">
            <div class="col-md-1 mt-2">
                <nuxt-link to="/catalogue" style="text-decoration: none; color: black;">
                    <i class="bi bi-arrow-left-circle fs-2"></i>
                </nuxt-link>
            </div>
            <div class="col-md-10">
                <h2 class="mt-3 head-tittle">Detail</h2>
            </div>
        </div>

        <div v-if="cake">
            <div class="row mt-5 justify-content-center">
                <div class="col-5 col-lg-3 d-flex ">
                    <div class="card card-img flex-fill cake rounded-4 shadow">
                        <div class="card-body text-center p-3">
                            <img :src="cake.foto_kue" alt="img-cake" class="card-img-top rounded-3">
                        </div>
                    </div>
                </div>
                <div class="col-7 col-lg-5">
                    <div class="card mb-5 rounded-4">
                        <div class="card-body">
                            <ul>
                            <li class="list-group-item name fw-semibold">{{ cake?.nama_kue }}</li>
                            <li class="list-group-item">{{ cake?.kategori?.nama }}</li>
                            <li class="list-group-item">Ukuran {{ cake?.ukuran?.nama }}</li>
                            <li class="list-group-item" v-if="cake.id_kategori !== 3 && cake.id_kategori !== 4">Free lilin</li>
                            <li class="list-group-item price fw-bold">{{ cake?.harga }}</li>
                            </ul>
                            <div class="row button mt-4">
                                <div class="col-md-6">
                                    <nuxt-link href="https://wa.me/6281224703915" style="text-decoration: none; color: black;" target="_blank">
                                        <button class="btn pesan rounded-5">Pesan via Whatsapp</button>
                                    </nuxt-link>
                                </div>
                                <div v-if="cake && cake.id_kue" class="col-md-6">
                                    <nuxt-link :to="`/form/${cake.id_kue}`">
                                        <button class="btn btn-pesan rounded-5">Pesan sekarang</button>
                                    </nuxt-link>
                                </div>
                            </div>
                                
                        </div>
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Kavoon&family=Miltonian+Tattoo&family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Red+Rose:wght@300..700&display=swap');


h2, h6, li, .btn {
    font-family: "Poppins", sans-serif;
}

.list-group-item {
    font-size: 20px;
    margin-top: 10px;
}

.name {
    font-size: 25px
}

i {
    margin-left: 50px;
}

.price {
    margin-top: 30px;
    font-size: 25px;
}

h6 {
    margin-top: 125px;
}

.pesan {
    width: 225px;
    border: 3px solid #C6AC7B;
}

.btn-pesan {
    width: 200px;
    color: white;
    font-weight: 500;
    background-color: #C6AC7B;
}


@media only screen and (min-width: 600px) and (max-width: 890px) {
    .head-tittle {
        margin-left: 30px;
    }
}


@media only screen and (max-width: 600px) {
    i {
        margin-left: 0;
    }

    .card-body li {
        font-size: small;
    }

    .btn {
        width: 120px;
        font-size: 0.6rem;
    }

    .btn-pesan {
        margin-top: 10px;
    }

    .card-img {
        height: 185px
    }

    .col-md-6 {
        margin-left: 25px;
    }

}

</style>