<template>
    <div class="login_container">
       <div class="login_box">
           <!-- 头像区域 -->
           <div class="avatar_box"> 
               <img src="../assets/logo.png" alt="">
           </div>
           <!-- 登陆表单 -->
           <el-form ref="loginFormRef" :model="loginForm" :rules="logniFormRules" label-width="0px" class="login_form">
               <!-- 用户名 -->
           <el-form-item prop="username">
            <el-input v-model="loginForm.username" prefix-icon="iconfont icon-touxiang" ></el-input>
            </el-form-item>
            <!-- 密码 -->
             <el-form-item prop="password">
            <el-input v-model="loginForm.password" prefix-icon="iconfont icon-mima" type="password"></el-input>
            </el-form-item>
            <!-- 按钮区域 -->
             <el-form-item class="btns">
                <el-button type="primary"  @click="login">登录</el-button>
                <el-button type="info" @click="restLoginForm">重置</el-button>
             </el-form-item>
           </el-form>
       </div>
    </div>
</template>

<script>
export default {
    data(){
        return{
            //登录表单对象
            loginForm:{
                username:'admin',
                password:'123456'
            },
            //表单的验证规则对象
            logniFormRules:{
                //验证用户名
                username:[
                     { required: true, message: '请输入用户名', trigger: 'blur' },
                      { min: 1, max: 10, message: '长度在 1 到 10 个字符', trigger: 'blur' }
                ],
                //验证密码
                password:[
                     { required: true, message: '请输入密码', trigger: 'blur' },
                      { min: 3, max: 15, message: '长度在 3 到 15 个字符', trigger: 'blur' }
                ]
            }
        }
    },
    methods:{
        //重置按钮，重置登录表单
        restLoginForm(){
            this.$refs.loginFormRef.resetFields();
        },
        login(){
            this.$refs.loginFormRef.validate(async valid=>{
                if(!valid)return;
                const {data:res}= await this.$http.post('login',this.loginForm);
                if(res.meta.status!=200) return this.$message.error('登录失败！');
                this.$message.success('登录成功！');
                // 调用sessionStorage保持token
                window.sessionStorage.setItem("token",res.data.token);
                this.$router.push('/home')

            });
        }
    }
}
</script>

<style lang="less" scoped>
.login_container{
    background: #2b4b6b;
    height: 100%;
}

.login_box{
    width: 450px;
    height: 300px;
    background: #fff;
    border-radius: 3px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
}

.avatar_box{
    height: 130px;
    width: 130px;
    border: 1px solid #eee;
    border-radius: 50%;
    padding: 10px;
    box-shadow: 1 1 2 10px #ddd;
    position: absolute;
    left: 50%;
    transform: translate(-50%,-50%);
    background: #fff;
    img{
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background:#eee;
    }

}

.btns{
    display: flex;
    justify-content: flex-end;
}

.login_form{
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0px 20px;
    box-sizing: border-box;
}
</style>


