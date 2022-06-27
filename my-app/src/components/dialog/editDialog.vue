<template>
  <div class="dialog">
    <el-container direction="vertical" class="dialog-main">
      <el-row class="dialog-main-title">
        <el-col :span="24">Redigerar</el-col>
      </el-row>
      <el-form label-width="70px" class="add-goods-list" >
        <el-form-item label="pizza">
          <el-input v-model="updatePizza.name" placeholder="nya namn" required="required"></el-input>
        </el-form-item>
        <el-form-item label="storlek">
          <el-input v-model="updatePizza.size" placeholder="nya storlek" required="required"></el-input>
        </el-form-item>
        <el-form-item label="pris">
          <el-input v-model="updatePizza.price" placeholder="nya pris" required="required"></el-input>
        </el-form-item>
        <el-row class="dialog-main-btn" type="flex" justify="end">
          <el-col :span="12"><el-button @click="cancelled">Avbryta</el-button></el-col>
          <el-col :span="12"><el-button @click="confirmed">Bekräfta</el-button></el-col>
        </el-row>
      </el-form>
    </el-container>
    <div class="bg-fade" @click="cancelled"></div>
  </div>
</template>

<script>
  export default {
    name: "Dialog",
    props:['updatePizza'],
    data(){
      return {
      }
    },
    methods:{
      confirmed(){
        let result = this.pizzaList.filter((pizza)=>{
          return pizza.id !== this.updatePizza.id && pizza.name == this.updatePizza.name && pizza.size == this.updatePizza.size
        });
        if (result !=null && result.length > 0){
          this.$alert('pizza är redan i lista',{
            type:'waring',
            callback:action=>{}
          })
        } else{
          this.axios.put('/pizza/'+this.updatePizza.id+'/.json',{
            name:this.updatePizza.name,
            size:this.updatePizza.size,
            price:this.updatePizza.price
          })
          .then(res=>{
            this.$message('Lyckas att redigera.');
            this.cancelled()
          })
        }
      },
      /*关闭窗口*/
      cancelled(){
        this.$emit('closed');
      }
    },
    computed:{
      pizzaList(){
        return this.$store.getters.getMenuPizza;
      }
    },
    created(){
    }
  }
</script>

<style scoped>
  .dialog-main{
    width: 30%;
    height: auto;
    z-index: 99;
    background: #fff;
    position: absolute;
    top: 50%;
    left: 50%;
    padding: 20px 40px;
    -webkit-transform: translateY(-60%) translateX(-50%);
    -moz-transform: translateY(-60%) translateX(-50%);
    -ms-transform: translateY(-60%) translateX(-50%);
    -o-transform: translateY(-60%) translateX(-50%);
    transform: translateY(-60%) translateX(-50%);
    color: #000;
    -webkit-border-radius: 8px;
    -moz-border-radius: 8px;
    border-radius: 8px;
    -webkit-box-shadow: 1px 1px 5px #696969;
    -moz-box-shadow: 1px 1px 5px #696969;
    box-shadow: 1px 1px 5px #696969;
  }
  .bg-fade{
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0,0,0,.5);
    z-index: 1;
  }
  .dialog-main-title{
    height: 50px;
    line-height: 50px;
    text-align: center;
    margin-bottom: 25px;
  }
  .dialog-main-btn{
    text-align: center;
    margin: 20px 0;
  }
</style>
