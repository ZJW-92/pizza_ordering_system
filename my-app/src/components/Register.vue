<template>
  <el-container class="login-register-main">
    <el-form :model="user" label-width="80px" @submit.native.prevent>   <!--@submit.native.prevent-->
      <el-row class="login-title" type="flex" justify="center">
        <el-col :sapn="24"><h1>Registrera dig</h1></el-col>
      </el-row>
      <el-form-item label="Användarnamn">
        <el-input type="email" v-model="user.account" placeholder="Ange mejladress" required="required"></el-input>
      </el-form-item>
      <el-form-item label="Lösenord">
        <el-input type="password" v-model="user.password" placeholder="Ange lösenord" required="required"></el-input>
      </el-form-item>
      <el-form-item label="Repeterar">
        <el-input type="password" v-model="user.confirmPsd" placeholder="Repeterar lösenord" required="required"></el-input>
      </el-form-item>
      <el-form-item class="submit-btn">
        <button @click="onSubmit">Registrera dig</button>
      </el-form-item>
    </el-form>
  </el-container>
</template>

<script>
  // import axios from 'axios'
    export default {
        name: "Register",
      data(){
        return {
          user:{
            account: '',
            password:'',
            confirmPsd:'',
          },
          userAccounts:[],
          dialogVisible:false,
          event:null,
        }
      },
      created(){
        this.selectData();
      },
      methods:{
        selectData(){
          this.axios.get('/users.json')
            .then((result)=>{
              let arr = [];
              for (let key in result.data){
                arr.push(result.data[key])    //获取用户信息的json对象
              }
              arr.forEach((val)=>{
                for (let key in val){
                  if(key == 'account'){
                    this.userAccounts.push(val[key]);       //获取已经注册了的用户账号
                  }
                }
              })
            })
        },
        onSubmit(){
          if (this.user.account !== "" && this.user.password !== "" && this.user.confirmPsd !== ""){
            let isNull =1;
            let regEMail = /^[A-Za-z0-9-_.]+@[a-zA-Z0-9_-]+(\.[a-zA-Z0-9_-]+)+$/;   //邮箱正则表达式
            this.userAccounts.forEach((val)=>{
              if(val===this.user.account){
                isNull = 0;
              }
            });
            if(isNull==0){
              this.$alert("Användarnamn är redan existerade.",{
                type: 'warning',
                callback:action=>{}
              });
            }
            else if (!regEMail.test(this.user.account)){      //验证邮箱
              this.$alert('Mejladress är inte korrekt.',{
                type:'error',
                callback:action=>{}
              })
            }
            else if (this.user.password.length <6 || this.user.password.length >18) {
              this.$alert('Längden av lösenord är mellan 6-18 siffror.',{
                type:'warning',
                callback:action=>{}
              })
            }
            else if (this.user.password !== this.user.confirmPsd){
              this.$alert('Lösenorden är olika.',{
                type:'warning',
                callback:action=>{}
              })
            }
            else{
              this.signUp();
            }
          }
        },
        signUp(){
          this.$confirm('Är du säker att registrear dig?',{
            confirmButtonText:'Bekräfta',
            cancelButtonText:'Avbryta',
            type:'info'
          }).then(response=>{
            this.axios.post('/users.json',this.user)
              .then(response=>{
                this.$router.push('/login')
              });
            this.$message({type:'info',message:'Lyckas att registrera dig'})
          }).catch(response=>{
            this.$message({type:'info',message:'Avbryta att registrera dig'})
          })
        },
      }
    }
</script>

<style scoped lang="less">
  .el-form-item{
    margin-left: -20px;
    input {
      -webkit-appearance: none;
      background-color: #fff;
      background-image: none;
      border-radius: 4px;
      border: 1px solid #dcdfe6;
      -webkit-box-sizing: border-box;
      box-sizing: border-box;
      color: #ccc;
      display: inline-block;
      font-size: inherit;
      height: 40px;
      line-height: 40px;
      outline: 0;
      padding: 0 15px;
      -webkit-transition: border-color .2s cubic-bezier(.645,.045,.355,1);
      transition: border-color .2s cubic-bezier(.645,.045,.355,1);
      width: 100%;
    }
  }
</style>
