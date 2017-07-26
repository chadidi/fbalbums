<template>
  <div class="photos">
    <div class="container">
      <div class="row">
        <div class="col-lg-12">
          <img :src="$parent.profilePicture" alt="profile" class="profile-picture"/>
          <h1 class="page-header">{{$parent.msg(album.name)}}</h1>
        </div>
        <div class="col-xs-6 thumb" v-for="photo in paginate(photos)">
          <a class="thumbnail" href="#">
            <img class="img-responsive" :src="photo.picture" alt="">
          </a>
        </div>
      </div>
      <div class="row">
        <ul class="pagination justify-content-center">
          <li class="page-item" v-if="isPrev">
            <a class="page-link" href="#" rel="prev" @click.prevent="pagination.current_page--">&laquo;</a>
          </li>
          <li class="page-item" v-if="isNext">
              <a class="page-link" href="#" rel="next" @click.prevent="pagination.current_page++">&raquo;</a>
          </li>
        </ul>
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
      album: [],
      pagination: {
        per_page: 4,
        current_page: 1,
        total: 0
      },
    }
  },
  mounted () {
    this.getAlbum()
    this.getAlbumPhotos()
  },
  methods: {
    getAlbum(){
      let vm = this;
      FB.api("/"+vm.id,function(response){
        vm.album = response;
      });
    },
    getAlbumPhotos(){
      let vm = this;
      FB.api("/"+vm.id+"/photos?fields=name,picture{url},link,id",function(response){
        vm.photos = response.data;
      });
    },
    paginate (array) {
      let per_page = this.pagination.per_page
      let current_page = this.pagination.current_page
      --current_page; // because pages logically start with 1, but technically with 0
      return _.slice(array, current_page * per_page, (current_page + 1) * per_page);
    }
  },
  computed: {
    isPrev(){
      if(this.pagination.current_page>1)
        return true
      return false
    },
    isNext(){
      this.pagination.total = this.photos.length/this.pagination.per_page
      if (this.pagination.current_page<this.pagination.total)
        return true
      return false
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
