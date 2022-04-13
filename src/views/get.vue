<template>
<div class="get">
  <h1> لیست آدرس ها </h1>
  <!-- get list wath API -->
  <div class="list" v-for="data in this.datas">
    <h1>آدرس: {{data.address}}</h1>
    <div class="name">
      <h1>نام: {{data.first_name}} {{data.last_name}}</h1>
      <h1>تلفن: {{ data.coordinate_mobile }}</h1>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: "get",
  data(){
    return{
      datas:null
    }
  },
  created() {
      //get list wath API
      fetch('https://stage.achareh.ir/api/karfarmas/address', {
        method: 'GET',
        headers: { 'Content-Type': 'application/json',
            'Authorization': 'Basic '+btoa('09822222222:Sana12345678')
        },
      })
      .then(res => res.json())
      .then(json => this.datas = json)
  }
}
</script>
<style scoped>
.get{
  display: flex;
  flex-direction: column;
  align-items: center;
}
.list{
  width: 80%;
  height: 150px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: space-evenly;
  background-color: #ece9e1;
  border:2px solid black;
  padding: 10px;
  margin: 10px;
  border-radius: 10px;
}
.name{
  width: 100%;
  display: flex;
  justify-content: space-around;
  align-items: center;
}

</style>