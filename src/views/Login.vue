<template>
    <div class="ma">
        
        <v-content>
        <v-snackbar v-model="messageBar" color="error" :timeout="2000" :top="true">{{ message }}</v-snackbar>

            <v-row align="center" justify="center" style="margin-top: 22%;">  <!--中央标题字体-->
            <h1 class="display-2 font-weight-thin">{{base.Title}}</h1>
        </v-row>
        <br>


        <v-card class="login_container" max-width="400">
            <v-card-title>{{$t('login.title')}}</v-card-title>
            <v-card-text>
                <v-form ref="form">
                    <v-text-field
                            v-model="inputForm.Name"
                            :rules="nameRules"
                            :label="$t('login.account')"
                            required
                            autocomplete="off"
                    />
                    <v-text-field
                            v-model="inputForm.Password"
                            :rules="passwordRules"
                            :label="$t('login.password')"
                            type="password"
                            required
                            autocomplete="off"
                    />
                </v-form>
            </v-card-text>

            <v-card-actions>
                <v-btn text color="primary" @click="onLogin">{{$t('login.login')}}</v-btn>
                <v-btn text @click="onReset">{{$t('login.reset')}}</v-btn>
            </v-card-actions>

        <div class="mt-8 text-center">Copyright © 2024 Covteam</div>
        </v-card>

        <!-- 登录等待 -->
        <v-dialog v-model="isLoading" hide-overlay persistent width="300">
            <v-card dark>
                <v-card-text>
                    <p>{{$t('login.loading')}}</p>
                    <v-progress-linear indeterminate color="white" class="mb-0"/>
                </v-card-text>
            </v-card>
        </v-dialog>

    </v-content>
    <!-- </div> --> 
</div>

</template>

<script>
    /* eslint-disable */
    export default {
        name: "Login",

        data() {
            return {
                isLoading: false,
                messageBar: false,
                message: '',

                base: {
                    Title: ''
                },

                nameRules: [
                    v => !!v || this.$i18n.t('login.account_empty')
                ],
                passwordRules: [
                    v => !!v || this.$i18n.t('login.password_empty')
                ],
                inputForm: {
                    Name: '',
                    Password: ''
                }
            }
        },

        created() {
            this.utils.GET('/base').then(res => {
                this.base = res
            }).catch(() => this.base.Title = '')
        },

        methods: {
            onLogin() {
                if (!this.$refs.form.validate()) {
                    return
                }
                this.utils.POST("/login", this.inputForm, false).then(res => {
                    localStorage.setItem('token', res)
                    this.$router.push('/')
                }).catch(err => {
                    this.messageBar = true
                    this.message = err.response.data.msg
                })
            },
            onReset() {
                this.inputForm = {
                    Name: '',
                    Password: ''
                }
            }
        }
    }
</script>

<style scoped>

.ma {
  /* //这里地址是用项目中图片所在路径为准 */
  background: url("http://47.101.181.195:8000/a.png");
  /* //将图片样式不重复 */
  background-repeat: no-repeat;
  /* //设置图片大小 */
  background-size: 100%;
  /* 设置全屏 */
  position: fixed;
  /* 设置div高度 */
  height: 100%;
  /* 设置div宽度 */
  width: 100%;
}
.mi {
  width: 400px;
  height: 400px;
  padding-right: 20px;
  padding-top: 20px;
  position: fixed;
  top: 50%;
  right: 50%;
  background-color: rgba(255, 255, 255, 0);
  border: none;
}
.login_container{
   

    position:absolute;
    /*定位方式绝对定位absolute*/
    top:140%;
    left:50%;
    /*顶和高同时设置50%实现的是同时水平垂直居中效果*/
    transform:translate(-50%,-50%);
    /*实现块元素百分比下居中*/
    width:450px;
    height: 350px;
    padding:50px;
    background: rgba(0,0,0,.5);
    /*背景颜色为黑色，透明度为0.8*/
    box-sizing:border-box;
    /*box-sizing设置盒子模型的解析模式为怪异盒模型，
    将border和padding划归到width范围内*/
    box-shadow: 0px 15px 25px rgba(0,0,0,.5);
    /*边框阴影  水平阴影0 垂直阴影15px 模糊25px 颜色黑色透明度0.5*/
    border-radius:15px;
    /*边框圆角，四个角均为15px*/

}
</style>
