<template>
    <div class="ma">
        <!-- 登录框靠右       <div class="mi"> -->
    <v-content>
        <v-snackbar v-model="messageBar" color="error" :timeout="2000" :top="true">{{ message }}</v-snackbar>

        <v-row align="center" justify="center" style="margin-top: 12%;">
            <h1 class="display-2 font-weight-thin">{{base.Title}}</h1>
        </v-row>
        <br>
        <v-card class="mx-auto" max-width="400">
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
        </v-card>
        <div class="mt-8 text-center">Copyright © 2024 Covteam</div>

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
            }).catch(() => this.base.Title = '2024年贵州省卫生健康系统网络安全技能竞赛-团队赛')
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

</style>
