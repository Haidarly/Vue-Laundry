<template>
    <div id="wrapper">
        <sidebar-component></sidebar-component>

        <div id="content-wrapper" class="d-flex flex-column">
            <div id="content">
                <navbar-component></navbar-component>
                <div class="container-fluid">
                    <h1 class="h3 mb-4 text-gray-800">Data Paket</h1>

                    <div class="row">
                        <div class="col-lg-12">
                            <div class="card shadow mb-4">
                                <div class="card-body">
                                    <div class="table-responsive">
                                        <table class="table table-striped table-bordered mt-3" width="100%" cellspacing="0">
                                            <thead>
                                                <tr>
                                                    <th>#</th>
                                                    <th>Jenis</th>
                                                    <th>Harga</th>
                                                    <th>Aksi</th>
                                                </tr>
                                            </thead>
                                            <tbody>
                                                <tr v-for="(p, index) in paket" :key="index">
                                                    <td>{{ index + 1 }}</td>
                                                    <td>{{ p.jenis }}</td>
                                                    <td>{{ "Rp" + p.harga }}</td>
                                                    <td>
                                                        <router-link class="btn btn-warning btn-circle" :to="{name : 'editpaket' , params : {id : p.id}}"><i class="fas fa-pen"></i></router-link>
                                                        <button type="button" @click="hapus(p.id)" class="btn btn-danger btn-circle"><i class="fas fa-trash"></i></button>
                                                    </td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <footer-component></footer-component>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            paket : {}
        }
    },
    created() {
        var data = JSON.parse(this.$store.state.datauser)
        var role = data.role

        if (role == 'kasir' || role == 'owner'){
            this.$swal("Error","Anda tidak dapat mengakses halaman ini","error")
            this.$router.push('/') 
        }

        this.axios.get('http://localhost/projek_laundry/public/api/paket', { headers : { 'Authorization' : 'Bearer' + this.$store.state.token}})
                    .then( res => {
                        this.paket = res.data.data
                    })
                    .catch(err => console.log(err))
    },
    methods : {
        hapus(id) {
            this.axios.delete(`/paket/${id}`, {headers : { 'Authorization' : 'Bearer' + this.$store.state.token}})
                        .then( () => {
                            let i = this.paket.map(item => item.id).indexOf(id);
                            this.paket.splice(i, 1);
                        })
        }
    }
}
</script>