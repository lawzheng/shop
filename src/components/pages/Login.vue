<template>
    <div>
       <van-nav-bar
        title="用户登录"
        left-text="返回"
        left-arrow
        @click-left="goBack"
        />
        <div class="register-panel">
            <van-field
                v-model="username"
                label="用户名"
                icon="clear"
                placeholder="请输入用户名"
                required
                @click-icon="username = ''"
                :error-message='usernameErrorMsg'
            />
            <van-field
                v-model="password"
                type="password"
                label="密码"
                placeholder="请输入密码"
                required
                :error-message='passwordErrorMsg'
            />
            <div class="button">
                <van-row>
                    <van-col span="12">
                        <van-button type="primary" @click="goRegister" :loading="openLoading" size="large">注册</van-button>
                    </van-col>
                    <van-col span="12">
                        <van-button type="primary" @click="LoginAction" :loading="openLoading" size="large">登录</van-button>
                    </van-col>
                </van-row>
                
            </div>
       </div>
    </div>
</template>
<script>
    import axios from 'axios'
    import url from '@/serviceAPI.config.js'
    import { Toast } from 'vant'
    export default {
        data() {
            return {
                username: '',
                password: '',
                openLoading: false, //是否开启用户的Loading
                usernameErrorMsg:'',   //当用户名出现错误的时候
                passwordErrorMsg:'',   //当密码出现错误的时候
                active:''
            }
        },
        created(){
            this.active = this.$route.params.active || ''
            this.username = this.$route.params.username || ''
            this.password = this.$route.params.password || ''
            if(this.username){
                this.axiosLoginUser()
            }
            // if(localStorage.userInfo){
            //     // Toast.success('您已经登录')
            //     this.$router.push('/user')
            // }
        },
        methods: {
            goBack() {
                this.$router.go(-1)
            },
            goRegister(){
                this.$router.push({name:'Register'})   
            },
            LoginAction(){
                if(this.checkForm()){
                    this.axiosLoginUser()
                }
            },
            checkForm(){
                let isOk= true
                if(this.username.length<2){
                    this.usernameErrorMsg="用户名不能小于2位"
                    isOk= false
                }else{
                    this.usernameErrorMsg=''
                }
                if(this.password.length<6){
                    this.passwordErrorMsg="密码不能少于6位"
                    isOk= false
                }else{
                    this.passwordErrorMsg=''
                }
                return isOk
            },
            //*********axios登录用户方法********
            axiosLoginUser(){
                this.openLoading=true
                axios({
                    url: url.login,
                    method: 'post',
                    data:{
                        userName:this.username,
                        password:this.password 
                    }
                })
                .then(response => {
                    console.log(response)
                    //如果返回code为200，代表登录成功，我们给用户作Toast提示
                    if(response.data.code == 200){
                        new Promise((resolve,reject)=>{
                            localStorage.userInfo=this.username
                            setTimeout(()=>{
                                resolve()
                            },300)
                        }).then(()=>{
                            Toast.success('登录成功')
                            if(this.active==2){
                                this.$router.replace('/cart')
                            }else{
                                this.$router.replace('/user')
                            }
                        }).catch(err=>{
                            Toast.fail('登录状态保存失败')
                            console.log(err)
                        })
                    }else{
                        // console.log(response.data.message)
                        Toast.fail('用户名或密码错误')
                        this.openLoading=false
                    }
                })
                .catch((error) => {   
                    Toast.fail('登录失败') 
                    this.openLoading=false 
                })
            },
        },
        
    }
</script>
<style scoped>
    .register-panel{
        width:96%;
        border-radius: 5px;
        margin:20px auto;
        padding-bottom:50px;
    }
    .van-col{
        padding: 5px;
    }
</style>