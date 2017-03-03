<template>
  <div class="main">
    <!-- container -->
    <div class="container">
      <!-- well, well-sm, well is add small round corner to div -->
      <div class="well well-sm">
        <!-- form group -->
        <div class="form-group">
          <!-- input group, input group md -->
          <div class="input-group input-group-md">
            <!-- div, icon-addon, addon-md, like the search icon????? -->
            <div class="icon-addon addon-md">
              <input type="text" placeholder="What are you looking for?" class="form-control" v-model="query">
            </div>
            
            <!-- span -->
            <!-- class, input group btn -->
            <span class="input-group-btn">
              <!-- search button, click, search -->
              <!-- v-if, not loading, !loading -->
              <button class="btn btn-default" type="button" @click="search()" v-if="!loading">Search!</button>
              <!-- another button -->
              <!-- disabled, vif.. loading -->
              <button class="btn btn-default" type="button" disabled="disabled" v-if="loading">Searching...</button>
            </span>
          </div>    
        </div>        
      </div><!-- end form group -->
      
      
      <div class="alert alert-danger" role="alert" v-if="error">
        <span class="glyphicon glyphicon-exclamation-sign" aria-hidden="true"></span>
        @{{ error }}
      </div>

      <div id="products" class="row list-group">
        <div class="item col-xs-4 col-lg-4" v-for="product in products">
          <div class="thumbnail">
            <img class="group list-group-image" :src="product.image" :alt="product.title" />
            <div class="caption">
              <p class="group inner list-group-item-text">@{{ product.title | truncate(40) }}</p>
              <p class="group inner list-group-item-text">@{{ product.description | truncate(50) }} </p>
            </div>
          </div>
        </div>
      </div>
      
    </div>
  </div>      
</template>

<script>
export default {
  data() {
    return {
      products: [],
      loading: false,
      error: false,
      query: '',
    };
  },
  methods: {
    search: function search() {
      this.error = '';
      this.products = [];
      this.loading = true;
      const theQuery = this.query;
      this.$http.get(`http://vue_search_back_1.local/api/search?q=${theQuery}`).then((response) => {
        if (response.body.error) {
          this.error = response.body.error;
        } else {
          this.products = response.body;
        }
        this.loading = false;
        this.query = '';
        /* eslint no-console: ["error", { allow: ["warn", "error", "log"] }] */
        // console.log(this.products);
      });
    },
  },
};
</script>

<style scoped>

</style>
