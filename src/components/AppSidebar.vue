<template>
      <div class="c-sidebar c-sidebar-dark c-sidebar-fixed c-sidebar-lg-show" id="sidebar">
      <div class="c-sidebar-brand"><img class="c-sidebar-brand-full" src="/img/logo.jpg" style="max-height: 46px" alt="Logo"><img class="c-sidebar-brand-minimized" src="/img/logo.jpg" style="max-height: 46px" alt="Logo"></div>
      <ul class="c-sidebar-nav">
        
        <li class="c-sidebar-nav-item">
          <router-link class="c-sidebar-nav-link" to="/"
          :class="[currentPage.includes('home') ? activeClass : '']">
            <i class="mr-3 flaticon-squares-4"></i>
            Dashboard
          </router-link>
        </li>
        
        <li class="c-sidebar-nav-title">Menu utama</li>
        
        <li class="c-sidebar-nav-item">
          <router-link class="c-sidebar-nav-link" :to="{  name: 'sekolah.setting' }"
          :class="[currentPage.includes('sekolah.setting') ? activeClass : '']">
            <i class="mr-3 flaticon-home-2"></i> Sampel menu
          </router-link>
        </li>

        <li class="c-sidebar-nav-item c-sidebar-nav-dropdown">
          <a class="c-sidebar-nav-link c-sidebar-nav-dropdown-toggle" href="#">
            <i class="mr-3 flaticon-folder"></i> Sub menu
          </a>

          <ul class="c-sidebar-nav-dropdown-items">
            <li class="c-sidebar-nav-item">
              <router-link class="c-sidebar-nav-link" :to="{ name: 'jurusan.data' }"
              :class="[currentPage.includes('jurusan.data') ? activeClass : '']"> 
                <span class="c-sidebar-nav-icon"></span> Sub menu 1
              </router-link>
            </li>
            <li class="c-sidebar-nav-item">
              <router-link class="c-sidebar-nav-link" :to="{ name: 'peserta.data' }"
              :class="[currentPage.includes('peserta.data') ? activeClass : '']">
                <span class="c-sidebar-nav-icon"></span> Sub menu 2
              </router-link>
            </li>

          </ul>
        </li>

        <li class="c-sidebar-nav-item">
          <a class="c-sidebar-nav-link" href="#" @click.prevent="logout">
            <i class="mr-3 flaticon-logout"></i> 
            Logout
          </a>
        </li>
      </ul>
      <button class="c-sidebar-minimizer c-class-toggler" type="button" data-target="_parent" data-class="c-sidebar-minimized"></button>
    </div>
</template>
<script>
import { mapState, mapActions, mapGetters } from 'vuex'
import { successToas, errorToas} from '@/entities/notif'

export default {
  data() {
    return {
      activeClass: 'c-active'
    }
  },
  methods: {
    ...mapActions('auth', ['loggedOut']),
    ...mapActions('setting', ['getSettingSekolah']),
    async logout() {
      try {
        await this.loggedOut()
        localStorage.removeItem('token')
        this.$store.state.token = localStorage.getItem('token')
        this.$router.push('/login')
      } catch (error) {
        localStorage.removeItem('token')
        this.$store.state.token = localStorage.getItem('token')
        this.$router.push('/login')
      }
    }
  },
  computed: {
    ...mapGetters(['isAuth','baseURL']),
    ...mapState('user', {
      user: state => state.authenticated
    }),
    ...mapState('setting',{
      sekolah: state => state.set_sekolah,
      ujian: state => state.set_ujian
    }),
    currentPage() {
      return this.$route.name ? this.$route.name : [];
    }
  },
  async created() {
    if(this.isAuth) {
      try {
        await this.getSettingSekolah()
      } catch (error) {
        this.$bvToast.toast(error.message, errorToas())
      }  
    }
  }
}
</script>
<style type="text/css">
  
</style>