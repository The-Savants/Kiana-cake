<script setup>
const supabase = useSupabaseClient()
const keyword = ref('')
const cakes = ref([])
const isCake = ref() 
const categories = ref([])


const getCake = async () => {
  const { data, error } = await supabase.from('produk').select(`*, kategori(*)`)
  .ilike('nama_kue', `%${keyword.value}%`)
  if (data) {
    cakes.value = data
    }
}

const getCategory = async () => {
    const { data, error } = await supabase.from('kategori').select('*')
    if (data) categories.value = data
}

const cakeFiltered = computed(() => {
  return cakes.value.filter((c) => {
    return (
      c.nama_kue?.toLowerCase().includes(keyword.value.toLowerCase()) ||
      c.harga?.toLowerCase().includes(keyword.value.toLowerCase())
    )
  })
})

const fetchProducts = async (categoryId) => {
  const { data, error } = await supabase
    .from('produk')
    .select('*')
    .eq('id_kategori', categoryId);
  
  if (data) {
    cakes.value = data
  }

}

const checkStock = (cake, e) => {
    if (!cake.stok) {
        e.preventDefault()
        alert('Stok habis! Anda tidak dapat membeli produk ini.')
  }
}


onMounted (() => {
    getCake()
    getCategory()
})
</script>

<template>
    <div class="container-fluid pt-4 mb-5">

        <div class="row">
            <div class="col-md-1 mt-2">
                <nuxt-link to="/" style="text-decoration: none; color: black;">
                    <i class="bi bi-arrow-left-circle fs-2"></i>
                </nuxt-link>
            </div>
            <div class="col-md-10">
                <h2 class="mt-3 head-tittle">Product</h2>
            </div>
        </div>

        <!--Categories-->
        <div class="row categories mt-5 mx-5 justify-content-center">
            <div v-for="category in categories" :key="category.id" class="col-6 col-md-2 text-center cursor" @click="fetchProducts(category.id)">
                <img :src="category.icon" alt="img-kategori">
                <h6 class="mt-2">{{ category.nama }}</h6>
            </div>
            <div class="col-md-2 text-center">
                <img src="~/assets/img/custom.png" alt="img-kategori">
                <nuxt-link to="/custom" style="text-decoration: none;">
                    <h6 class="mt-2 custom rounded-5">Custom cake</h6>
                </nuxt-link>
            </div>
        </div>

        <!--Search-->
        <div class="row mt-5 d-flex justify-content-center">
            <div class="col-10 col-md-7">
                <form @submit.prevent="getCake" class="input-group flex-nowrap">
                    <input v-model="keyword" type="text" class="form-control shadow rounded-5" placeholder="Cari nama atau harga kue" aria-label="Search"
                        aria-describedby="search-addon" />
                </form>
            </div>
        </div>

        <!--Catalogue-->
        <div class="catalogue p-5">
                <div class="row">
                    <div v-for="(cake, i) in cakeFiltered" :key="i" class="col-6 col-md-4 col-lg-3 d-flex mt-4">
                    <div class="card flex-fill rounded-4 shadow p-3">
                          <div v-if="cake">
                            <img :src="cake.foto_kue" alt="img-cake" class="card-img-top">
                            <div class="card-body">
                                <h5 class="mt-3">{{ cake.nama_kue }}</h5>
                                <h3 class="fw-bold">{{ cake.harga }}</h3>
                                <div class="text-end my-2">
                                    <!-- <NuxtLink :to="`/catalogue/${cake.id}`">Lihat Detail</NuxtLink> -->

                                    <NuxtLink 
                                    :to="cake.stok ? `/catalogue/${cake.id_kue}` : '#'"
                                    class="btn mt-3 rounded-5"
                                    :class="{ disabled: !cake.stok }"
                                    @click="checkStock(cake)">
                                    {{ cake.stok ? 'Beli' : 'Habis' }}
                                    </NuxtLink>
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
@import url('https://fonts.googleapis.com/css2?family=Junge&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Kavoon&family=Miltonian+Tattoo&family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Red+Rose:wght@300..700&display=swap');


h2 {
    font-family: "Junge", cursive;
}

h3, h5, h6, input, .btn {
    font-family: "Poppins", sans-serif;
}

i {
    margin-left: 50px;
}

input {
    font-size: 14px;
    height: 45px;
    border: 1px solid #C6AC7B;
}

.form-control {
  border-right: none;
}

.custom {
    background-color: #C6AC7B;
    color: white;
    height: 20px;
}

.btn {
    width: 100px;
    /* margin-right: 10px; */
    font-size: 17px;
    color: white;
    background-color: #C6AC7B
}

/* .cake {
    padding: 50px;
} */

.categories img {
    width: 100px;
}

.card {
    border: 1px solid #C6AC7B;
}

.cursor {
    cursor: pointer;
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

    .catalogue h3, h5 {
        font-size: small;
    }

    .btn {
        width: 75px;
        font-size: small;
    }

    .card-dessert {
        width: 150px;
    }

    .catalogue h2 {
        margin-left: 50px;
        margin-top: 40px;
    }
    .card-body {
        padding: 0;
    }

    .categories img {
        width: 75px;
    }

    .categories h6 {
        font-size: small;
    }
    

}

</style>