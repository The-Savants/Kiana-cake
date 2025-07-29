<template>
   <div class="row footer p-4">
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
                        <h6 class="mt-2">kianacakeandcookies@gmail.com</h6>
                    </div>
                </div>                  
            </div>
        </div> 
</template>

<style scoped>
h4, h6, p {
    font-family: "Poppins", sans-serif;
}

.footer {
    width: 100%;    
    margin: 0;
    border: 2px solid #C6AC7B;
    background-color: #F0EDDC;
}

.address {
    margin-left: 70px;
}

.footer i {
    font-size: 1.5rem;
    margin-left: 20px;
}

.footer h6 {
    margin-top: 15px;
    margin-left: 15px;
}


.footer p {
    margin-top: 15px;
    margin-left: 5px;
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

</style>