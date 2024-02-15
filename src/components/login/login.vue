<template>
    <div class="main-page">
        <div class="close" @click="close">
            <img :src="require('../../assets/login/back.png')" />
            <span>Back</span>
        </div>
        <div class="login-info">
            <div class="login-logo">
                <div class="login-logo-box">
                    <img class="login-logo-img" :src="require('../../assets/login/logo.png')" />
                </div>
            </div>
            <div class="welcome">Welcome to Flcarrico. Have a good time!</div>
            <div class="main-form">
                <div id="name-box" class="login-info-item" @click="nameFocus">
                    <div class="v-box" style="position:relative;">
                        <div><img :src="require('../../assets/login/phone.png')" /></div>
                        <div style="height:40px;line-height:40px;font-size:16px;color:#fff">+55</div>
                        <input class="v-input" ref="loginname" type="text" :placeholder="GLOBAL.lanLocal['mobile']" @focus="nameFocus" @blur="nameBlur"
                            @input="nameInput" v-model="loginname" />
                    </div>
                </div>
                <div class="msg-box">
                    <div class="login-info-message error-info">{{ nameinfo }}</div>
                </div>
                <div id="passcode-box" class="login-info-item" @click="passcodeFocus">
                    <div class="v-box">
                        <div><img :src="require('../../assets/login/key.png')" /></div>
                        <input class="v-input" :placeholder="GLOBAL.lanLocal['passcode']" ref="passcode" type="password"
                            @focus="passcodeFocus" @blur="passcodeBlur" @input="passcodeInput" v-if="!isEye" v-model="passcode" />
                        <input class="v-input" :placeholder="GLOBAL.lanLocal['passcode']" ref="passcode" type="text"
                            @focus="passcodeFocus" @blur="passcodeBlur" @input="passcodeInput" v-else v-model="passcode" />
                    </div>
                    <span class="v-icon mdi" :class="eyeClass" v-if="!isEye" @click="changeEye">
                        <img class="eye-img" :src="require('../../assets/login/pwd-normal.png')" />
                    </span>
                    <span class="v-icon mdi" :class="eyeClass" v-else @click="changeEye">
                        <img class="eye-img" :src="require('../../assets/login/pwd-show.png')" />
                    </span>
                </div>
                <div class="msg-box">
                    <div class="login-info-message error-info">{{ passcodeinfo }}</div>
                </div>
                <div class="bar-box">
                    <div class="left-box">
                        <img class="remember-img" v-if="isRemember" @click="changeRemember"
                            :src="require('../../assets/login/selected.png')" />
                        <img class="remember-img" v-else @click="changeRemember"
                            :src="require('../../assets/login/normal.png')" />
                        <span class="keep-login">{{ GLOBAL.lanLocal['keeplogin'] }}</span>
                    </div>
                    <!-- <span class="forget-passcode" @click="toForgot">{{ GLOBAL.lanLocal['forgotpasscode'] }}?</span> -->
                </div>
                <div class="button-box">
                    <input class="login-button"
                        :class="nameempty || nameerror || passcodeempty || passcodeerror ? 'login-button-disable' : ''"
                        type="button" @click="doLogin" :value="GLOBAL.lanLocal['login']" />
                </div>
                <div class="tag-box">
                    <div class="reg" @click="changeTag()">
                        <div>{{GLOBAL.lanLocal['regnow'] }}</div>
                        <div class="border-bottom"></div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
  
<script>
import md5 from 'js-md5';
import loadFile from "../../api/loadFile.js"
import AES from "../../common/AES.js"
import { doPost } from '../../api/api.js'
import decodeApiLan from '../../api/decodeApiLan.js'
export default {
    name: 'login',
    data() {
        return {
            isEye: false,
            isRemember: true,
            loginname: '',
            passcode: '',
            nameerror: false,
            namefocus: false,
            nameempty: true,
            nameinfo: '',
            passcodeerror: false,
            passcodefocus: false,
            passcodeempty: true,
            passcodeinfo: '',
            innerHeight: 0,
        }
    },
    created() {
        this.innerHeight = window.innerHeight - 60
    },
    computed: {
        loginClass() {
            return {
                'error-info': this.nameerror,
                'normal-info': !this.nameerror && this.namefocus,
                'v-label-active': this.namefocus || !this.nameempty
            }
        },
        passcodeClass() {
            return {
                'error-info': this.passcodeerror,
                'normal-info': !this.passcodeerror && this.passcodefocus,
                'v-label-active': this.passcodefocus || !this.passcodeempty
            }
        },
        eyeClass() {
            return {
                'error-info': this.passcodeerror,
                'normal-info': !this.passcodeerror && this.passcodefocus
            }
        }
    },
    methods: {
        close() {
            this.$emit('close')
        },
        changeTag() {
            this.$emit("reg")
        },
        changeEye() {
            this.isEye = !this.isEye
        },
        changeRemember() {
            this.isRemember = !this.isRemember
        },
        nameFocus() {
            this.$refs.loginname.focus()
            this.namefocus = true
            // this.checkName()
        },
        nameBlur() {
            this.namefocus = false
            this.checkName()
        },
        nameInput() {
            this.checkName()
        },
        checkName(needResult = false) {
            let loginname = this.loginname.trim()
            if(loginname.length <= 0)
            {
                if(needResult) {
                    return false
                }
                this.nameempty = true
                this.nameerror = true
                this.nameinfo = this.GLOBAL.lanLocal['plzinput']
            } else {
                if (needResult) {
                    return true
                }
                this.nameempty = false
                this.nameerror = false
                this.nameinfo = ''
            }
        },
        passcodeFocus() {
            this.$refs.passcode.focus()
            this.passcodefocus = true
            // this.checkPasscode()
        },
        passcodeBlur() {
            this.passcodefocus = false
            this.checkPasscode()
        },
        passcodeInput() {
            this.checkPasscode()
        },
        checkPasscode(needResult = false) {
            if (this.passcode.length <= 0) {
                if (needResult) {
                    return false
                }
                this.passcodeempty = true
                this.passcodeerror = true
                this.passcodeinfo = this.GLOBAL.lanLocal['plzinput']
            } else if (this.passcode.length < 6) {
                if (needResult) {
                    return false
                }
                this.passcodeempty = false
                this.passcodeerror = true
                this.passcodeinfo = this.GLOBAL.lanLocal['sixpasscode']
            } else {
                if (needResult) {
                    return true
                }
                this.passcodeempty = false
                this.passcodeerror = false
                this.passcodeinfo = ''
            }
        },
        doLogin() {
            let that = this
            if (this.checkName(true) && this.checkPasscode(true)) {
                const passcode = md5(this.passcode)
                let loginname = this.loginname.trim()
                doPost({
                    n: 'AppGame',
                    a: 'do_login',
                    mobile: loginname,
                    password: passcode
                }).then((res) => {
                    if (res.code === 0) {
                        const json = loadFile("static/nonce", true);
                        if (json != null) {
                            let api_aes_key = json['api_aes_key']
                            localStorage.setItem(md5('__name__'), AES.encrypt(loginname, api_aes_key))
                            localStorage.setItem(md5('__pwd_'), AES.encrypt(passcode, api_aes_key))

                            this.GLOBAL.userInfo.name = loginname
                            this.GLOBAL.userInfo.passcode = passcode

                            that.$emit('reload');
                        }
                    } else {
                        const data = decodeApiLan(res, this.GLOBAL.lanArr);
                        this.$toast.error(data.message, {
                            position: "top-center",
                            timeout: 2000,
                            closeOnClick: true,
                            pauseOnFocusLoss: true,
                            pauseOnHover: true,
                            draggable: true,
                            draggablePercent: 0.6,
                            showCloseButtonOnHover: false,
                            hideProgressBar: true,
                            closeButton: false,
                            icon: true,
                            rtl: false
                        });
                    }
                })
            }
        },
        toSignup() {
            this.$emit('toSignup')
        },
        toForgot() {
            this.$emit('toForgot')
        }
    }
};
</script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
.main-page {
    height: 100vh;
    width: 100%;
    padding: 12px 0;
    text-align: center;
    box-sizing: border-box;
    position: relative;
    background-color: #000;

    .close {
        display:none;
        align-items:center;
        justify-content:flex-start;
        >img {
            height: 24px;
            width: auto;
        }
        >span {
            color: #fff;
            font-size: 14px;
        }
    }

    @media only screen and (max-width: 450px) {
        .close {
            display: flex;
        }
    }
}

.login-info {
    margin-top: 100px;
    background-color: #000;
    position: relative;
    .login-logo {
        position: absolute;
        top: -65px;
        left: 0;
        width: 100%;
        text-align: center;
        .login-logo-box {
            width: 300px;
            padding-top: 10px;
            margin: 0 auto;
            .login-logo-img {
                width: auto;
                height: 50px;
            }
        }
    }

    .welcome {
        color: #a68574;
        font-size: 14px;
        text-align: center;
    }

    .main-form {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        margin-top: 50px;
    }
}

.tag-box {
    height: 48px;
    padding-top: 60px;
    display: flex;
    align-items: flex-end;
    box-sizing: border-box;

    .reg {
        font-family: Arial;
        font-size: 16px;
        color: #fff;
        .border-bottom {
            margin-top: 6px;
            visibility: hidden;
            height: 3px;
            width: 40px;
            border-radius: 4px;
        }
    }


    .active {
        font-size: 24px;
        color: #fff;
        .border-bottom {
            visibility:visible;
            background-color: rgb(249,233,58);
        }
    }
}

.login-info-item {
    text-align: left;
    margin: 0 40px 4px 40px;
    border: 1px solid #616060;
    height: 40px;
    width: 300px !important;
    border-radius: 6px;
    position: relative;
    color: #fff;
    padding-left: 10px;
    background-color: #2c2b2b;

    .v-box {
        display: flex;
        align-items: center;
        >div {
            display: flex;
            align-items: center;
            >img {
                width: 24px;
                height: 30px;
            }
        }
    }
}

@media only screen and (max-width: 425px) {
    .login-info-item {
        width: 200px;
        border-radius: 54px;
    }
}

.eye-img {
    height: 20px;
    width: 20px;
}

.button-box {
    margin-top: 24px;
    width: 400px;
}

.v-label {
    position: absolute;
    left: 20px;
    font-size: 16px;
    top: 17px;
    height: 20px;
    line-height: 20px;
    letter-spacing: normal;
    -webkit-animation: v-shake .6s cubic-bezier(.25, .8, .5, 1);
    animation: v-shake .6s cubic-bezier(.25, .8, .5, 1);
    transform-origin: top left;
}

.v-input {
    width: 100%;
    color: #fff;
    background: transparent;
    padding: 0 16px;
    border: 0;
    font-size: 16px;
    height: 40px;
    line-height: 40px;
    letter-spacing: normal;
}

.v-input::placeholder {
    color: #b6b2b2;
}

.login-info-item input:focus {
    outline: none;
}

.msg-box {
    min-height: 20px;
    overflow: hidden;
    margin: 0 40px;
    position: relative;
}

.login-info-message {
    min-height: 14px;
    padding: 0 12px;
    color: #fff;
    line-height: 12px;
    word-break: break-word;
    word-wrap: break-word;
    -webkit-hyphens: auto;
    -ms-hyphens: auto;
    hyphens: auto;
    font-size: 12px;
    text-align: left;
}

.bar-box {
    margin-top: 6px;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 0 40px;
    box-sizing: border-box;

    .left-box {
        display: flex;
        align-items: center;

        .remember-img {
            height: 20px;
            width: 20px;
            margin-right: 4px;
        }

        .keep-login {
            font-family: Arial;
            font-size: 12px;
            color: #ffb600;
        }
    }

    .forget-passcode {
        font-size: 12px;
        color: #ffae2e;
    }
}

.error-info {
    color: #dd2c00 !important;
    caret-color: #dd2c00 !important;
}

.normal-info {
    color: #1976d2 !important;
    caret-color: #1976d2 !important;
}

.theme-dark {
    color: #fff;
}

.v-icon {
    position: absolute;
    right: 14px;
    top: 0;
    height: 40px;
    display: flex;
    align-items: center;
}

.v-label-active {
    transform: translateY(-10px) scale(.75);
}

@-webkit-keyframes v-shake {
    59% {
        margin-left: 0
    }

    60%,
    80% {
        margin-left: 2px
    }

    70%,
    90% {
        margin-left: -2px
    }
}

@keyframes v-shake {
    59% {
        margin-left: 0
    }

    60%,
    80% {
        margin-left: 2px
    }

    70%,
    90% {
        margin-left: -2px
    }
}

.login-button {
    background: rgb(104, 50, 22);;
    color: #fff !important;
    height: 44px;
    min-width: 66.66666%;
    padding: 0 19.5555555556px;
    font-size: 12px !important;
    letter-spacing: 0 !important;
    border-radius: 6px !important;
    transition: all .2s cubic-bezier(.02, .54, .58, 1);
    border: none;
    font-family: Arial;
    padding: 0 20px;
}

@media only screen and (max-width: 450px) {
    .login-button {
        border-radius: 41px !important;
        background: linear-gradient(180deg,#6892a4,#416375);
    }
}
</style>
  