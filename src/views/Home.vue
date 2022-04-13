<template>
  <div class="post">

    <!-- info form -->
    <div class="info">
      <div class="title"><h6>لطفا اطلاعات ورود خود را وارد نمایید</h6></div>
        <!-- inputs -->
        <div class="forms">
          <input v-model="name" class="input" type="text" placeholder="نام">
          <input v-model="family" class="input" type="text" placeholder="نام خانوادگی">
          <input v-model="phone" class="input" type="text" placeholder="تلفن همراه">
          <input v-model="tel" class="input" type="text" placeholder="تلفن ثابت">
        </div>
    <!-- more inputs -->
    <div class="more">
        <input v-model="location" class="input" type="text" placeholder="آدرس دقیق">

      <!-- radio button -->
      <div class="radiobtn">
        <h1>جنسیت</h1>
      <div class="btn">
        <button v-if="radio == 'female'" @click="radio = 'male'" class="button is-info is-outlined is-small radios">آقا</button>
        <button v-if="radio == 'male'" @click="radio = 'male'" class="button is-info is-small radios">آقا</button>
        <button v-if="radio == 'male'" @click="radio = 'female'" class="button is-info is-outlined is-small radios">خانم</button>
        <button v-if="radio == 'female'" @click="radio = 'female'" class="button is-info is-small radios">خانم</button>
      </div>  
    </div>
      <!--submit form and next page -->
      <button @click="post" class="button is-primary submit">مرحله بعد</button>
    </div>
  </div>
 </div>
</template>

<script>
import { join } from "path"
import swal from 'sweetalert'
import { log } from 'console'

export default {
  name: "post",
  data(){
    return{
      name:null,
      family:null,
      phone:null,
      tel:null,
      location:null,
      radio:'male',
    }
  },
  methods:{
    //submit form and go to next page
    post(){
      if(this.name != null && this.family != null && this.phone != null && this.tel != null && this.location != null){
        if(this.phone.length == 11 ){

          //create object wath data for submit form
          let obj = {
            name: this.name,
            family: this.family,
            phone: this.phone,
            tel: this.tel,
            location: this.location,
            radio: this.radio
          }
          
          //add obj to localstorage for netx page
          localStorage.setItem('obj',JSON.stringify(obj))

          //go to next page
          this.$router.push('/map')
        }else{
           swal({
            icon: "error",
            title: "خطا",
            text: 'تعداد شماره تلفن یا شماره موبایل اشتباه میباشد!'
          });
        }
   
      }else{
         swal({
          icon: "error",
          title: "خطا",
          text: "لطفا کادر ها را پر نمایید"
         });
      }
    }
  }
}
</script>

<style scoped>
input::-webkit-input-placeholder {
  color: #2483b3;
}
.post{
  width: 100%; height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
}
.title{
  width: 100%; height: 60px;
  display: flex;
  justify-content: flex-start;
  align-items: center;
  background-color: #f2f2f2;
  padding: 15px;
  color: gray;
}
.info{
  width: 90%; height: 70vh;
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  align-items: center;
}
.forms{
  width: 100%; height: 50vh;
  display: flex;
  flex-direction: column;
  justify-content: space-evenly;
  align-items: center;
}
.more{
  width: 100%; height: 3vh;
  margin-top: 30px;
}
.radiobtn{
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 20px;
}
.radios{
  width: 60px;
  border-radius: 0px;
}
.submit{
  width: 100%;
  margin-top: 20px;
}
h6{
  font-size: 15px;
}
</style>
