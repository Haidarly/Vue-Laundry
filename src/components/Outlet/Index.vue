<template>
    <div id="wrapper">
        <sidebar-component></sidebar-component>
        <div id="content-wrapper" class="d-flex flex-column">
            <div id="content">
                <navbar-component></navbar-component>
                <div class="container-fluid">
                    <h1 class="h3 mb-4 text-gray-800">Data Outlet</h1>

                    <div class="row">
                        <div class="col-lg-12">
                            <div class="card shadow mb-4">
                                <div class="card-body">
                                    <div class="table-responsive">
                                        <router-link to="/outlet/tambah" class="btn btn-info btn-icon-split">
                                            <span class="icon text-white-50">
                                                <i class="fas fa-plus"></i>
                                            </span>
                                            <span class="text">Tambah</span>
                                        </router-link>
                                        <table class="table table-striped table-bordered mt-3" width="100%" cellspacing="0">
                                            <thead>
                                                <tr>
                                                    <th>#</th>
                                                    <th>Nama</th>
                                                    <th>Alamat</th>
                                                    <th>Aksi</th>
                                                </tr>
                                            </thead>
                                            <tbody>
                                                <tr v-for="(o, index) in outlet" :key="index">
                                                    <td>{{ index + 1 }}</td>
                                                    <td>{{ o.nama_outlet }}</td>
                                                    <td>{{ o.alamat }}</td>
                                                    <td>
                                                        <router-link class="btn btn-warning btn-circle" :to="{ name : 'editoutlet' , params : { id : o.id_outlet } }"><i class="fas fa-pen"></i></router-link>
                                                        <button type="button" @click="hapus(o.id_outlet)" class="btn btn-danger btn-circle ml-1"><i class="fas fa-trash"></i></button>
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
            outlet : {}
        }
    },
    created() {
        var data = JSON.parse(this.$store.state.datauser)
        var role = data.role

        if (role == 'kasir' || role == 'owner'){
            this.$swal("Error","Anda tidak dapat mengakses halaman ini","error")
            this.$router.push('/') 
        }
        
        this.axios.get('http://localhost/projek_laundry/public/api/outlet', { headers : { 'Authorization' : 'Bearer' + this.$store.state.token }})
                    .then( res => {
                        this.outlet = res.data.data
                    })
    },
    methods : {
        hapus(id_outlet) {
            this.axios.delete(`http://localhost/projek_laundry/public/api/outlet/${id_outlet}`, {
                headers : { 'Authorization' : 'Bearer' + this.$store.state.token}
            })
            .then( (res) => {
                let i = this.outlet.map(item => item.id_outlet).indexOf(id_outlet);
                this.outlet.splice(i, 1);
                if(res.data.success) {
                              this.$swal("Sukses", res.data.message, "success")
                            //   this.$router.push('/');
                          }
            });
        }
    }
}
</script>