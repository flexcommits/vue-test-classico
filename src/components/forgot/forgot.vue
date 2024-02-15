<template>
    <div class="main-page">
        <div class="login-logo">
            <img class="login-logo-img" :src="require('../../assets/login/logo.png')" />
        </div>
        <div class="page-info" v-show="step == 1">
            <div id="name-box" class="login-info-item" @click="nameFocus">
                <div class="v-box">
                    <label class="v-label" :class="loginClass">{{GLOBAL.lanLocal['oldpassword']}}</label>
                    <input class="v-input" ref="oldpwd" type="text" @focus="nameFocus" @blur="nameBlur"
                        @input="nameInput" v-model="oldpwd" />
                </div>
            </div>
            <div class="msg-box">
                <div class="login-info-message error-info">{{ nameinfo }}</div>
            </div>
            <div id="passcode-box" class="login-info-item" @click="passcodeFocus">
                <div class="v-box">
                    <label class="v-label" :class="passcodeClass">{{GLOBAL.lanLocal['newpassword']}}</label>
                    <input class="v-input" ref="passcode" type="password" @focus="passcodeFocus" @blur="passcodeBlur"
                        @input="passcodeInput" v-if="!isEye" v-model="passcode" />
                    <input class="v-input" ref="passcode" type="text" @focus="passcodeFocus" @blur="passcodeBlur"
                        @input="passcodeInput" v-else v-model="passcode" />
                </div>
                <span class="v-icon mdi mdi-eye-off theme-dark" :class="eyeClass" v-if="!isEye" @click="changeEye"></span>
                <span class="v-icon mdi mdi-eye theme-dark" :class="eyeClass" v-else @click="changeEye"></span>
            </div>
            <div class="msg-box">
                <div class="login-info-message error-info">{{ passcodeinfo }}</div>
            </div>
            <div id="passcode2-box" class="login-info-item" @click="passcodeFocus2">
                <div class="v-box">
                    <label class="v-label" :class="passcodeClass2">{{GLOBAL.lanLocal['newpassword']}}</label>
                    <input class="v-input" ref="passcode2" type="password" @focus="passcodeFocus2" @blur="passcodeBlur2"
                        @input="passcodeInput2" v-if="!isEye2" v-model="passcode2" />
                    <input class="v-input" ref="passcode2" type="text" @focus="passcodeFocus2" @blur="passcodeBlur2"
                        @input="passcodeInput2" v-else v-model="passcode2" />
                </div>
                <span class="v-icon mdi mdi-eye-off theme-dark" :class="eyeClass2" v-if="!isEye2"
                    @click="changeEye2"></span>
                <span class="v-icon mdi mdi-eye theme-dark" :class="eyeClass2" v-else @click="changeEye2"></span>
            </div>
            <div class="msg-box">
                <div class="login-info-message error-info">{{ passcodeinfo2 }}</div>
            </div>
            <div class="button-box"  @click="doReset"
                    :class="nameempty || nameerror || passcodeempty || passcodeerror || passcodeempty2 || passcodeerror2 ? 'button-box-disable' : ''">
                {{GLOBAL.lanLocal['confirm']}}
            </div>
        </div>
    </div>
</template>
  
<script>
import md5 from 'js-md5';
import { doPost } from '../../api/api.js'
import decodeApiLan from '../../api/decodeApiLan.js'
export default {
    name: 'forgot',
    data() {
        return {
            isEye: false,
            isEye2: false,
            oldpwd: '',
            passcode: '',
            passcode2: '',
            nickname: '',
            code: '',
            nameerror: false,
            namefocus: false,
            nameempty: true,
            nameinfo: '',
            nicknameerror: false,
            nicknamefocus: false,
            nicknameempty: true,
            nicknameinfo: '',
            codeerror: false,
            codefocus: false,
            codeempty: true,
            codeinfo: '',
            isProtocal: false,
            isAgree: false,
            step: 1,
            passcodeerror: false,
            passcodefocus: false,
            passcodeempty: true,
            passcodeinfo: '',
            passcodeerror2: false,
            passcodefocus2: false,
            passcodeempty2: true,
            passcodeinfo2: '',
            sendcount: 0,
            countTimer: '',
            shareid: '',
            shareiderror: false,
            shareidfocus: false,
            shareidempty: true,
            shareidInfo: '',
            innerHeight: 0,
        }
    },
    computed: {
        shareidClass() {
            return {
                'error-info': this.shareiderror,
                'normal-info': !this.shareiderror && this.shareidfocus,
                'v-label-active': this.shareidfocus || !this.shareidempty
            }
        },
        nicknameClass() {
            return {
                'error-info': this.nicknameerror,
                'normal-info': !this.nicknameerror && this.nicknamefocus,
                'v-label-active': this.nicknamefocus || !this.nicknameempty
            }
        },
        loginClass() {
            return {
                'error-info': this.nameerror,
                'normal-info': !this.nameerror && this.namefocus,
                'v-label-active': this.namefocus || !this.nameempty
            }
        },
        codeClass() {
            return {
                'error-info': this.codeerror,
                'normal-info': !this.codeerror && this.codefocus,
                'v-label-active': this.codefocus || !this.codeempty
            }
        },
        protocalClass() {
            return {
                'protocal-show': this.isProtocal
            }
        },
        passcodeClass() {
            return {
                'error-info': this.passcodeerror,
                'normal-info': !this.passcodeerror && this.passcodefocus,
                'v-label-active': this.passcodefocus || !this.passcodeempty
            }
        },
        passcodeClass2() {
            return {
                'error-info': this.passcodeerror2,
                'normal-info': !this.passcodeerror2 && this.passcodefocus2,
                'v-label-active': this.passcodefocus2 || !this.passcodeempty2
            }
        },
        eyeClass() {
            return {
                'error-info': this.passcodeerror,
                'normal-info': !this.passcodeerror && this.passcodefocus
            }
        },
        eyeClass2() {
            return {
                'error-info': this.passcodeerror2,
                'normal-info': !this.passcodeerror2 && this.passcodefocus2
            }
        },
    },
    created() {
        this.innerHeight = window.innerHeight - 60
    },
    methods: {
        close () {
            this.$emit('close')
        },
        shareidFocus() {
            this.$refs.shareid.focus()
            this.shareidfocus = true
        },
        shareidBlur() {
            this.shareidfocus = false
            this.checkShareid()
        },
        shareidInput() {
            this.checkShareid()
        },
        checkShareid(needResult = false) {
            if (this.shareid.length <= 0) {
                if (needResult) {
                    return false
                }
                this.shareidempty = true
                this.shareiderror = true
                this.shareidInfo = this.GLOBAL.lanLocal['plzinput']
            } else {
                if (needResult) {
                    return true
                }
                this.shareidempty = false
                this.shareiderror = false
                this.shareidInfo = ''
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
            if (this.passcode2) {
                this.checkPasscode2()
            }
        },
        passcodeInput() {
            this.checkPasscode()
            if (this.passcode2) {
                this.checkPasscode2()
            }
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
        passcodeFocus2() {
            this.$refs.passcode2.focus()
            this.passcodefocus2 = true
            // this.checkPasscode()
        },
        passcodeBlur2() {
            this.passcodefocus2 = false
            this.checkPasscode2()
        },
        passcodeInput2() {
            this.checkPasscode2()
        },
        checkPasscode2(needResult = false) {
            if (this.passcode != this.passcode2) {
                if (needResult) {
                    return false
                }
                this.passcodeempty2 = false
                this.passcodeerror2 = true
                this.passcodeinfo2 = this.GLOBAL.lanLocal['samepasscode']
            } else {
                if (needResult) {
                    return true
                }
                this.passcodeempty2 = false
                this.passcodeerror2 = false
                this.passcodeinfo2 = ''
            }
        },
        doReset() {
            if (this.checkName(true) && this.checkPasscode(true) && this.checkPasscode2(true)) {
                const password = md5(this.oldpwd)
                const newPassword = md5(this.passcode)
                doPost({
                    n: 'AppEx',
                    a: 'reset_login_password',
                    mobile: this.GLOBAL.userInfo.name,
                    password: password,
                    newPassword: newPassword,
                }).then((res) => {
                    if (res.code === 0) {
                        const data = decodeApiLan(res, this.GLOBAL.lanArr);
                        this.$toast.success(data.message, {
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
                        this.GLOBAL.userInfo.accountid = 0
                        this.GLOBAL.userInfo.name = ''
                        this.GLOBAL.userInfo.passcode = ''
                        localStorage.removeItem(md5('__name__'))
                        localStorage.removeItem(md5('__pwd_'))
                        this.$emit('signin');
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
        toSinin() {
            this.$emit('toSignin')
        },
        showProtocal() {
            this.isProtocal = true;
        },
        dismissProtocal() {
            this.isProtocal = false;
        },
        changeEye() {
            this.isEye = !this.isEye;
        },
        changeEye2() {
            this.isEye2 = !this.isEye2;
        },
        nameFocus() {
            this.$refs.oldpwd.focus()
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
            if (this.oldpwd.length <= 0) {
                if (needResult) {
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
        nicknameFocus() {
            this.$refs.nickname.focus()
            this.nicknamefocus = true
            // this.checkName()
        },
        nicknameBlur() {
            this.nicknamefocus = false
            this.checkNickname()
        },
        nicknameInput() {
            this.checkNickname()
        },
        checkNickname(needResult = false) {
            if (this.nickname.length <= 0) {
                if (needResult) {
                    return false
                }
                this.nicknameempty = true
                this.nicknameerror = true
                this.nicknameinfo = this.GLOBAL.lanLocal['plzinput']
            } else {
                if (needResult) {
                    return true
                }
                this.nicknameempty = false
                this.nicknameerror = false
                this.nicknameinfo = ''
            }
        },
        codeFocus() {
            this.$refs.code.focus()
            this.codefocus = true
        },
        codeBlur() {
            this.codefocus = false
            this.checkCode()
        },
        codeInput() {
            this.checkCode()
        },
        checkCode(needResult = false) {
            if (this.code.length <= 0) {
                if (needResult) {
                    return false
                }
                this.codeempty = true
                this.codeerror = true
                this.codeinfo = this.GLOBAL.lanLocal['plzinput']
            } else if (this.code.length < 6) {
                if (needResult) {
                    return false
                }
                this.codeempty = false
                this.codeerror = true
                this.codeinfo = this.GLOBAL.lanLocal['sixcode']
            } else {
                if (needResult) {
                    return true
                }
                this.codeempty = false
                this.codeerror = false
                this.codeinfo = ''
            }
        },
    }
}
</script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
.main-page {
    height: 100%;
    width: 100%;
    padding: 10px 6px 80px;
    box-sizing: border-box;
    background-color: #20242f;
    text-align: center;
}


.page-title {
    position: relative;
    font-weight: 500;
    font-size: 18px;

    .close {
        color: #fff;
        position: absolute;
        top: 0;
        right: 0;

        .v-icon {
            top: 0 !important;
            right: 0 !important;
            align-items: center;
            display: inline-flex;
            font-feature-settings: "liga";
            font-size: 24px;
            justify-content: center;
            letter-spacing: normal;
            line-height: 1;
            position: relative;
            text-indent: 0;
            transition: .3s cubic-bezier(.25, .8, .5, 1), visibility 0s;
            vertical-align: middle;
            -webkit-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none;
            user-select: none;
        }
    }
}

.login-logo {
    padding-top: 2.2rem;
    margin-bottom: 1.8rem;
    text-align: center;
}

.login-logo-img {
    margin-top: 10px;
    width: 120px;
    height: auto;
}

.login-info-item {
    text-align: left;
    margin: 0 32px 4px 32px;
    background: hsla(0, 0%, 100%, .08);
    height: 52px;
    border-radius: 10px;
    position: relative;
    color: hsla(0, 0%, 100%, .7);
    border: 1px solid hsla(0,0%,100%,.2);

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
    width: auto;
    color: #fff;
    background: transparent;
    margin-top: 22px;
    padding: 0 20px;
    border: 0;
    font-size: 16px;
    height: 20px;
    line-height: 20px;
    letter-spacing: normal;
}

.d-flex {
    display: flex !important;
    margin: 0 32px;
    padding-bottom: 12px;

    .v-input-box {
        align-items: flex-start;
        display: flex;
        font-size: 16px;
        letter-spacing: normal;
        max-width: 100%;
        text-align: left;
        flex: 0 1 auto;
        margin-top: 5px;
        padding-top: 5px;

        .v-input__control {
            display: flex;
            flex-direction: column;
            height: auto;
            flex-grow: 1;
            flex-wrap: wrap;
            min-width: 0;
            width: 100%;

            .v-input__slot {
                align-items: center;
                color: inherit;
                display: flex;
                margin-bottom: 8px;
                min-height: inherit;
                position: relative;
                transition: .3s cubic-bezier(.25, .8, .5, 1);
                width: 100%;
                cursor: pointer;

                .v-input--selection-controls__input {
                    color: inherit;
                    display: inline-flex;
                    flex: 0 0 auto;
                    height: 24px;
                    position: relative;
                    transition: .3s cubic-bezier(.25, .8, .5, 1);
                    transition-property: transform;
                    width: 24px;
                    -webkit-user-select: none;
                    -moz-user-select: none;
                    -ms-user-select: none;
                    user-select: none;
                    margin-right: 8px;

                    .v-icon {
                        align-items: center;
                        display: inline-flex;
                        font-feature-settings: "liga";
                        font-size: 24px;
                        justify-content: center;
                        letter-spacing: normal;
                        line-height: 1;
                        position: relative;
                        text-indent: 0;
                        transition: .3s cubic-bezier(.25, .8, .5, 1), visibility 0s;
                        vertical-align: middle;
                        -webkit-user-select: none;
                        -moz-user-select: none;
                        -ms-user-select: none;
                        user-select: none;
                        width: 100%;
                    }

                    .primary--text {
                        color: #1976d2 !important;
                        caret-color: #1976d2 !important;
                    }

                    .mdi:before {
                        content: "\F0131";
                        display: inline-block;
                        font: normal normal normal 24px/1 Material Design Icons;
                        font-size: inherit;
                        text-rendering: auto;
                        line-height: inherit;
                        -webkit-font-smoothing: antialiased;
                        -moz-osx-font-smoothing: grayscale;
                    }

                    .v-icon.v-icon:after {
                        background-color: currentColor;
                        border-radius: 50%;
                        content: "";
                        display: inline-block;
                        height: 100%;
                        left: 0;
                        opacity: 0;
                        pointer-events: none;
                        position: absolute;
                        top: 0;
                        transform: scale(1.3);
                        width: 100%;
                        transition: opacity .2s cubic-bezier(.4, 0, .6, 1);
                    }

                    .v-input--selection-controls__ripple {
                        border-radius: 50%;
                        cursor: pointer;
                        height: 34px;
                        position: absolute;
                        transition: inherit;
                        width: 34px;
                        left: -12px;
                        top: calc(50% - 24px);
                        margin: 7px;
                    }

                    .v-input--selection-controls__ripple:before {
                        border-radius: inherit;
                        bottom: 0;
                        content: "";
                        position: absolute;
                        opacity: .2;
                        left: 0;
                        right: 0;
                        top: 0;
                        transform-origin: center center;
                        transform: scale(.2);
                        transition: inherit;
                    }
                }
            }
        }
    }

    .more-information {
        color: #fff;

        >span {
            background: linear-gradient(180deg, #ffdf46, #ffdc42 23.96%, #feeb52 49.48%, #fbc316 71.87%, #f69b09);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            font-weight: 600;
            cursor: pointer;
        }
    }
}

.login-info-item input:focus {
    outline: none;
}

.msg-box {
    min-height: 20px;
    overflow: hidden;
    margin: 0 32px;
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

.forget-passcode {
    word-break: break-word;
    word-wrap: break-word;
    -webkit-hyphens: auto;
    -ms-hyphens: auto;
    hyphens: auto;
    font-size: 14px;
    position: absolute;
    right: 1.5rem;
    bottom: 0;
    color: #fff;
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
    right: 20px;
    top: 15px;
    font-size: 16px;
}

.v-label-active {
    transform: translateY(-10px) scale(.75);
}

.protocal {
    position: fixed;
    top: 0;
    left: 0;
    display: none;
    background-color: #000 !important;
    color: white;
    font-family: Noto Sans Thai, Roboto !important;
    font-size: 14px !important;
    text-align: left;
    height: 100%;

    .container {
        margin-top: 50%;
        width: 100%;
        padding: 12px;
        margin-right: auto;
        margin-left: auto;
        box-sizing: border-box;

        .text-center {
            text-align: center !important;
        }
    }
}

.protocal-show {
    display: block !important;
    ;
}

.row {
    display: flex;
    flex-wrap: wrap;
    flex: 1 1 auto;
    margin: -12px;
    margin-top: 12px;

    .dailog-notuser-register {
        color: #fff !important;
        text-align: center;
        margin-bottom: 1rem;

        >a {
            font-size: 17px;
            color: #fff !important;
            text-decoration: underline;
        }
    }

    .col-12 {
        width: 100%;
        padding: 0;
        padding-top: 0 !important;
        padding-bottom: 0 !important;
        justify-content: center;
        flex: 0 0 100%;
        max-width: 100%;
    }

    .dailog-call-center {
        display: flex;
        color: #fff !important;
        text-align: center;
        margin-bottom: 1rem;

        >a {
            font-size: 14px;
            text-decoration: none;
            display: flex;
            cursor: pointer;

            >div {
                margin: 0 10px;
                color: #ffd600;
            }

            >img {
                width: 25px;
                height: 25px;
            }
        }
    }
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

.button-box {
    background-color: #f9e93a;
    height: 50px;
    line-height: 50px;
    font-size: 18px;
    color: #000;
    text-align: center;
    border-radius: 10px;
    border: none;
    width: 70%;
    margin: 30px auto 0;
}

.button-box-disable {
    opacity: .5 !important;
}</style>
  