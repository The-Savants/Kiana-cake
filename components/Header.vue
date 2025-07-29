<script setup>
const route = useRoute()
const router = useRouter()

const hiddenRoutes = ['login']

const shouldShowNavbar = !hiddenRoutes.includes(route.name)

const isActive = ref('home');

const setActive = (tab) => {
  isActive.value = tab;
};

</script>

<template>
    <nav v-if="route?.name != 'login'" class="navbar navbar-expand-lg">
        <div class="container-fluid">
            <img src="../assets/img/logo-rb.png" alt="logo" class="logo">
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav ms-auto">
                    <li class="nav-item">
                        <nuxt-link to="/" class="nav-link" :class="{ active: isActive === 'home' }" @click="setActive('home')">Home</nuxt-link>
                    </li>
                    <li class="nav-item">
                        <nuxt-link to="/catalogue" class="nav-link" :class="{ active: isActive === 'catalogue' }" @click="setActive('catalogue')">Product</nuxt-link>
                    </li>
                    <li class="nav-item">
                        <nuxt-link to="/about" class="nav-link" :class="{ active: isActive === 'about' }" @click="setActive('about')">About</nuxt-link>
                    </li>
                    <li class="nav-item">
                        <nuxt-link to="/contact" class="nav-link" :class="{ active: isActive === 'contact' }" @click="setActive('contact')">Contact us</nuxt-link>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
</template>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Kavoon&family=Miltonian+Tattoo&family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&family=Red+Rose:wght@300..700&display=swap');

.navbar {
    background-color: #F0EDDC;
}

.nav-item {
    font-size: 1rem;
    margin-right: 2rem;
}

.container-fluid {
    width: 100%;
    padding: 0%;
}

h1 {
    margin-top: 1rem;
    margin-left: 1.5rem;
    font-size: 2rem;

}

.navbar-collapse {
    margin-right: 5rem;
}

i {
    margin-left: 2rem;
}

h1, .nav-item {
    font-family: "Poppins", sans-serif;

}

.logo {
    width: 75px;
    margin-left: 50px;
}

.nav-link.active {
  position: relative;
}

.nav-link.active::after {
  content: '';
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  height: 2px;
  background-color: #000;
}

@media only screen and (max-width: 600px) {
    .logo {
        width: 50px;
        margin-left: 25px;
        margin-top: 5px;
    }
}

@media only screen and (max-width: 890px) {
    .logo {
        width: 60px;
        margin-left: 30px;
        margin-top: 5px;
    }
}
</style>