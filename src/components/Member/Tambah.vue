<template>
    <div id="wrapper">
        <sidebar-component></sidebar-component>
        <div id="content-wrapper" class="d-flex flex-column">
            <div id="content">
                <navbar-component></navbar-component>

                <div class="container-fluid">
                    <div class="row">
                        <div class="col-lg-8">
                            <div class="card shadow mb-4">
                                <div class="card-header py-3">
                                    <h6 class="m-0 font-weight-bold text-primary">Tambah Data Member</h6>
                                </div>
                                <div class="card-body">
                                    <form @submit.prevent="tambah">
                                        <div class="form-group">
                                            <label>Nama</label>
                                            <input type="text" class="form-control" v-model="member.nama">
                                        </div>
                                        <div class="form-group">
                                            <label>Alamat</label>
                                            <textarea rows="4" class="form-control" v-model="member.alamat"></textarea>
                                        </div>
                                        <div class="form-group">
                                            <div>
                                                <label>Jenis Kelamin</label>
                                            </div>
                                            <div class="btn-group btn-group-toggle" data-toggle="buttons">
                                                <label class="btn btn-secondary">
                                                    <input type="radio" value="Laki-laki" v-model="member.jk"> Laki-laki
                                                </label>
                                                <label class="btn btn-secondary">
                                                    <input type="radio" value="Perempuan" v-model="member.jk"> Perempuan
                                                </label>
                                            </div>
                                        </div>
                                        <div class="form-group">
                                            <label>Nomor Telepon</label>
                                            <input type="text" class="form-control" v-model="member.telepon">
                                        </div>
                                        <!-- <div class="btn-group">
                                            <button type="submit" class="btn btn-success btn-block">Simpan</button>
                                            <router-link to="/member" class="nav-link btn btn-warning btn-block">Kembali</router-link>
                                        </div> -->
                                        <div class="btn-toolbar" role="toolbar" aria-label="Toolbar with button groups">
                                            <div class="btn-group mr-2" role="group" aria-label="Second group">
                                                <button type="submit" class="btn btn-success btn-block">Simpan</button>
                                            </div>
                                            <div class="btn-group" role="group" aria-label="Third group">
                                                <router-link to="/member" class="nav-link btn btn-warning btn-block">Kembali</router-link>
                                            </div>
                                        </div>
                                    </form>
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
            member: {}
        }
    },
    created() {
        var data = JSON.parse(this.$store.state.datauser)
        var role = data.role

        if (role == 'owner'){
            this.$swal("Error","Anda tidak dapat mengakses halaman ini","error")
            this.$router.push('/') 
        }
    },
    methods : {
        tambah() {
        this.axios.post('http://localhost/projek_laundry/public/api/member',
                        this.member, { headers: {Authorization : `Bearer` + this.$store.state.token}})
                    .then( (res) => {
                        if(res.data.success) {
                              this.$swal("Sukses", res.data.message, "success")
                              this.$router.push('/member')
                          }
                    })
                    .catch(err => console.log(err))
        }
    }
}
</script>