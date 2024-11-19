<script setup>
const supabase = useSupabaseClient()
const selectCustomers = ref([])
const cakes = ref([])
const idOrder = ref()
const customers = ref([])

function toggleDelete(customer) {
    selectCustomers.value = customer
}

async function getCustomer() {
    const { data, error } = await supabase.from('customer').select(`*, produk(*)`).order("created_at", { ascending: false })
    if(data) customers.value = data
}

async function markAsCompleted () {
    console.log(idOrder.value)
    const { data, error } = await supabase.from("customer").update({status: true}).eq("id", idOrder.value)
    if (data) getCustomer()
}

const getCake = async () => {
  const { data, error } = await supabase.from('produk').select(`*, kategori(*)`)
  if (data) {
    cakes.value = data
    }
}

const deleteCustomer = async (id) => {
    console.log(id)
    const { error } = await supabase.from('customer').delete().eq('id', id)
    if (error) throw error
    else getCustomer()
}

onMounted (() => {
    getCake()
    getCustomer()
    
    const channels = supabase.channel('custom-update-channel')
    .on(
        'postgres_changes',
        { event: 'UPDATE', schema: 'public', table: 'customer' },
        (payload) => {
        getCustomer()
        }
    )
    .subscribe()
})

definePageMeta({
    layout: 'admin',
    middleware: 'auth'
})

</script>

<template>
    <div class="container-fluid pt-4 mb-5">
        
        <div class="row">
            <div class="col-md-1 mt-2">
                <nuxt-link to="/dashboard" style="text-decoration: none; color: black;">
                    <i class="bi bi-arrow-left-circle fs-2"></i>
                </nuxt-link>
            </div>
            <div class="col-md-10">
                <h2 class="mt-3 head-tittle">Daftar Pesanan</h2>
            </div>
        </div>

        <div class="row mt-5 justify-content-center">
            <div v-for="(customer, i) in customers" :key="i" class="col-md-8 col-lg-5">  
                <div class="card add mt-3 rounded-4 shadow">
                    <div class="card-body">
                        <div class="row p-3">
                            <div class="col-md-7">
                                <ul>
                                    <li class="list-group-item name fw-semibold">{{ customer.nama_pembeli }}</li>
                                    <li class="list-group-item">{{ customer.alamat_lengkap }}</li>
                                    <li class="list-group-item">{{ customer.no_hp }}</li>
                                    <li class="list-group-item cake fw-semibold">{{ customer.produk?.nama_kue }}</li>
                                </ul>
                            </div>
                            <div class="col-md-4">
                                <img :src="customer.produk?.foto_kue" alt="img-cake" class="rounded-3">
                            </div>
                        </div>
                        <div class="text-center mt-3 d-flex justify-content-around">
                                <button class="btn complited rounded-5" data-bs-toggle="modal" data-bs-target="#complitedModal" @click="() => idOrder=customer.id" :disabled="customer.status">Tandai selesai</button>
                                <button class="btn btn-danger rounded-5" data-bs-toggle="modal" data-bs-target="#deleteModal" @click="toggleDelete(customer)">Batalkan pesanan</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <!--Delete Modal-->
        <div class="modal fade center" id="deleteModal" tabindex="-1" aria-labelledby="deleteModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered modal-md p-4">
                <div class="modal-content">
                <div class="modal-header">
                    <div class="modal-title fs-5" id="exampleModalLabel"></div>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body text-center">
                    <h5>Apakah anda yakin akan menghapus pesanan ini?</h5>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn rounded-5" data-bs-dismiss="modal">Batal</button>
                    <button type="button" class="btn delete rounded-5" @click="deleteCustomer(selectCustomers.id)" data-bs-dismiss="modal">Hapus</button>
                </div>
                </div>
            </div>
        </div>

        <!--Complited Modal-->
        <div class="modal fade center" id="complitedModal" tabindex="-1" aria-labelledby="complitedModalLabel" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered modal-md p-4">
                <div class="modal-content">
                <div class="modal-header">
                    <div class="modal-title fs-5" id="exampleModalLabel"></div>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body text-center">
                    <h5>Apakah anda yakin pesanan ini selesai?</h5>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn rounded-5" data-bs-dismiss="modal">Batal</button>
                    <button type="button" class="btn btn-light rounded-5" @click="markAsCompleted" data-bs-dismiss="modal">Ya</button>
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


li, .btn, h5 {
    font-family: "Poppins", sans-serif;
}

i {
    margin-left: 50px;
}

.list-group-item {
    font-size: 20px;
    margin-top: 8px;
}

.name {
    font-size: 25px
}

.cake {
    margin-top: 20px;
    font-size: 20px;
}

img {
    width: 175px;
}

.btn-danger {
    background-color: red !important;
}

.btn {
    width: 200px;
    font-size: 17px;
    color: white;
    background-color: #C6AC7B;
}

.delete {
    margin-left: 25px
}

.modal {
    height: 300px;
}

.modal-header, .modal-content, .modal-footer {
    border: none;
}

.modal-dialog {
    background-color: white;
}

.modal-footer .btn {
    width: 100px;
}

.modal-footer .btn-light {
    background-color: white !important;
    border: 3px solid #C6AC7B;
    color: black;
}

.modal-footer .delete {
    background-color: red !important;
}

.disabled {
  opacity: 0.6;
  pointer-events: none;
}

@media only screen and (min-width: 600px) and (max-width: 890px) {
    .head-tittle {
        margin-left: 30px;
    }
}

@media only screen and (max-width: 600px) {
    .btn {
        font-size: small;
        width: 150px;
    }

    .list-group-item {
        font-size: medium;
    }

    img {
        margin-left: 65px;
    }

    i {
        margin-left: 0;
    }
}

</style>
