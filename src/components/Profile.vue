<template>
  <div class="profile">
    <div class="container">
      <div class="row">
        <!-- https://stackoverflow.com/questions/42309986/vuejs-get-url-query -->
        <div class="item col-xs-12 col-lg-12" v-for="item in content">
          <h3>{{ item.name }}</h3>
          <div class="container">
            <div class="row">
              <div class="item col-xs-12 col-md-4" v-for="imgId in item.imageKeyIds">
                <img :src="'http://json-porn.com/image/' + imgId + '/300.jpg'" class="img-thumbnail" />
              </div>
            </div>
          </div>
        </div>
      </div>
      
    </div>
  </div>      
</template>

<script>
import myconfig from '../../myconfig';

export default {
  data() {
    return {
      content: [],
    };
  },
  mounted: function mymounted() {
    /* eslint no-console: ["error", { allow: ["warn", "error", "log"] }] */
    // console.log('mounted');
    // pass
    const config = { headers: {
      'X-Mashape-Key': myconfig.x_mashape_key,
    } };
    const actorId = this.$route.query.actorId;
    const apiUrl = `https://steppschuh-json-porn-v1.p.mashape.com/porn/?actorid=${actorId}&includedownloads=true&includeimages=true&offset=0&pornType=4`;
    this.axios.get(apiUrl, config).then((response) => {
      // console.log(response);
      this.content = response.data.content;
    });
  },
};
</script>

<style scoped>

</style>
