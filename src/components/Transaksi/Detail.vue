<template>
    <div id="wrapper">
        <sidebar-component></sidebar-component>
        <div id="content-wrapper" class="d-flex flex-column">
            <div id="content">
                <navbar-component></navbar-component>
                <div class="container-fluid">
                    <div class="row">
                        <div class="col-md-10">
                            <div class="row">
                                <div class="col">
                                    <div class="card border-left-primary shadow h-100 py-2">
                                        <div class="card-body">
                                            <div class="row no-gutters align-items-center">
                                                <div class="col mr-2">
                                                    <div class="row">
                                                        <div class="col">
                                                            <div class="text-xs font-weight-bold text-primary text-uppercase">Member</div>
                                                            <div class="h6 mb-0 font-weight-bold text-gray-800 mb-2">{{ transaksi.nama }}</div>
                                                            <div class="text-xs font-weight-bold text-primary text-uppercase">Tanggal Order</div>
                                                            <div class="h6 mb-0 font-weight-bold text-gray-800 mb-2">{{ transaksi.tgl_order | moment("DD/MM/YYYY") }}</div>
                                                            <div class="text-xs font-weight-bold text-primary text-uppercase">Batas Waktu</div>
                                                            <div class="h6 mb-0 font-weight-bold text-gray-800 mb-2">{{ transaksi.batas_waktu | moment("DD/MM/YYYY") }}</div>
                                                        </div>
                                                        <div class="col">
                                                            <div class="text-xs font-weight-bold text-primary text-uppercase">Status Laundry</div>
                                                            <div class="h6 mb-0 font-weight-bold text-gray-800 mb-2">{{ transaksi.status }}</div>
                                                            <div class="text-xs font-weight-bold text-primary text-uppercase">Tanggal Pembayaran</div>
                                                            <div v-if="transaksi.tgl_bayar == null" class="h6 mb-0 font-weight-bold text-gray-800 mb-2">-</div>
                                                            <div v-else class="h6 mb-0 font-weight-bold text-gray-800 mb-2">{{ transaksi.tgl_bayar | moment("DD/MM/YYYY") }}</div>
                                                            <div class="text-xs font-weight-bold text-primary text-uppercase">Status Pembayaran</div>
                                                            <div v-if="transaksi.dibayar == 'belum dibayar'" class="h6 mb-0 font-weight-bold text-gray-800 mb-2">Belum Dibayar</div>
                                                            <div v-else class="h6 mb-0 font-weight-bold text-gray-800 mb-2">Sudah Dibayar</div>
                                                        </div>
                                                    </div>
                                                    <div class="row mt-3">
                                                        <button :disabled="disableStatus" type="button" class="btn btn-warning" @click="ubahStatus(id_transaksi)">Ubah Status Laundry</button>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <!-- <div class="col">
                    <button :disabled="disableStruk"
                        type="button"
                        class="btn btn-sm btn-info btn-icon-split mb-3"
                        @click="generateReport"
                    >
                        <span class="icon text-white-50">
                        <i class="fas fa-print"></i>
                        </span>
                        <span class="text">Cetak Struk</span>
                    </button>
                    </div>
                    <div class="col-md-6 align-items-center">
                        <VueHtml2pdf
                        :show-layout="true"
                        :float-layout="false"
                        :enable-download="false"
                        :preview-modal="true"
                        :paginate-elements-by-height="1400"
                        filename="report_transaksi"
                        :pdf-quality="2"
                        :manual-pagination="false"
                        pdf-format="a4"
                        pdf-orientation="portrait"
                        pdf-content-width="800px"
                        ref="html2Pdf">
                        <section slot="pdf-content">
                            <div class="report text-center">
                            <hr>
                            <h1 class="h1 font-weight-bold mb-0 text-gray-900">Struk Transaksi</h1>
                            <h3 class="h3 mb-0 text-gray-800">Laundry Online</h3>
                            <h5 class="h5 mb-0 text-gray-800">Jl. Dr. Wahidin No 86, Balung</h5>
                            </div>
                            <table class="table" cellspacing="0">
                            <thead>
                                <tr>
                                <th>No.</th>
                                <th>Jenis</th>
                                <th>Jumlah</th>
                                <th>Harga</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(m, index) in detail" :key="index">
                                <td>{{ index + 1 }}</td>
                                <td>{{ m.jenis }}</td>
                                <td>{{ m.quantity }}</td>
                                <td>{{ m.subtotal }}</td>
                                </tr>
                                <tr v-if="total != ''">
                                <td colspan="3" class="text-right">Total</td>
                                <td>
                                    <h6 class="font-weight-bold">Rp {{ total }}</h6>
                                </td>
                                </tr>
                            </tbody>
                            </table>
                        </section>
                        </VueHtml2pdf>
                    </div> -->
                    <div class="row mt-3">
                        <div class="col">
                            <div class="card shadow mb-4">
                                <div class="card-header py-3">
                                    <h6 class="m-0 font-weight-bold text-primary">Detail Transaksi</h6>
                                </div>
                                <div class="card-body">
                                    <table class="table">
                                        <thead>
                                            <tr>
                                                <th>#</th>
                                                <th>Jenis</th>
                                                <th>Jumlah (kg/satuan)</th>
                                                <th>Subtotal</th>
                                            </tr>
                                        </thead>
                                        <tbody>
                                            <tr v-for="(d, index) in detail" :key="index">
                                                <td>{{ index + 1 }}</td>
                                                <td>{{ d.jenis }}</td>
                                                <td>{{ d.quantity }}</td>
                                                <td>Rp {{ d.subtotal }}</td>
                                            </tr>
                                            <tr v-if="total != ''">
                                                <td colspan="3" class="text-right">Total</td>
                                                <td><h6 class="font-weight-bold"> Rp {{ total }} </h6></td>
                                            </tr>
                                        </tbody>
                                    </table>
                                    <button :disabled="disableBayar" type="button" class="btn btn-success mr-3" @click="bayar">Bayar</button>
                                    <router-link v-if="transaksi.status != 'Diambil' && transaksi.status != 'Selesai' && transaksi.dibayar != 'dibayar'" :to="{ name : 'tambahdetail' , params : { id : this.id_transaksi }}" class="btn btn-primary">
                                        Tambah Detail Transaksi
                                    </router-link>
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
            id_transaksi : this.$route.params.id,
            transaksi : {},
            outlet : {},
            detail : {},
            total : ''
        }
    },
    created() {
        var data_o = JSON.parse(this.$store.state.dataoutlet)
        this.outlet = data_o

        var data = JSON.parse(this.$store.state.datauser)
        var role = data.role

        if (role == 'owner'){
            this.$swal("Error","Anda tidak dapat mengakses halaman ini","error")
            this.$router.push('/') 
        }

        this.axios.get(`http://localhost/projek_laundry/public/api/transaksi/${this.id_transaksi}`, { headers : { 'Authorization' : `Bearer` + this.$store.state.token} })
            .then(res => {
                this.transaksi = res.data
            })
            .catch(err => console.log(err))
        this.axios.get(`http://localhost/projek_laundry/public/api/transaksi/detail/${this.id_transaksi}`, { headers : { 'Authorization' : `Bearer` + this.$store.state.token} })
            .then(res => {
                this.detail = res.data
            })
            .catch(err => console.log(err))
        this.axios.get(`http://localhost/projek_laundry/public/api/transaksi/total/${this.id_transaksi}`, { headers : { 'Authorization' : `Bearer` + this.$store.state.token} })
            .then(res => {
                this.total = res.data.total
            })
    },
    computed : {
        disableStatus() {
            if(this.transaksi.status == 'selesai' || this.transaksi.status == 'diambil' || this.detail.length == 0) {
                return true
            } else {
                return false
            }
        },
        disableStruk() {
            if(this.transaksi.status == 'baru' || this.transaksi.status == 'proses' || this.transaksi.status == 'selesai') {
                return true
            } else {
                return false
            }
        },
        disableBayar() {
            if(this.transaksi.status == 'baru' || this.transaksi.status == 'proses' || this.transaksi.status == 'diambil') {
                return true
            } else {
                return false
            }
        }
    },
    methods : {
        bayar() {
            this.axios.post(`http://localhost/projek_laundry/public/api/transaksi/bayar/${this.id_transaksi}`, 
                        this.transaksi, { headers : {'Authorization' : `Bearer` + this.$store.state.token} })
                    .then( () => {
                        this.$router.go();
                    })
                    .catch(err => console.log(err))
        },
        ubahStatus() {
            if (this.transaksi.status == 'baru') { this.transaksi.status = 'proses' }
            else { this.transaksi.status = 'selesai' }

            this.axios.post(`http://localhost/projek_laundry/public/api/transaksi/status/${this.id_transaksi}`, 
                            this.transaksi, { headers : {'Authorization' : `Bearer` + this.$store.state.token} })
                    .then( () => {
                        this.$router.go();
                    })
                    .catch(err => console.log(err))
        }
    }
}
</script>