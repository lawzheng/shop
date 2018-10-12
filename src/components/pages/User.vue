<template>
  <div>
    <van-nav-bar
      title="会员中心"
      left-text="返回"
      right-text="退出"
      left-arrow
      @click-left="onClickLeft"
      @click-right="onClickRight"
    />
    <div class="user-card">
      <div class="image">
        <!-- <img src="@/assets/images/me.jpg" alt=""> -->
      </div>
      <div class="userName">
        <span class="name">用户名:{{userName}}</span>
        <span class="score">积分:189496</span>
      </div>
      <div class="userControl">
        <img src="@/assets/images/settings.png" alt=""> 账号管理
      </div>
    </div>
    <van-row class="user-links">
      <van-col span="6">
        <van-icon name="pending-payment" color="#ff9900" />
        待付款
      </van-col>
      <van-col span="6">
        <van-icon name="logistics" color="#ff9900" />
        待收货
      </van-col>
      <van-col span="6">
        <van-icon name="cash-back-record" color="#ff9900" />
        退/换货
      </van-col>
      <van-col span="6">
        <van-icon name="pending-orders" color="#f10215" />
        全部订单
      </van-col>
    </van-row>
    <van-cell-group>
      <van-cell icon="certificate" title="收藏商品" is-link />
      <van-cell icon="gold-coin" title="我的优惠券" is-link />
      <van-cell icon="gift" title="我收到的礼物" is-link />
    </van-cell-group>
  </div>
</template>

<script>
import axios from 'axios'
import url from '@/serviceAPI.config.js'

export default {
  data(){
    return{
      errorImg:"http:\/\/images.baixingliangfan.cn\/firstCategoryPicture\/20180408\/20180408112010_4489.png",
      userName:''
    }
  },
  created(){
    // this.getUserInfo()
    this.userName=localStorage.userInfo
  },
  methods: {
    onClickLeft() {
      this.$router.go(-2)
    },
    onClickRight() {
      localStorage.clear()
      this.$router.push({name:'ShoppingMall'})
    },
    //取用户信息
    // getUserInfo(){
    //   axios({
    //     url:url.getUserInfo,
    //     method:'post',
    //     data:{
    //             userName:localStorage.userInfo
    //         }
    //   })
    //   .then(response=>{
    //     // console.log(response)
    //     if(response.data.code == 200 && response.data.message.length ){
    //         this.goodList=this.goodList.concat(response.data.message)
    //     }else{
            
    //     }
    //     this.loading=false;
    //     // console.log(this.finished)
    //   })
    //   .catch(error=>{
    //     console.log(error)
    //   })
    // }
  },
};
</script>

<style lang="less">
.user {
  &-poster {
    width: 100%;
    display: block;
  }
  &-group {
    margin-bottom: .3rem;
  }
  &-links {
    padding: 15px 0;
    font-size: 12px;
    text-align: center;
    background-color: #fff;
    .van-icon {
      display: block;
      font-size: 24px;
    }
  }
  &-card {
    width: 93%;
    height: 7rem;
    margin: 10px auto;
    border-radius: 7px;
    background: linear-gradient(90deg,#eb3c3c,#ff7459);
    background: -webkit-linear-gradient(left,#eb3c3c,#ff7459);
    box-shadow: 0 2px 5px rgba(255,98,98,.4);
    position: relative;
    overflow: hidden;
    .image{
      width: 60px;
      height: 60px;
      position: absolute;
      left: 15px;
      top: 50%;
      transform: translateY(-50%);
      border-radius: 50%;
      overflow: hidden;
      background: #fff;
      img{
        width: 100%;
      }
    }
    .userName{
      width: 140px;
      height: 70px;
      position: absolute;
      left: 80px;
      top: 50%;
      transform: translateY(-50%);
      font-size: 14px;
      text-align: left;
      color: #fff;
      .name{
        margin-top: 10px;
        /*width: 100%;*/
        height: 20px;
        display: block;
      }
      .score{
        /*width: 100%;*/
        height: 20px;
        display: block;
        margin-top: 5px;
      }
    }
    .userControl{
      float: right;
      font-size: 12px;
      margin-top: 15px;
      margin-right: 5px;
      img{
        width: 12px;
      }
    }
  }
}
</style>