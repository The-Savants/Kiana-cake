<template>
    <div class="container-fluid pt-4 mb-5">
        
        <div class="row">
            <div class="col-md-1 mt-2">
                <nuxt-link to="/" style="text-decoration: none; color: black;">
                    <i class="bi bi-arrow-left-circle fs-2"></i>
                </nuxt-link>
            </div>
            <div class="col-md-10">
                <h2 class="mt-3 head-tittle">About</h2>
            </div>
        </div>

        <!--About-->
        <div class="row bg mt-5 justify-content-center">
            <div class="col-lg-5 col-md-5 content">
                <div class="tittle">
                    <h3>Kiana Cake and Cookies</h3>
                    <h5>Cake for every moment</h5>
                </div>
                <h6 class="text-about mt-5">Sebuah bisnis rumahan bidang cake and cookies yang mulai berdiri 
                    pada tahun 2019. Menyediakan produk kue dengan berbagai ukuran dan harga. Kami selalu berusaha untuk dapat dapat memberikan hasil karya 
                    kue yang menarik, enak dan memenuhi keinginan customer.</h6>
            </div>
            <div class="col-lg-4 col-md-5 cake">
                <div class="card card-custom rounded-4">
                    
                </div>
            </div>    
        </div>

    </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Junge&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Kavoon&family=Miltonian+Tattoo&family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Red+Rose:wght@300..700&display=swap');

.card {
  background: url('~/assets/img/img-about.png') no-repeat center center;
  background-size: cover;
  height: 350px;
}

i {
    margin-left: 50px;
}

h2 {
    font-family: "Junge", cursive;
}

h5 {
    margin-right: 50px;
}

h5, h6 {
    font-family: "Poppins", sans-serif;
}

.content h3 {
  font-family: "Berkshire Swash", serif;
  font-weight: 400;
  font-style: normal;
}

.content h5 {
  font-family: "Charmonman", cursive;
  font-weight: 400;
  font-style: normal;
}

.img-logo {
    width: 260px;
    margin: auto;
    margin-top: 10px;
}

.img-owner, .img-kitchen, .img-product {
    margin: auto;
    width: 275px;
}

.img-product {
    height: 275px;
}

.card {
    border: none;
}



@media only screen and (max-width: 600px) {
    .tittle {
        margin-left: 20px;
    }

    .text-about {
        margin: 10px 20px;    
    }

    .card {
        height: 250px;
        margin: 10px;
    }
    
    i {
        margin-left: 0;
    }
}

@media only screen and (min-width: 600px) and (max-width: 890px) {
    .head-tittle {
        margin-left: 30px;
    }
}
</style>