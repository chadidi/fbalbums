<template>
  <div id="app">
    <navbar></navbar>
    <router-view :profile="profile" :albums="albums"></router-view>
    <div v-if="!ready" class="">
      <center>
        <img src="/static/loading.gif" alt="" width="400px"/>
      </center>
    </div>
  </div>
</template>

<script>
import Navbar from '@/components/Navbar'
/* global FB */

export default {
  name: 'app',
  components: {'navbar': Navbar},
  data () {
    return {
      profile: {name: 'Anonymous'},
      albums: {},
      ready: false,
      authorized: false
    }
  },
  computed: {
    profilePicture () {
      return (this.profile.id)
        ? 'https://graph.facebook.com/' + this.profile.id + '/picture?width=300'
        : '/static/default-avatar.png'
    }
  },
  methods: {
    msg (name) {
      return name+' Album'
    },
    getAlbums(){
      let vm = this;
      FB.api('me/albums?fields=picture{url},name,count',  function(resp) {
        vm.albums = _.orderBy(resp.data, 'name', 'asc');
      });
    },
    getProfile () {
      let vm = this
      FB.api('/me', function (response) {
        vm.profile = response;
      })
    },
    login () {
      let vm = this
      FB.login(function (response) {
        vm.statusChangeCallback(response)
      }, {scope: 'user_photos'})
    },
    logout () {
      let vm = this
      FB.logout(function (response) {
        vm.statusChangeCallback(response)
      })
      this.profile = {name: 'Anonymous'};
      this.albums = {};
    },
    statusChangeCallback (response) {
      let vm = this
      vm.ready = true
      if (response.status === 'connected') {
        vm.authorized = true
        vm.getProfile()
        vm.getAlbums()
      } else if (response.status === 'not_authorized') {
        vm.authorized = false
      } else {
        vm.authorized = false
      }
    }
  },
  mounted () {
    let vm = this;
    window.fbAsyncInit = () => {
      FB.init({
        appId: '1943943495818634',
        cookie: true,
        xfbml: true,
        version: 'v2.10'
      })
      FB.getLoginStatus(function (response) {
        vm.statusChangeCallback(response)
      })
    }
  }
}
</script>