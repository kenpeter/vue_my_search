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
              <input type="text" placeholder="Search actor name?" class="form-control" v-model="query">
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
            <div class="caption">
              <router-link :to="{ path: 'profile', query: { actorId: product.key.id }}">
                <img :src="product.singleImgUrl" />
              </router-link>
              <p class="group inner list-group-item-text">Name: {{ product.name | truncate(40) }}</p>
              <p class="group inner list-group-item-text">Cupsize: {{ product.cupSize }} </p>
            </div>
          </div>
        </div>
      </div>
      
    </div>
  </div>      
</template>

<script>
import Promise from 'bluebird';
import myconfig from '../../myconfig';

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
      const apiUrl = `https://steppschuh-json-porn-v1.p.mashape.com/search/?advanced=false&q=${theQuery}`;
      // const apiUrl = `https://steppschuh-json-porn-v1.p.mashape.com/actors/?&actorname=${theQuery}&count=5&offset=0`;
      const config = { headers: {
        'X-Mashape-Key': myconfig.x_mashape_key,
      } };
      this.axios.get(apiUrl, config).then((response) => {
        /* eslint no-console: ["error", { allow: ["warn", "error", "log"] }] */
        // console.log(response.data.content);
        let arr = response.data.content;
        arr = arr.filter((el) => {
          // https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/filter
          if (el.entryType === 1) {
            return true;
          }
          return false;
        });
        /* eslint arrow-body-style: ["error", "always"] */
        return new Promise((resolve, reject) => {
          const condi = 0;
          // get away lint error
          if (condi) return reject();
          return resolve(arr);
        });
      }).then((arr) => {
        return Promise.each(arr, (item) => {
          return new Promise((resolve, reject) => {
            const imgKeyId = item.imageKeyIds[0];
            const singleImgUrl = `http://json-porn.com/image/${imgKeyId}/300.jpg`;
            item.singleImgUrl = singleImgUrl;
            const condi = 0;
            // get away lint error
            if (condi) return reject();
            return resolve();
          });
        });
      }).then((newArr) => {
        this.loading = false;
        this.products = newArr;
        // console.log(newArr);
      });
    },
  },
};
</script>

<style scoped>

</style>
