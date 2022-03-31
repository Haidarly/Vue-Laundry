<template>
    <nav class="navbar navbar-expand navbar-light bg-gradient-white topbar mb-4 static-top shadow">
        <ul class="navbar-nav ml-auto">
            <li class="nav-item mt-4 text-gray-800"><strong>{{ nama }}</strong></li>
            <div class="topbar-divider d-none d-sm-block"></div>
            <li class="nav-item dropdown no-arrow">
                <a 
                class="nav-link dropdown-toggle"
                href="#"
                id="userDropdown"
                role="button"
                data-toggle="dropdown"
                aria-haspopup="true"
                aria-expanded="false"
                >
                    <span class="mr-2 d-done d-lg-inline text-gray-800 medium"><strong>{{ nama_user }}</strong></span>
                    <img class="img-profile rounded-circle" src="@/assets/image/profile.jpg" alt="">
                </a>
                <div class="dropdown-menu dropdown-menu-right shadow animated--grow-in" aria-labelledby="userDropdown">
                    <a class="dropdown-item" @click="logout">
                        <i class="fas fa-sign-out-alt fa-sm fa-fw mr-2 text-gray-400"></i>
                        Logout
                    </a>
                </div>
                <!-- <button class="btn btn-sm btn-danger btn-icon-split" @click="logout">
                    <span class="icon text-white-50">
                        <i class="fas fa-sign-out-alt"></i>
                    </span>
                    <span class="text">Logout</span>
                </button> -->
            </li>
        </ul>
    </nav>
</template>

<script>
export default {
    data() {
        return {
            nama_user : '',
            nama : ''
        }
    },
    created() {
        var user = JSON.parse(this.$store.state.datauser);
        this.nama_user = user.name;

        var data = JSON.parse(this.$store.state.dataoutlet)
        var namaoutlet = data.nama_outlet

        this.nama = namaoutlet

        this.axios.get('http://localhost/projek_laundry/public/api/login/check', {
            headers : {'Authorization' : 'bearer' + this.$store.state.token}
        })
        .then( (res) => {
            if(!(res.data.success)) {
                this.$store.commit('clearToken')
                this.$store.commit('clearUser')
                this.$swal("Error","Sesi Login Sudah Berakhir","Error")
                this.$router.push('/login')
            }
        })
    },
    methods : {
        logout() {
            this.axios.post('http://localhost/projek_laundry/public/api/logout', { headers : {'Authorization' : 'Bearer' + this.$store.state.token}}).then( () => {
                this.$store.commit('clearToken')
                this.$store.commit('clearUser')
                this.$router.push('/login')
            })
        }
    }
}
</script>