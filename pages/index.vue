<script setup>
const supabase = useSupabaseClient()
const cakes = ref([])
const desserts = ref([])


const fetchDesserts = async () => {
  const { data, error } = await supabase.from('produk').select(`*, kategori(*)`).eq('id_kategori', '4')
  if (data) {
    desserts.value = data
    }
}

const getCake = async () => {
  const { data, error } = await supabase.from('produk').select(`*, kategori(*)`).limit(4)
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
    fetchDesserts()
    getCake()
})
</script>


<template>
    <div class="container-fluid p-0">
        
        <div class="container-fluid cover p-0">
            <div class="row">
                <div class="col">
                    <div class="text-center">
                        <h1 class="mt-5">Kiana Cake and Cookies</h1>
                        <h3 class="text mt-4">Cake for every moment</h3>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="catalogue p-md-5 p-lg-5">
            <h2>Product</h2>

            <!--Dessert-->
            <div class="dessert mt-5">
                <div class="row justify-content-center">
                    <div class="col-lg-3">
                        <div class="text-center">
                            <h4>Dessert</h4>
                            <hr class="mt-3">
                        </div>
                    </div>
                </div>
            </div>

            <div class="product p-4">
                <div v-if="desserts.length" class="grid-container m-4">
                    <div v-for="produk in desserts" :key="produk.i" class="card card-dessert rounded-4 shadow p-3">
                        <img :src="produk.foto_kue" alt="img-cake" class="card-img-top">
                        <div class="card-body">
                            <h5 class="mt-3">{{ produk.nama_kue }}</h5>
                            <h3 class="fw-bold">{{ produk.harga }}</h3>
                            <div class="text-end my-2">
                                <NuxtLink 
                                :to="produk.stok ? `/catalogue/${produk.id}` : '#'"
                                class="btn mt-3 rounded-5"
                                :class="{ disabled: !produk.stok }"
                                @click="checkStock(product)">
                                {{ produk.stok ? 'Beli' : 'Habis' }}
                                </NuxtLink>                        
                            </div>
                        </div>
                    </div>
                </div>
                <p v-else>Tidak ada produk dalam kategori dessert.</p>
            </div>

            <!--Cake-->
            <div class="cake mt-5">
                <div class="row justify-content-center">
                    <div class="col-lg-3">
                        <div class="text-center">
                            <h4>Cake</h4>
                            <hr class="mt-3">
                        </div>
                    </div>
                </div>
            </div>

            <div class="row product gy-5">
                <div v-for="(cake, i) in cakes" :key="i" class="col-6 col-md-6 col-lg-3 d-flex">
                    <div class="card flex-fill rounded-4 shadow p-3 d-flex">
                        <img :src="cake.foto_kue" alt="img-cake" class="card-img-top">
                        <div class="card-body">
                            <h5 class="mt-3">{{ cake.nama_kue }}</h5>
                            <h3 class="fw-bold">{{ cake.harga }}</h3>
                            <div class="text-end my-2">
                                <nuxt-link :to="`/catalogue/${cake.id}`">
                                    <button class="btn mt-3 rounded-5">Beli</button>
                                </nuxt-link>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="text-center mt-3">
                <nuxt-link to="/catalogue">
                    <button class="btn seemore rounded-5">See more</button>
                </nuxt-link>
            </div>
        </div>


        <!--Custom-->
        <div class="container-custom p-4 rounded-5">

            <div class="text-center">
                <h3 class="mt-3">Custom Your Cake</h3>
            </div>

            <div class="row costum p-5 justify-content-center">
                <div class="col-lg-3 col-md-4 d-flex">
                    <div class="card flex-fill card-custom rounded-4">
                        <div class="card-body text-center">
                            <img src="../assets/img/bgcake-custom.png" alt="img-custom" class="card-img-top">
                        </div>
                    </div>
                </div>
                <div class="col-lg-5 col-md-6 text-costum">
                    <h5 class="fw-bold">Custom kue keinginanmu, </h5>
                    <h5>agar moment perayaan terasa lebih bermakna</h5>
                    <nuxt-link href="https://wa.me/6281224703915" style="text-decoration: none; color: black;" target="_blank">
                        <button class="btn pesan rounded-5">Pesan via Whatsapp</button>
                    </nuxt-link>
                </div>
            </div>
        </div>

        <!--About-->
        <div class="p-5">
            <h2>About</h2>
        </div>
        <div class="row justify-content-center">
            <div class="col-lg-5 col-md-5">
                <div class="tittle">
                    <h3>Kiana Cake and Cookies</h3>
                    <h5>Cake for every moment</h5>
                </div>
                <h6 class="text-about mt-4">Sebuah bisnis rumahan bidang cake and cookies yang mulai berdiri 
                    pada tahun 2019. Menyediakan produk kue dengan berbagai ukuran dan harga. Kami selalu berusaha untuk dapat dapat memberikan hasil karya 
                    kue yang menarik, enak dan memenuhi keinginan customer.</h6>
            </div>
            <div class="col-lg-4 col-md-5 cake">
                <div class="card card-about rounded-4">
                    <div class="card-body">

                    </div>
                </div>
            </div>  
        </div>

        <!--Footer-->
        <div class="row footer p-4 mt-5">
            <div class="col-lg-1">
                <img src="../assets/img/logo-rb.png" alt="img-logo" class="logo">
            </div>
            <div class="col-lg-3">
                <h3>Kiana Cake and Cookies</h3>
                <h5>Cake for every moment</h5>
            </div>
            <div class="col-lg-4">
                <h4 class="address">Alamat</h4>
                <hr>
                <div class="d-flex align-items-center">
                        <i class="bi bi-geo-alt-fill"></i>
                        <h6>Jl. H. Oce Sk, Sukanagara, Purbaratu, Kota Tasikmalaya</h6>
                </div>
            </div>
            <div class="col-lg-4">
                <h4>Social Media</h4>
                <hr>
                <div class="socialmedia mt">
                    <nuxt-link href="https://wa.me/6281224703915" style="text-decoration: none; color: black;" target="_blank">
                        <i class="bi bi-whatsapp"></i>
                    </nuxt-link>
                    <nuxt-link href="https://www.facebook.com/kiana.arrabella.9" style="text-decoration: none; color: black;" target="_blank">
                        <i class="bi bi-facebook"></i>
                    </nuxt-link>
                    <nuxt-link href="https://www.instagram.com/kianacakeandcookies/profilecard/?igsh=dGI2Z3kzNnQ1dXl4" style="text-decoration: none; color: black;" target="_blank">
                        <i class="bi bi-instagram"></i>  
                    </nuxt-link>                             
                    <div class="d-flex align-items-center mt-2">
                        <i class="bi bi-envelope-at"></i>
                        <span><h6>kianacakeandcookies@gmail.com</h6></span>
                    </div>
                </div>                  
            </div>
        </div>

        <!--whatsapp-->
            <nuxt-link class="btn-wa fixed-bottom text-center" href="https://wa.me/6281224703915" style="text-decoration: none; color: white;" target="_blank">
                <i class="bi bi-whatsapp fs-3"></i>
            </nuxt-link>
        

    </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Berkshire+Swash&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Berkshire+Swash&family=Charmonman:wght@400;700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Kavoon&family=Miltonian+Tattoo&family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Red+Rose:wght@300..700&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Junge&display=swap');


.cover {
  background: url('../assets/img/bg-cake-rb.png') no-repeat center center;
  background-size: cover;
  background-color: #F0EDDC;
  height: 550px;
  width: 100%;
}

.card-about {
  background: url('~/assets/img/img-about.png') no-repeat center center;
  background-size: cover;
  height: 350px;
}

.tittle h3 {
  font-family: "Berkshire Swash", serif;
  font-weight: 400;
  font-style: normal;
}

.tittle h5 {
  font-family: "Charmonman", cursive;
  font-weight: 400;
  font-style: normal;
}

h1 {
  font-family: "Berkshire Swash", serif;
  font-weight: 400;
  font-style: normal;
  color: #c6ac7b;
}

h2 {
  font-family: "Junge", cursive;
  font-weight: 400;
  font-style: normal;
}

.text{
  font-family: "Charmonman", cursive;
  font-weight: 400;
  font-style: normal;
}

h3, h4, h5, h6, .btn  {
    font-family: "Poppins", sans-serif;
}

.row {
    margin-right: 0;
}

.btn {
    width: 100px;
    font-size: 17px;
    color: white;
    background-color: #C6AC7B;
}

.product {
    padding: 50px;
}

.content {
    margin-right: 50px;
}

.text-costum {
    margin-left: 20px;
}

.pesan {
    width: 225px;
    margin-top: 50px;
    margin-left: 20px;
    background-color: white;
    color: black;
    border: 3px solid #C6AC7B;
}

.footer {
    border: 2px solid #C6AC7B;
    background-color: #F0EDDC;
}

.address {
    margin-left: 70px;
}

.footer i {
    font-size: 25px;
    margin-left: 20px;
}

.footer h6 {
    margin-top: 15px;
    margin-left: 10px;
}

.footer h3 {
  font-family: "Berkshire Swash", serif;
  font-weight: 400;
  font-style: normal;
}

.footer h5 {
  font-family: "Charmonman", cursive;
  font-weight: 400;
  font-style: normal;
}

.img-logo {
    width: 170px;
    margin: auto;
    margin-top: 10px;
}

.footer hr {
    color: #C6AC7B;
}

.card {
    border: 1px solid #C6AC7B;
}

.footer img {
    width: 100px;
}

.footer h3, .footer h5 {
    margin-left: 20px;
}

.footer span {
    margin-left: 15px;
}

.seemore {
    font-size: 20px;
    width: 200px;
    font-weight: 500;
    color: white;
}

.container-custom {
    background-color: #F0EDDC;
}

.grid-container {
  display: grid;
  grid-auto-flow: column;
  grid-auto-columns: minmax(18rem, 1fr);
  gap: 1rem;
  overflow-x: auto;
  white-space: nowrap;
  -ms-overflow-style: none;
  scrollbar-width: none;
}

.btn-wa {
    background-color: green;
    width: 2.5rem;
    border-radius: 100px;
    margin-bottom: 50px;
    margin-left: 1000px;
}

.card-dessert {
    width: 270px;
}

@media only screen and (min-width: 600px) and (max-width: 890px) {
    .footer {
        font-size: small;
    }

    .footer img {
        width: 75px;
    }

    .footer h3 {
        margin-top: 10px;
    }

    .footer h3, .footer h5, .footer h4 {
        margin-left: 0px;
    }

    .footer h4 {
        margin-top: 20px;
        font-size: 1.5rem;
    }

    .address {
        margin-left: 0px;
    }

    .footer i {
        font-size: 1.5rem;
        margin-left: 10px;
    }

    .footer h6 {
        margin-left: 10px;
        margin-top: 10px;
    }

    .footer span {
        margin-left: 3px;
    }

}

@media only screen and (max-width: 600px) {
    
    .product h3, h5 {
        font-size: small;
    }

    .btn {
        width: 75px;
        font-size: small;
    }

    .seemore {
        width: 100px;
        margin-bottom: 20px;
    }

    .pesan {
        width: 200px;

    }

    .card-dessert {
        width: 150px;
    }

    .grid-container {
        grid-auto-columns: minmax(10rem, 1fr);
        gap: 0.5rem;
    }

    .catalogue h2 {
        margin-left: 50px;
        margin-top: 40px;
    }

    .text-costum {
        margin-top: 15px;
    }

    .tittle {
        margin-left: 20px;
    }

    .text-about {
        margin: 10px 20px;    
    }

    .card-about {
        height: 250px;
        margin: 10px;
    }

    .footer {
        font-size: small;
    }

    .footer img {
        width: 50px;
    }

    .footer h3 {
        margin-top: 10px;
    }

    .footer h3, .footer h5, .footer h4 {
        margin-left: 0px;
    }

    .footer h4 {
        margin-top: 20px;
        font-size: 1rem;
    }

    .address {
        margin-left: 0px;
    }

    .footer i {
        font-size: 1rem;
        margin-left: 10px;
    }

    .footer h6 {
        margin-left: 10px;
        margin-top: 10px;
    }

    .footer span {
        margin-left: 3px;
    }

}

</style>