<script setup>
import emailjs from 'emailjs-com'

const formData = ref ({
    name: '',
    no_hp: '',
    email: '',
    message: '',
})

const isSuccess = ref(false)
const isError = ref(false)
const isLoading = ref(false)


const sendEmail = async () => {
    isLoading.value = true

    const serviceID = 'service_7g828la'
    const templateID = 'template_h4cxosb'
    const userID = "Wiorh2ygVXyiBn6Vf"

    emailjs.send(serviceID, templateID, {
        form_name: formData.value.name,
        no_hp: formData.value.no_hp,
        email: formData.value.email,
        message: formData.value.message
    }, userID)
    .then (() => {
        isSuccess.value = true
        isError.value = false
        formData.value = { name: '', no_hp: '', email: '', message: '' }
        setTimeout(() => {
            isSuccess.value = false
        }, 4000)
    })
    .catch (() => {
        isError.value = true
        setTimeout(() => {
            isSuccess.value = false
        }, 4000)
    })
    .finally(() => {
        isLoading.value = false
    })
}
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
                <h2 class="mt-3 head-tittle">Contact Us</h2>
            </div>
        </div>

        <!--contact-->
        <div class="row contact">
            <div class="col p-5">
                <div class="card rounded-3">
                    <div class="row">
                        <div class="col-md-6 p-5">
                            <div class="p-3">
                                <h2>Hubungi Kami</h2>
                            <h6 class="mt-3">Melayani pertanyaan dan permasalahan customer 24jam</h6>
                            </div>
                            <p v-if="isSuccess">Pesan terkirim</p>
                            <p v-if="isError">Pesan gagal terkirim</p>
                            <p v-if="isLoading">Mengirim pesan..</p>
                            <form @submit.prevent="sendEmail" class="p-3">
                                <div class="mb-3">
                                    <label for="exampleInput" class="form-label">Nama</label>
                                    <input v-model="formData.name" type="text" class="form-control">
                                </div>
                                <div class="mb-3">
                                    <label for="exampleInput" class="form-label">E - mail</label>
                                    <input v-model="formData.email" type="email" class="form-control">
                                </div>
                                <div class="mb-3">
                                    <label for="exampleInput" class="form-label">Pesan atau pertanyaan</label>
                                    <textarea v-model="formData.message" class="form-control"></textarea>
                                </div>
                                <div class="text-center">
                                    <input type="submit" class="btn mt-4 rounded-5" />
                                </div>
                            </form>
                        </div>
                        <div class="col-md-6">
                            <div class="card">
                                <div class="card-body">
                                    
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

h5, h6, label, .btn, .contact h2 {
    font-family: "Poppins", sans-serif;
}

.card-body {
  background: url('~/assets/img/bgcake-contact.jpg') no-repeat center center;
  background-size: cover;
  height: 650px;
}

.btn {
    width: 100px;
    font-size: 17px;
    color: white;
    background-color: #C6AC7B;
}

i {
    margin-left: 50px;
}

textarea {
    height: 100px;
}

input, textarea, .card {
    border: 1px solid #C6AC7B;
}


@media only screen and (min-width: 600px) and (max-width: 890px) {
    .head-tittle {
        margin-left: 30px;
    }

    .card-body {
        height: 670px;
    }
}

@media only screen and (max-width: 600px) {
    i {
        margin-left: 0;
    }

    .contact {
        font-size: small;
    }

    .card-body {        
        height: 450px;
    }

}
</style>