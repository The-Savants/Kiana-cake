<template>
    <div class="container-fluid pt-4 mb-5">

        <div class="row">
            <div class="col-lg-1 mt-2">
                <nuxt-link to="/" style="text-decoration: none; color: black;">
                    <i class="bi bi-arrow-left-circle fs-2"></i>
                </nuxt-link>
            </div>
            <div class="col-lg-10 text-center">
                <h3 class="mt-3">Costum Your Cake</h3>
            </div>
        </div>

        <div class="row costum mt-3 p-5 justify-content-center">
                <div class="col-lg-3 col-md-4 d-flex">
                    <div class="card flex-fill card-custom rounded-4">
                        <div class="card-body text-center">
                            <img src="~/assets/img/bgcake-custom.png" alt="img-custom" class="card-img-top">
                        </div>
                    </div>
                </div>
                <div class="col-lg-5 col-md-6 text-costum">
                    <h5 class="fw-bold">Custom kue keinginanmu, </h5>
                    <h5>agar moment perayaan terasa lebih bermakna</h5>
                    <nuxt-link href="https://wa.me/6281224703915" style="text-decoration: none; color: black;">
                        <button class="btn pesan rounded-5">Pesan via Whatsapp</button>
                    </nuxt-link>
                </div>
        </div>

    </div>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Kavoon&family=Miltonian+Tattoo&family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Red+Rose:wght@300..700&display=swap');


h3, h5, .btn {
    font-family: "Poppins", sans-serif;
}

i {
    margin-left: 50px;
}

.text-costum {
    margin-left: 20px;
}

.card-custom {
    border: 1px solid #C6AC7B;
}


.pesan {
    width: 225px;
    margin-top: 50px;
    margin-left: 20px;
    background-color: white;
    color: black;
    border: 3px solid #C6AC7B;
}

.container-fluid {
    background-color: #F0EDDC;
}

.content {
    margin-left: 50px;
}

@media only screen and (max-width: 600px) {

    .btn {
        width: 75px;
        font-size: small;
    }

    .pesan {
        width: 200px;

    }

    .text-costum {
        margin-top: 15px;
    }

}

</style>