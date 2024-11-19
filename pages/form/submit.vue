<template>
    <div class="container-fluid pt-4 mb-5">

        <div class="row">
            <div class="col mt-2">
                <nuxt-link to="/" style="text-decoration: none; color: black;">
                    <i class="bi bi-arrow-left-circle fs-2"></i>
                </nuxt-link>
            </div>
        </div>

        <div class="text-center mt-2">
            <h3 class="fw-semibold">Pesanan terkirim.</h3>
            <h5 class="mt-4">Silahkan menunggu pesan konfirmasi dari penjual (maks. 24 jam), atau</h5> 
            <nuxt-link href="https://wa.me/6281224703915" style="text-decoration: none; color: black;" target="_blank">
                <button class="btn rounded-5 mt-2">Hubungi Penjual</button>
            </nuxt-link>
        </div>
    </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Kavoon&family=Miltonian+Tattoo&family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Red+Rose:wght@300..700&display=swap');


h3, h5, input, .btn {
    font-family: "Poppins", sans-serif;
}

.btn {
    font-size: 18px;
    width: 200px;
    color: white;
    background-color: #c6ac7b;
}

i {
    margin-left: 50px;
}

@media only screen and (max-width: 600px) {

    h5, .btn {
        font-size: small;
    }

    h3 {
        font-size: medium;
    }

    i {
        margin-left: 0;
    }

}

</style>