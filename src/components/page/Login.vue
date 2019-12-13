<template>
    <div class="login-wrap">
        <div class="ms-login">
            <div class="ms-title">CIMC综合管理系统</div>
            <el-form :model="param" :rules="rules" ref="login" label-width="0px" class="ms-content">
                <el-form-item prop="username">
                    <el-input v-model="param.username" placeholder="username">
                        <el-button slot="prepend" icon="el-icon-lx-people"></el-button>
                    </el-input>
                </el-form-item>
                <el-form-item prop="password" class="pr">
                    <el-input placeholder="验证码"  v-model="param.password"><el-button slot="prepend" icon="el-icon-lx-lock"></el-button></el-input>
                    <button @click.prevent="getCode()"  class="code-btn" :disabled="disabled=!show">
                        <span v-show="show">获取验证码</span>
                        <span v-show="!show" class="count">{{count}} s</span>
                    </button>
<!--                    <el-input-->
<!--                        type="password"-->
<!--                        placeholder="password"-->
<!--                        v-model="param.password"-->
<!--                        @keyup.enter.native="submitForm()"-->
<!--                    >-->
<!--                        <el-button slot="prepend" icon="el-icon-lx-lock"></el-button>-->
<!--                    </el-input>-->
                </el-form-item>
                <div class="login-btn">
                    <el-button type="primary" @click="submitForm()">登录</el-button>
                </div>

            </el-form>
        </div>
    </div>
</template>

<script>
export default {
    data: function() {
        return {
            param: {
                username: 'admin',
                password: '123123',
            },
            rules: {
                username: [{ required: true, message: '请输入用户名', trigger: 'blur' }],
                password: [{ required: true, message: '请输入密码', trigger: 'blur' }],
            },
            show: true,  // 初始启用按钮
            count: '',   // 初始化次数
            timer: null,
        };
    },
    methods: {
        getCode(){
            let  TIME_COUNT = 60; //更改倒计时时间
            if (!this.timer) {
                this.count = TIME_COUNT;
                this.show = false;
                this.timer = setInterval(() => {
                    if (this.count > 0 && this.count <= TIME_COUNT) {
                        this.count--;
                    } else {
                        this.show = true;
                        clearInterval(this.timer);  // 清除定时器
                        this.timer = null;
                    }
                }, 1000)
            }
            let _this=this;
            this.$axios({
                url: "http://out.ccsc58.cc/OMS/v1/public/index/login/code",
                method: "post",
                data: {
                    UserName: this.param.username,
                },
                transformRequest: [
                    function(data) {
                        let ret = "";
                        for (let it in data) {
                            ret +=
                                encodeURIComponent(it) +
                                "=" +
                                encodeURIComponent(data[it]) +
                                "&";
                        }
                        return ret;
                    }
                ],
                // headers: { "Content-Type": "application/x-www-form-urlencoded;charset=UTF-8" }
            }).then(function(res) {

                if (res.data.code == "200") {
                    _this.$message.success(res.data.msg)

                }else if(res.data.code == "400"){
                    _this.$message.error(res.data.msg)

                }else if(res.data.code == "300"){//未绑定
                    _this.addSendDetailsModel2 = true

                }else if(res.data.code == "301"){//未关注

                    _this.addSendDetailsModel =true;
                }
            });

        },
        submitForm() {

            this.$refs.login.validate(valid => {
                if (valid) {
                    this.$message.success('登录成功');
                    localStorage.setItem('ms_username', this.param.username);
                    this.$router.push('/');
                } else {
                    this.$message.error('请输入账号和密码');
                    console.log('error submit!!');
                    return false;
                }
            });
        },
    },
};
</script>

<style scoped>
    .pr{
        position: relative;
    }
    .code-btn {
        width: 100px;
        height: 31px;
        position: absolute;
        top: 0px;
        right: 0px;
        z-index: 222;
        color: #fff;
        background: #66B1FF;
        font-size: 14px;
        border: none;
        border-left: 1px solid #bababa;
        padding-left: 10px;

        cursor: pointer;
    }
.login-wrap {
    position: relative;
    width: 100%;
    height: 100%;
    background-image: url(../../assets/img/login-bg.jpg);
    background-size: 100%;
}
.ms-title {
    width: 100%;
    line-height: 50px;
    text-align: center;
    font-size: 20px;
    color: #fff;
    border-bottom: 1px solid #ddd;
}
.ms-login {
    position: absolute;
    left: 50%;
    top: 50%;
    width: 350px;
    margin: -190px 0 0 -175px;
    border-radius: 5px;
    background: rgba(255, 255, 255, 0.3);
    overflow: hidden;
}
.ms-content {
    padding: 30px 30px;
}
.login-btn {
    text-align: center;
}
.login-btn button {
    width: 100%;
    height: 36px;
    margin-bottom: 10px;
}
.login-tips {
    font-size: 12px;
    line-height: 30px;
    color: #fff;
}
</style>