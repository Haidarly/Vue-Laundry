<template>
    <!-- Sidebar -->
        <ul class="navbar-nav bg-gradient-info sidebar sidebar-dark accordion" id="accordionSidebar">

            <!-- Sidebar - Brand -->
            <router-link to="/" class="sidebar-brand d-flex align-items-center justify-content-center">
                <div class="sidebar-brand-icon rotate-n-15">
                    <i class="fas fa-laugh-wink"></i>
                </div>
                <div class="sidebar-brand-text mx-3">Laundry Jaya Berkah</div>
            </router-link>

            <!-- Divider -->
            <hr class="sidebar-divider my-0">

            <!-- Nav Item - Dashboard -->
            <li class="nav-item">
                <router-link to="/" class="nav-link">
                    <i class="fas fa-fw fa-tachometer-alt"></i>
                    <span>Dashboard</span>
                </router-link>
            </li>

            <li v-if="isAdmin" class="nav-item">
                <router-link to="/user" class="nav-link">
                    <i class="fas fa-user"></i>
                    <span>User</span>
                </router-link>
            </li>

            <li v-if="isAdmin || isKasir" class="nav-item">
                <router-link to="/member" class="nav-link">
                    <i class="fas fa-users"></i>
                    <span>Member</span>
                </router-link>
            </li>

            <li v-if="isAdmin" class="nav-item">
                <router-link to="/outlet" class="nav-link">
                    <i class="fas fa-store"></i>
                    <span>Outlet</span>
                </router-link>
            </li>

            <li v-if="isAdmin" class="nav-item">
                <router-link to="/paket" class="nav-link">
                    <i class="fas fa-tshirt"></i>
                    <span>Paket</span>
                </router-link>
            </li>

            <li v-if="isAdmin || isKasir" class="nav-item">
                <router-link to="/transaksi" class="nav-link">
                    <i class="fas fa-money-check-alt"></i>
                    <span>Transaksi</span>
                </router-link>
            </li>
            
            <li v-if="isAdmin || isKasir || isOwner" class="nav-item">
                <router-link to="/report" class="nav-link">
                    <i class="fas fa-file-alt"></i>
                    <span>Report</span>
                </router-link>
            </li>
        </ul>
        <!-- End of Sidebar -->
</template>

<script>
export default {
    data() {
        return {
            isAdmin : false,
            isKasir : false,
            isOwner : false
        }
    },
    created() {
        var user = JSON.parse(this.$store.state.datauser)
        var role = user.role

        if(role == 'admin') this.isAdmin = true
        else if (role == 'kasir') this.isKasir = true
        else this.isOwner = true
    }
}
</script>