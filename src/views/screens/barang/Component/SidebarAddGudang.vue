<template>
  <b-sidebar
    id="sidebar-add-gudang"
    sidebar-class="sidebar-lg"
    bg-variant="white"
    shadow
    backdrop
    no-header
    right
  >
    <template #default="{ hide }">
      <!-- Header -->
      <div
        class="d-flex justify-content-between align-items-center content-sidebar-header px-2 py-1"
      >
        <h5 class="mb-0">
          Tambah Gudang
        </h5>

        <feather-icon
          class="ml-1 cursor-pointer"
          icon="XIcon"
          size="16"
          @click="hide"
        />
      </div>

      <!-- Body -->
      <b-form
        class="p-2"
        @submit.prevent
      >
        <!-- Customer Name -->
        <b-form-group
          label="Nama Gudang"
          label-for="nama-gudang"
        >
          <b-form-input
            id="nama-gudang"
            v-model="nama_gudang"
            trim
            placeholder="Nama Gudang"
          />
        </b-form-group>

        <b-form-group
          label="Alamat Gudang"
          label-for="alamat-gudang"
        >
          <b-form-input
            id="alamat-gudang"
            v-model="alamat_gudang"
            trim
            placeholder="Alamat Gudang"
          />
        </b-form-group>
        <!-- Form Actions -->
        <div class="d-flex mt-2">
          <b-button
            v-ripple.400="'rgba(255, 255, 255, 0.15)'"
            variant="primary"
            class="mr-2"
            type="submit"
            @click="create"
          >
            Add
          </b-button>
          <b-button
            v-ripple.400="'rgba(186, 191, 199, 0.15)'"
            variant="outline-secondary"
            @click="hide"
          >
            Cancel
          </b-button>
        </div>
      </b-form>
    </template>
  </b-sidebar>
</template>

<script>
import {
  BSidebar, BForm, BFormGroup, BFormInput, BButton,
} from 'bootstrap-vue'
import Ripple from 'vue-ripple-directive'
import store from '@/store'

export default {
  components: {
    BSidebar,
    BForm,
    BFormGroup,
    BFormInput,
    BButton,
  },
  directives: {
    Ripple,
  },
  data() {
    return {
      nama_gudang: '',
      alamat_gudang: '',
    }
  },
  methods: {
    error() {
      this.$swal({
        title: 'Error!',
        text: "Oopss there' a problem!",
        icon: 'error',
        customClass: {
          confirmButton: 'btn btn-primary',
        },
        buttonsStyling: false,
      })
    },
    suksesToast(nama) {
      this.$bvToast.toast(`Gudang ${nama} ditambahkan`, {
        title: 'Sukses',
        variant: 'success',
        toaster: 'b-toaster-bottom-center',
      })
    },
    create() {
      if (this.nama_gudang !== '') {
        store
          .dispatch('app-barang/addGudang', {
            nama: this.nama_gudang,
            alamat: this.alamat_gudang,
          })
          .then(res => {
            if (res.status === 200) {
              store.commit('app-barang/UPDATE_LIST_GUDANG', res.data)
              this.$root.$emit('bv::toggle::collapse', 'sidebar-add-gudang')
              this.suksesToast(res.data.nama)
              this.nama_gudang = ''
              this.alamat_gudang = ''
            } else {
              this.error()
            }
          })
      }
    },
  },
}
</script>

<style lang="scss">
@import '@core/scss/vue/libs/vue-select.scss';
</style>
