<template>
  <div class="about">
    <!-- Map for get location-->
    <l-map @ready="onReady" @click="onLocationFound" style="height: 90vh" :zoom="zoom" :center="center">
      <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
      <l-marker :icon="icon" :lat-lng="markerLatLng"></l-marker>
    </l-map>
    <!--add post to API-->
    <button @click="add" class="button is-primary">تایید موقعیت</button>
  </div>
</template>

<script>
import L from 'leaflet'
import {LMap, LTileLayer, LMarker} from 'vue2-leaflet';
import swal from 'sweetalert';

export default {
  components: {
    LMap,
    LTileLayer,
    LMarker
  },
  data () {
    return {
      url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
      attribution:
        '&copy; <a target="_blank" href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      zoom: 8,
      center: [35.603, 51.438],
      markerLatLng: [35.604, 51.437],
      //pin icon Edite
      icon: L.icon({
        iconUrl: 'https://cdn.iconscout.com/icon/premium/png-64-thumb/pin-574-356372.png',
        iconSize: [32, 37],
        iconAnchor: [16, 37]
      }),
    };
  },
  methods: {
    //load map 
    onReady (mapObject) {
       mapObject.locate();
    },
    
    //Change pin in map 
    onLocationFound(location){
       this.markerLatLng = [location.latlng.lat,location.latlng.lng]
    },

    //add post in API and go to next page
    add(){
       try{
         let obj = JSON.parse(localStorage.getItem('obj'))
         fetch('https://stage.achareh.ir/api/karfarmas/address', {
            method: 'POST',
            headers: { 'Content-Type': 'application/json',
                'Authorization': 'Basic '+btoa('09822222222:Sana12345678')
            },
            body: JSON.stringify({
                  region:1,
                  address: obj.location,
                  lat: this.markerLatLng[0],
                  lng: this.markerLatLng[1],
                  coordinate_mobile:obj.phone,
                  coordinate_phone_number:obj.tel,
                  first_name: obj.name,
                  last_name: obj.family,
                  gender:obj.radio
            })
         })
          .then(res => res.json())
          .then(json => console.log(json))
          localStorage.clear()
     
         swal({
          icon: "success",        
          title: 'موفق',
          text: 'درخواست موفق'
         });
         this.$router.push('/get')

        }catch{
           swal({
            icon: "error",
            title: 'خطا',
            text: 'درخواست نا موفق'
           });
         }
     }
  }
}
</script>
<style scoped>
.button{
  width: 100%;
  margin-top: 20px;
  position: absolute;
}
</style>