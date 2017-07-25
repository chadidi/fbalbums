<template>
  <div class="albums">
    <div class="container">
      <div class="row">
        <div class="col-lg-12">
          <img :src="profilePicture" alt="profile" class="profile-picture"/>
          <h1 class="page-header">{{msg}}</h1>
        </div>
        <div class="col-xs-6 thumb" v-for="album in albums">
          <router-link class="thumbnail" :to="'/album/'+album.id">
            <img class="img-responsive" :src="album.picture.data.url" alt="">
            <p>{{album.name}}</p>
          </router-link>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: 'Albums',
  props: ['profile', 'albums'],
  data () {
    return {
    }
  },
  mounted() {
    // this.getAlbums;
  },
  computed: {
    msg () {
      return this.profile.name+' Albums'
    },
    profilePicture () {
      return (this.profile.id)
        ? 'https://graph.facebook.com/' + this.profile.id + '/picture?width=300'
        : '/static/default-avatar.png'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
