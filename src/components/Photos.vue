<template>
  <div class="photos">
    <div class="container">
      <div class="row">
        <div class="col-lg-12">
          <img :src="profilePicture" alt="profile" class="profile-picture"/>
          <h1 class="page-header">{{msg}}</h1>
        </div>
        <div class="col-xs-6 thumb" v-for="photo in photos">
          <a class="thumbnail" href="#">
            <img class="img-responsive" :src="photo.picture" alt="">
          </a>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: 'Photos',
  props: ['profile'],
  data () {
    return {
      id: this.$route.params.id,
      photos: [],
      album: []
    }
  },
  mounted () {
    this.getAlbum()
    this.getAlbumPhotos()
  },
  computed: {
    msg () {
      return this.album.name+' Album'
    },
    profilePicture () {
      return (this.profile.id)
        ? 'https://graph.facebook.com/' + this.profile.id + '/picture?width=300'
        : '/static/default-avatar.png'
    }
  },
  methods: {
    getAlbum(){
      let vm = this;
      window.FB.api("/"+vm.id,function(response){
        vm.album = response;
      });
    },
    getAlbumPhotos(){
      let vm = this;
      window.FB.api("/"+vm.id+"/photos?fields=name,picture{url},link,id",function(response){
        vm.photos = response.data;
      });
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
