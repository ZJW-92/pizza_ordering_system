<template>
  <el-container direction="vertical" class="app-main">
    <el-row class="app-home-bg">
      <el-col class="app-home-content" :span="24">
        <h2>Välkommen till Pizzaria!</h2>
        <el-button class="btn" @click="goTo">Beställa nu!</el-button>
      </el-col>
    </el-row>
  </el-container>
</template>

<script>
  import bus from '../eventBus/index'
    export default {
        name: "Home",
      methods:{
        goTo(){
          this.$router.push('/menu');
          bus.$emit('changeMenu',2)       //把2传给header组件（兄弟组件间传值）
        }
      }
    }
</script>

<style scoped lang="less">
  .app-main{
    .app-home-bg{
      background: url("../assets/pizza.jpg");
      height: 85vh;
      min-height: 500px;
      padding: 15%;
      .app-home-content{
        line-height: 180px;
        background: transparent;
        margin: 0 auto;
        text-align: center;
        padding: 15%;
        line-height: 60px;
        color: yellow;
        font-size: 40px;
        button{
          background: #b5258c;
          border: 0;
          color: white;
        }
        button:hover{
          background: #b5258c;
          color: gold;
        }
      }
    }
  }
</style>
