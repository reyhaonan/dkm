<template>
  <nav class="navigation">
    <div class="navigation__brand bg-white rounded shadow">
      <img src="@/assets/logo.svg" class="navigation__brand__logo" draggable="false">
      <span class="navigation__brand__name text-dovegray h4">
        Hai DKM
      </span>
      <a-dropdown :trigger="['click']">
        <a-icon type="ellipsis" class="navigation__brand__more" @click="e => e.preventDefault()"/>
        <a-menu slot="overlay">
          <a-menu-item key="0">
            <a @click.prevent="backup">Backup database</a>
          </a-menu-item>
          <a-menu-item key="1">
            <a @click.prevent="reset">Reset aplikasi</a>
          </a-menu-item>
          <a-menu-item key="2">
            <a @click.prevent="openLinks('https://github.com/reyhaonan/dkm/issues/new')">Laporkan kendala</a>
          </a-menu-item>
          <a-menu-item key="3">
            <a @click.prevent="about">Tentang aplikasi</a>
          </a-menu-item>
        </a-menu>
      </a-dropdown>
    </div>

    <div class="navigation__routes">
      <router-link to="/beranda" class="navigation__routes__link"><a-icon type="home" theme="filled"/>Beranda</router-link>
      <router-link to="/data" class="navigation__routes__link"><a-icon type="folder" theme="filled"/>Data warga</router-link>
      <router-link to="/galeri" class="navigation__routes__link"><a-icon type="camera" theme="filled"/>Galeri</router-link>
      <router-link to="/pengaturan" class="navigation__routes__link"><a-icon type="setting" theme="filled"/>Pengaturan</router-link>
      <a class="navigation__routes__link pointer" @click.prevent="logout"><a-icon type="logout" />Logout</a>
    </div>

  </nav>
</template>

<script>
import VueContext from '@/components/VueContext'
import {shell} from 'electron'
const { dialog } = require('electron').remote
const WIN = require('electron').remote.getCurrentWindow();

export default {
  components:{
    VueContext
  },
  methods:{
    about(){
      alert('Hai DKM ver 0.1.0', 'About')
    },
    logout(){
      this.$store.dispatch('logout')
      this.$router.push('/')
    },
    openLinks(url){
      shell.openExternal(url)
    },
    reset(){
      this.$http.delete('/reset')
    },
    backup(){
      dialog.showSaveDialog(WIN,{
        //Placeholder 1
        title: "Save file",
        
        //Placeholder 2
        defaultPath : "DataDKM" + '.sqlite',
        
        //Placeholder 4
        buttonLabel : "Simpan file",
        
        //Placeholder 3
        filters :[
          // {name: 'Microsoft excel', extensions: ['xlsx', 'xls']},
          {name: 'Database file', extensions: ['sqlite']},
          {name: 'All Files', extensions: ['*']}
        ]
        }, path => this.$http.post('/exportdatabase', {path: path})
      )
      
      
    }
  }
}
</script>

<style lang="scss">
  .navigation{
    display: flex;
    flex-direction: column;
    flex-basis: 222px;

    &__brand{
      height: 50px;
      display: flex;
      margin-bottom: 3rem;

      &__logo{
        margin: margin($right:0,$left:1rem);
        height: 50%;
      }

      &__name{
        margin: margin($left:1rem);
      }

      &__more{
        margin: margin($right:1rem);
        cursor: default;
      }
    }


    &__routes{
      display: flex;
      flex-direction: column;

      &__link{
        @extend .text-cadetblue, .h4;
        text-decoration: none;
        margin: .5rem 0;
        display: flex;

        &:hover{
          color: $cadetBlue;
        }
      }

      .router-link-exact-active{
        @extend .text-dovegray;

        &:hover{
          color: $doveGray
        }
      }

      .anticon{
        font-size: 1rem;
        margin: margin($left:1.5rem,$right:1rem);
        transition: none !important;
      }
    }
  }
</style>