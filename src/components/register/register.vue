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
            <div class="page-info" v-show="step == 1">
                <div id="name-box" class="login-info-item" @click="nameFocus">
                    <div class="v-box" style="position:relative;">
                        <div><img :src="require('../../assets/login/phone.png')" /></div>
                        <div style="height:40px;line-height:40px; font-size:16px;color:#fff;">+55</div>
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
                        <input class="v-input" ref="passcode" type="password" :placeholder="GLOBAL.lanLocal['passcode']" @focus="passcodeFocus" @blur="passcodeBlur"
                            @input="passcodeInput" v-if="!isEye" v-model="passcode" />
                        <input class="v-input" ref="passcode" type="text" @focus="passcodeFocus" @blur="passcodeBlur"
                            @input="passcodeInput" v-else v-model="passcode" />
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
                <div id="passcode2-box" class="login-info-item" @click="passcodeFocus2">
                    <div class="v-box">
                        <div><img :src="require('../../assets/login/key.png')" /></div>
                        <input class="v-input" ref="passcode2" type="password" :placeholder="GLOBAL.lanLocal['passcode']" @focus="passcodeFocus2" @blur="passcodeBlur2"
                            @input="passcodeInput2" v-if="!isEye2" v-model="passcode2" />
                        <input class="v-input" ref="passcode2" type="text" @focus="passcodeFocus2" @blur="passcodeBlur2"
                            @input="passcodeInput2" v-else v-model="passcode2" />
                    </div>
                    <span class="v-icon mdi" :class="eyeClass2" v-if="!isEye2" @click="changeEye2">
                        <img class="eye-img" :src="require('../../assets/login/pwd-normal.png')" />
                    </span>
                    <span class="v-icon mdi" :class="eyeClass2" v-else @click="changeEye2">
                        <img class="eye-img" :src="require('../../assets/login/pwd-show.png')" />
                    </span>
                </div>
                <div class="msg-box">
                    <div class="login-info-message error-info">{{ passcodeinfo2 }}</div>
                </div>
                <div id="nickname-box" class="login-info-item" @click="nicknameFocus">
                    <div class="v-box">
                        <!-- <label class="v-label" :class="nicknameClass">{{GLOBAL.lanLocal['nickname']}}</label> -->
                        <input class="v-input" ref="nickname" type="text" :placeholder="GLOBAL.lanLocal['name']" @focus="nicknameFocus" @blur="nicknameBlur"
                            @input="nicknameInput" v-model="nickname" />
                    </div>
                </div>
                <div class="msg-box">
                    <div class="login-info-message error-info">{{ nicknameinfo }}</div>
                </div>
                <div id="shareid-box" class="login-info-item" @click="shareidFocus">
                    <div class="v-box">
                        <!-- <label class="v-label" :class="shareidClass">{{GLOBAL.lanLocal['shareid']}}</label> -->
                        <input class="v-input" ref="shareid" type="text" :placeholder="GLOBAL.lanLocal['shareid']" @focus="shareidFocus" @blur="shareidBlur"
                            @input="shareidInput" v-model="shareid" />
                    </div>
                </div>
                <div class="msg-box">
                    <!-- <div class="login-info-message error-info">{{ shareidInfo }}</div> -->
                </div>
                <div class="d-flex register-condition">
                    <div
                        class="v-input-box v-input--is-label-active v-input--is-dirty theme--dark v-input--selection-controls v-input--checkbox primary--text">
                        <div class="v-input__control">
                            <div class="v-input__slot">
                                <div class="v-input--selection-controls__input">
                                    <!-- <i aria-hidden="true" class="v-icon notranslate mdi mdi-checkbox-marked theme--dark"></i> -->
                                    <input v-model="isAgree" style="width: 20px;height: 20px;" role="checkbox" type="checkbox"
                                        value="">
                                    <!-- <div class="v-input--selection-controls__ripple primary--text"></div> -->
                                </div>
                            </div>
                            <div class="v-messages theme--dark primary--text">
                                <div class="v-messages__wrapper"></div>
                            </div>
                        </div>
                    </div>
                    <p class="more-information">
                        {{GLOBAL.lanLocal['agreement']}}
                        <span @click="showProtocal"> {{GLOBAL.lanLocal['readmore']}} </span>
                    <div class="v-dialog__container"></div>
                    </p>
                </div>
                <div class="button-box">
                    <input class="login-button"
                        :class="nicknameempty || nicknameerror || nameempty || nameerror || passcodeempty || passcodeerror || passcodeempty2 || passcodeerror2 || !isAgree ? 'login-button-disable' : ''"
                        type="button" @click="doRegister" :value="GLOBAL.lanLocal['confirm']" />
                </div>
                <div class="tag-box">
                    <div class="login" @click="changeTag()">
                        <div>{{GLOBAL.lanLocal['login-low'] }}</div>
                        <div class="border-bottom"></div>
                    </div>
                </div>
            </div>
            <div class="protocal" :class="protocalClass">
                <div class="v-dialog x-middle v-dialog--active" style="transform-origin: center center; max-width: 600px;">
                    <div class="container">
                        <div>
                            <h3 class="text-center mb-2">{{GLOBAL.lanLocal['registeragree']}}</h3>
                            <p>
                                1. {{GLOBAL.lanLocal['agree11']}}
                                {{GLOBAL.lanLocal['agree12']}}
                                {{GLOBAL.lanLocal['agree13']}}
                            </p>
                            <p>
                                2. {{GLOBAL.lanLocal['agree21']}}
                                {{GLOBAL.lanLocal['agree22']}}
                            </p>
                            <p>3. {{GLOBAL.lanLocal['agree31']}}</p>
                            <p>
                                4. {{GLOBAL.lanLocal['agree41']}}
                                {{GLOBAL.lanLocal['agree42']}}
                            </p>
                            <p>
                                5. {{GLOBAL.lanLocal['agree51']}}
                                {{GLOBAL.lanLocal['agree52']}}
                            </p>
                        </div>
                        <div class="text-center mb-2">
                            <input class="login-button" type="button" @click="dismissProtocal" :value="GLOBAL.lanLocal['agree']" />
                        </div>
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
    name: 'register',
    data() {
        return {
            isEye: false,
            isEye2: false,
            loginname: '',
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
            hasShareid: true,
            shareiderror: false,
            shareidfocus: false,
            shareidempty: true,
            shareidInfo: '',
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
        let shareid = localStorage.getItem(md5("__shareid___"))
        if (shareid) {
            this.shareid = shareid
            this.hasShareid = true
            this.shareidInput()
        } else {
            doPost({
                n: 'AppShare',
                a: 'get_share_id',
                udid: this.GLOBAL.deviceid
            }).then((res) => {
                if(res.code === 0 && res.data)
                {
                    this.shareid = res.data
                    this.hasShareid = true
                    this.shareidInput()
                }else {
                    // this.shareid = 1195575
                    // this.hasShareid = true
                    this.shareidInput()
                }
            })
        }
    },
    methods: {
        changeTag() {
            this.$emit("signin")
        },
        close() {
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
                // this.shareiderror = true
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
        sendCode() {
            if (this.checkName(true)) {
                let that = this
                clearInterval(that.countTimer)
                doPost({
                    n: 'AppEx',
                    a: 'send_code',
                    mobile: this.loginname,
                    type: 1
                }).then((res) => {
                    if (res.code === 0) {
                        this.sendcount = 60
                        this.countTimer = setInterval(() => {
                            that.sendcount -= 1
                            if (that.sendcount < 0) {
                                that.sendcount = 0
                                clearInterval(that.countTimer)
                            }
                        }, 1000)
                        this.$toast.info(this.GLOBAL.lanLocal['sendsuccess'], {
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
        toStep(step) {
            if (step == 2) {
                if (this.isAgree && this.checkName(true) && this.checkCode(true)) {
                    doPost({
                        n: 'AppEx',
                        a: 'validate_phone_code',
                        mobile: this.loginname,
                        code: this.code
                    }).then((res) => {
                        if (res.code === 0) {
                            this.step = step
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
            } else {
                this.step = step
            }
        },
        doRegister() {
            if (this.checkNickname(true) && this.checkName(true) && this.checkPasscode(true) && this.checkPasscode2(true) && this.isAgree) {
                const passcode = md5(this.passcode)
                let loginname = this.loginname
                doPost({
                    n: 'AppEx',
                    a: 'simple_phone_register',
                    username: loginname,
                    mobile: loginname,
                    password: passcode,
                    rolename: this.nickname,
                    deviceid: this.GLOBAL.deviceid,
                    shareid: this.shareid
                }).then((res) => {
                    if (res.code === 0) {
                        const json = loadFile("static/nonce", true);
                        if (json != null) {
                            let api_aes_key = json['api_aes_key']
                            localStorage.setItem(md5('__name__'), AES.encrypt(loginname, api_aes_key))
                            localStorage.setItem(md5('__pwd_'), AES.encrypt(passcode, api_aes_key))

                            this.GLOBAL.userInfo.name = loginname
                            this.GLOBAL.userInfo.passcode = passcode

                            localStorage.setItem('reg', 1);
                            this.$emit('success');
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
        toSinin() {
            this.$emit('signin')
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
            var numReg = /^[0-9]*$/
            var numRe = new RegExp(numReg)
            let loginname = this.loginname.trim()
            if (loginname.length <= 0) {
                if (needResult) {
                    return false
                }
                this.nameempty = true
                this.nameerror = true
                this.nameinfo = this.GLOBAL.lanLocal['plzinput']
            }else if(!numRe.test(loginname) || loginname.length < 10 || loginname.length > 11) {
                if (needResult) {
                    return false
                }
                this.nameempty = true
                this.nameerror = true
                this.nameinfo = this.GLOBAL.lanLocal['mobilelengtherror']
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

    .page-info {
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
    margin: 0 20px 2.2rem 40px;
    display: flex;
    align-items: flex-end;
    box-sizing: border-box;

    .login,
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
        color: rgb(249,233,58);
        .border-bottom {
            visibility:visible;
            background-color: rgb(249,233,58);
        }
    }
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
            background: #ffb600;
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
    right: 14px;
    top: 0;
    height: 40px;
    display: flex;
    align-items: center;
}



.eye-img {
    height: 20px;
    width: 20px;
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
    align-items: center;

    .container {
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
    display: flex !important;
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

.login-button {
    background: brown;
    color: #fff !important;
    height: 44px;
    min-width: 66.66666%;
    padding: 0 19.5555555556px;
    font-size: 18px !important;
    letter-spacing: 0 !important;
    border-radius: 6px !important;
    transition: all .2s cubic-bezier(.02, .54, .58, 1);
    border: none;
    font-family: Arial;
    padding: 0 20px;
}

@media only screen and (max-width: 450px) {
    .login-button {
        width: 300px;
        border-radius: 41px !important;
        background: linear-gradient(180deg,#6892a4,#416375);
    }
}

.login-button-disable {
    opacity: .5 !important;
}
.input-disabled {
    color: #dcdcdc;
}

</style>
  