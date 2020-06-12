<template>
  <div>
  	<div class="login_box" onmouseout="style.opacity='0.8'" onmouseover="style.opacity='1'">
      <el-tabs type="border-card">
        <el-tab-pane label="Log in">
          <div class="top_bar">
            <div class="name">在线聊天应用——登录</div>
          </div>
          <div style="height: 30px;"></div>
          <i class="fa fa-user-o fa-2x" aria-hidden="true" style="float:left"></i>
          <el-input v-model="userName" placeholder="用户名" style="width:250px"></el-input>
          <div style="height: 30px;"></div>
          <i class="fa fa-address-book fa-2x" aria-hidden="true" style="float:left"></i>
          <el-input v-model="password" placeholder="密码" style="width:250px" show-password></el-input>
          <div style="height: 50px;"><span class="login_tips">{{login_error}}</span></div>
          <el-button type="success" icon="el-icon-check"  @click="login" circle></el-button>
          <div style="height: 140px;"></div>
        </el-tab-pane>
        <el-tab-pane label="Sign up">
          <div class="top_bar">
            <div class="name">在线聊天应用——注册</div>
            <div class=""></div>
          </div>
          <div style="height: 20px;"></div>
          <i class="fa fa-user-o fa-2x" aria-hidden="true" style="float:left"></i>
          <el-input v-model="userName2" placeholder="用户名" style="width:250px"></el-input>
          <div style="height: 30px;"></div>
          <i class="fa fa-address-book fa-2x" aria-hidden="true" style="float:left"></i>
          <el-input v-model="password2" placeholder="密码" style="width:250px" show-password></el-input>
          <div style="height: 30px;"></div>
          <i class="fa fa-quora fa-2x" aria-hidden="true" style="float:left"></i>
          <el-input v-model="nickname" placeholder="昵称" style="width:250px"></el-input>
          <div style="height: 30px;"><span class="register_tips">{{register_error}}</span></div>
          <el-button type="success" icon="el-icon-check"  @click="register" circle></el-button>
          <div style="height: 140px;"></div>
        </el-tab-pane>
      </el-tabs>
  	</div>
  </div>
</template>

<script src="https://code.jquery.com/jquery-3.4.1.min.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script src="https://cdn.bootcss.com/lodash.js/4.17.15/lodash.min.js"></script>
<script>  
export default {
  name: 'login',
  data () {
    return {
      nickname:'',
      userName:'',
      password:'',
      userName2:'',
      password2:'',
      login_state:-1,   // 记录登录的请求状态
      login_error:'',
      register_error:'',
    }
  },
	mounted: function(){
    
	},
  methods: {
  	async login(){
      // 调用 LarkCloud 函数
      await axios.post(
          'https://afwt8c.toutiao15.com/login',
          { userName: this.userName,
            password: this.password,
          }
      ).then((res)=>{
          // 处理正常结果
          const data = res.data;             
          this.login_state = data.state;             
          console.log("处理正常结果！");
          if(data.state!=0){
            //alert(data.info)
            this.login_error = data.info;
            var _this = this;
            setTimeout(function(){
              _this.login_error = '';
            }, 1500);
            return;
          }
          else{
            console.log("登录成功！");
            var userInfo = {'userName': this.userName};
            var session = window.sessionStorage;      // 使用一个session对象保存登录状态
            session.setItem('user', this.userName);   // 记录登录的用户
            this.$router.push({name: 'chat', params: userInfo});
          }
      }).catch(function(error) {
          // 处理异常结果
          console.log("处理异常结果！");
          console.log(JSON.stringify(error));
          console.log(error.result);
      }).finally(function() {
          console.log('调用登录完成！');
      }); 
    },
    register(){
      var base64str = 'data:image/jpeg;base64,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'
      // 调用 LarkCloud 函数
      axios.post(
          'https://afwt8c.toutiao15.com/register',
          { userName: this.userName2,
            password: this.password2,
            nickname: this.nickname,
            headImg: base64str
          }
      ).then((res)=> {
          // 处理正常结果
          const data = res.data;
          this.register_error = data.info;
          var _this = this;
          setTimeout(function(){
            _this.register_error = '';
          }, 1500);
          return;
          console.log("data:", data);
      }).catch(function(error) {
          // 处理异常结果
          console.log(JSON.stringify(error));
          console.log(error.result);
      }).finally(function() {
          console.log('调用注册完成！');
      });
    }
  }
}
</script>

<style scoped>
	.login_box {
    opacity: 0.8;
		width: 350px;
		height: 500px;
		background: #f5f5f5;
		border-radius: 4px;
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%,-50%);
		-webkit-transform: translate(-50%,-50%);
		overflow: hidden;
		box-shadow: 0 0 10px 0px #999;
	}
	.top_bar {
		width: 100%;
		height: 64px;
	}
	.top_bar .name {
		float: left;
		font-size: 20px;
		line-height: 64px;
		margin-left: 26px;
		color: #4a4a4a;
	}
	.login_tips{
    float: left;
    margin-top: 10px;
    font-size: 12px;
    color: #999;
  }
  .register_tips{
    float: left;
    margin-top: 5px;
    font-size: 12px;
    color: #999;
  }
</style>