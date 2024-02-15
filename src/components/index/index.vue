<template>
    <div style="position:  relative;">
        <div class="background"></div>
        <NavHeader  @changeSlider="changeSlider"
            @setLan="changeLanCode" @toLogin="showPage(5)" @toReg="showPage(51)" @toRecharge="showPage(6)" :accountid="GLOBAL.userInfo.accountid"
            :amount="amount" :lanData="lanData" @email="showPage(3)">
        </NavHeader>
        <div class="loading-masking" v-if="loading">
            <div class="loading-page">
                <vue-simple-spinner size="large" line-fg-color="#f69b09"></vue-simple-spinner>
            </div>
        </div>
        <div class="container">
            <div class="slider-menu-masking" @click="changeSlider">
                <div class="slider-menu" :style="{width: isShowSlider ? '250px' : '90px'}"  @click.stop="func1()">
                    <div class="menu-list">
                        <div class="menu-item" @click="showPage(1)">
                            <img class="icon" :src="require('../../assets/slider/home.png')" />
                            <div v-if="isShowSlider">{{ GLOBAL.lanLocal['lobby'] }}</div>
                        </div>
                        <div class="menu-item menu-top" @click="showPage(2)">
                            <img class="icon" :src="require('../../assets/slider/activity.png')" />
                            <div v-if="isShowSlider">{{ GLOBAL.lanLocal['activity'] }}</div>
                        </div>
                        <div class="menu-item menu-top" @click="showPage(10)">
                            <img class="icon" :src="require('../../assets/slider/bonus.png')" />
                            <div v-if="isShowSlider">{{ GLOBAL.lanLocal['bonus'] }}</div>
                        </div>
                        <div class="menu-item menu-top" @click="showPage(6)">
                            <img class="icon" :src="require('../../assets/slider/recharge.png')" />
                            <div v-if="isShowSlider">{{ GLOBAL.lanLocal['recharge'] }}</div>
                        </div>
                        <div class="menu-item menu-top" @click="showPage(4)">
                            <img class="icon" :src="require('../../assets/slider/me.png')" />
                            <div v-if="isShowSlider">{{ GLOBAL.lanLocal['me'] }}</div>
                        </div>
                    </div>
                    <div class="menu-item" @click="saveDesktop">
                        <img class="icon" :src="require('../../assets/slider/save.png')" />
                        <div v-if="isShowSlider">{{ GLOBAL.lanLocal['savetodesktop'] }}</div>
                    </div>
                    <div class="menu-item menu-top" @click="showPage(3)">
                        <img class="icon" :src="require('../../assets/slider/email.png')" />
                        <div v-if="isShowSlider">{{ GLOBAL.lanLocal['email'] }}</div>
                    </div>
                    <a class="menu-item menu-top" v-if="customer != ''" :href="customer" target="_blank">
                        <img class="icon" v-if="customerType == 1" :src="require('../../assets/line.png')" />
                        <img class="icon" v-if="customerType == 2" :src="require('../../assets/fb.png')" />
                        <img class="icon" v-if="customerType == 3" :src="require('../../assets/slider/whatsapp.png')" />
                        <img class="icon" v-if="customerType == 4" :src="require('../../assets/twitter.png')" />
                        <img class="icon" v-if="customerType == 5" :src="require('../../assets/slider/telegram.png')" />
                        <div v-if="isShowSlider">{{ GLOBAL.lanLocal['contact'] }}</div>
                    </a>
                    <div class="menu-item menu-top" @click="changeLan">
                        <template v-for="(item, index) in lanData">
                            <img v-if="lanCode == item.code" class="icon" :src="item.img" />
                            <div v-if="lanCode == item.code" class="lan-desc">
                                <div v-if="isShowSlider">{{ item.title }}</div>
                                <img v-if="isShowSlider" class="down" :src="require('../../assets/icon/down.png')" />
                            </div>
                        </template>
                        <div class="lan-box" v-if="isShowLan">
                            <div class="lan-item" v-for="(item, index) in lanData" @click="changeLanCode(item.code)">
                                <img class="icon" :src="item.img" />
                                <div v-if="isShowSlider">{{ item.title }}</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="main-wrap" :style="{width: isShowSlider ? 'calc(100% - 250px)' : 'calc(100% - 90px)', marginLeft: isShowSlider ? '250px' : '90px'}">
                <HomePage v-if="tabIndex == 1" @toGame="toGame" @signin="signin" @reg="reg" @recharge="showPage(6)" @vip="showPage(11)"
                @toForgot="showPage(7)"></HomePage>
                <ActivityPage v-if="tabIndex == 2" @detail="activityDetail"></ActivityPage>
                <ActivityDetailPage v-if="tabIndex == 21" :id="id" @close="closeModal"></ActivityDetailPage>
                <EmailPage v-if="tabIndex == 3" @detail="emailDetail"></EmailPage>
                <EmailDetailPage v-if="tabIndex == 31" :id="id" @close="closeModal"></EmailDetailPage>
                <MyPage v-if="tabIndex == 4" @reload="reload" @logout="logout" @email="showPage(3)" @forgot="showPage(7)" :amount="amount" @vip="showPage(11)" @gameRecord="showPage(12)"></MyPage>
                <LoginPage v-if="tabIndex == 5" :isReg="isReg" @close="closeModal" @reload="reload" @reg="reg" @toForgot="showPage(7)"></LoginPage>
                <RegisterPage v-if="tabIndex == 51" @signin="signin" @close="closeModal" @success="reload"></RegisterPage>
                <DepositPage v-if="tabIndex == 6" @close="closeModal" @toBind="toBind" @toService="showPage(8)" @reload="reload">
                </DepositPage>
                <ForgotPage v-if="tabIndex == 7" @close="closeModal" @signin="signin"></ForgotPage>
                <ServicePage v-if="tabIndex == 8" @close="closeModal"></ServicePage>
                <BindPage v-if="tabIndex == 9" @close="closeModal"></BindPage>
                <SharePage v-if="tabIndex == 10" @close="closeModal" @reload="reload"></SharePage>
                <VipPage v-if="tabIndex == 11" @close="closeModal" @toDeposit="toDeposit"></VipPage>
                <GameRecordPage v-if="tabIndex == 12" @close="closeModal"></GameRecordPage>
                <div class="page-masking" v-if="activeList.length > 0 && ((activetype == 1 && activeList[0]['active'] != ''))">
                    <div class="active-content">
                        <img class="img" v-if="activetype == 1" :src="activeList[0]['active']" />
                        <div class="topage" v-if="activetype == 1" @click="reg">{{GLOBAL.lanLocal['registernow']}}</div>
                        <img class="close" @click="closeActive" src="@/assets/active-close.png" />
                    </div>
                </div>
                <div class="page-masking" v-if="activeList.length > 0 && ((activetype == 2 && activeList[1]['active'] != ''))">
                    <div class="active-content">
                        <img class="img" v-if="activetype == 2" :src="activeList[1]['active']" />
                        <div class="topage" v-if="activetype == 2" @click="toDeposit">{{GLOBAL.lanLocal['rechargenow']}}</div>
                        <img class="close" @click="closeActive" src="@/assets/active-close.png" />
                    </div>
                </div>
                <Sign v-if="signCover > 0" @reload="reload" @close="changeSign"></Sign>
                <Turntable @reload="reload" @changeTurntable="changeTurntable" :type="turntableType"></Turntable>
                <div class="frameBack" v-show="url">
                    <img @click="changeCloseMasking" src="@/assets/index/back.png" />
                </div>
                <div class="close-masking" v-if="closeMasking">
                    <div class="close-box">
                        <div class="close-title">{{GLOBAL.lanLocal['hint']}}</div>
                        <div class="close-content">{{GLOBAL.lanLocal['closegamehint']}}</div>
                        <div class="close-btn">
                            <div class="cancel" @click="changeCloseMasking">{{GLOBAL.lanLocal['cancel']}}</div>
                            <div class="confirm" @click="closeFrame">{{GLOBAL.lanLocal['ok']}}</div>
                        </div>
                    </div>
                </div>
                <div class="save-masking" v-if="saveMasking">
                    <div class="save-box">
                        <img @click="changeSaveMasking" class="close-icon" src="@/assets/slider/close.png" />
                        <div class="save-title" v-if="saveType == 1">{{GLOBAL.lanLocal['savetitle']}}</div>
                        <div class="save-title" v-else>{{GLOBAL.lanLocal['savetitle']}}</div>
                        <div class="save-content" v-if="saveType == 1">
                            <div class="tips">{{GLOBAL.lanLocal['savetipsandroid']}}</div>
                            <div class="save-guide android">
                                <img class="guide-bg" src="@/assets/slider/guide-android.png" />
                                <div>{{webUrl.toLowerCase()}}</div>
                            </div>
                            <img class="tutorial" src="@/assets/slider/tutorial-android.png" />
                        </div>
                        <div class="save-content" v-else>
                            <div class="tips">{{GLOBAL.lanLocal['savetipsiphone']}}</div>
                            <div class="save-guide">
                                <img class="guide-bg" src="@/assets/slider/guide.png" />
                                <div>{{'https://www.'+webUrl.toLowerCase()}}</div>
                            </div>
                            <img class="tutorial" src="@/assets/slider/tutorial.png" />
                        </div>
                        <div class="save-desc">{{GLOBAL.lanLocal['savedesc']}}</div>
                    </div>
                </div>
                <iframe id="gameFrame" v-if="url" :src="url" class="gameFrame" scrolling="no"
                    sandbox="allow-scripts allow-top-navigation allow-same-origin">
                </iframe>
            </div>
        </div>
        <Tabbar :accountid="GLOBAL.userInfo.accountid" v-if="tabIndex != 5 && tabIndex != 51" :index="tabIndex" :emailCount="emailCount" @change="showPage">
        </Tabbar>
    </div>
</template>
<script>
import loadFile from "../../api/loadFile.js"
import md5 from 'js-md5';
import HomePage from '../home/home.vue'
import ActivityPage from '../activity/activity.vue'
import ActivityDetailPage from '../activity/activityDetail.vue'
import LoginPage from '../login/login.vue'
import RegisterPage from '../register/register.vue'
import MyPage from '../my/my.vue'
import DepositPage from '../deposit/deposit.vue'
import EmailPage from '../email/email.vue'
import EmailDetailPage from '../email/emailDetail.vue'
import ForgotPage from '../forgot/forgot.vue'
import ServicePage from '../service/service.vue'
import BindPage from '../bind/bind.vue'
import SharePage from '../share/share.vue'
import VipPage from '../vip/vip.vue'
import NavHeader from '../navheader/navheader.vue'
import Tabbar from '../tabbar/tabbar.vue'
import AES from "../../common/AES.js"
import { doPost } from '../../api/api.js'
import { v4 as uuidv4 } from 'uuid'
import Turntable from '../turntable/turntable.vue'
import Sign from '../sign/sign.vue'
import GameRecordPage from '../gameRecord/gameRecord.vue'
import { watch } from "vue";
export default {
    name: 'index',
    components: { NavHeader, HomePage, Tabbar, LoginPage, MyPage, ActivityPage, DepositPage, EmailPage, ActivityDetailPage, EmailDetailPage
        , RegisterPage, ForgotPage, ServicePage, BindPage, SharePage, VipPage, Turntable, Sign, GameRecordPage },
    data() {
        return {
            id: 0,
            url: '',
            lanCode: '',
            lanData: [
                { 'code': 'por', 'img': require('../../assets/icon/por.png'), 'title': 'Portugal' },
                { 'code': 'en', 'img': require('../../assets/icon/en.png'), 'title': 'English' },
            ],
            tabIndex: 1,
            isShowSlider: true,
            activityList: [],
            loading: false,
            isShowLan: false,
            amount: '0.00',
            loginType: 1,
            innerHeight: 0,
            emailTimer: null,
            emailCount: 0,
            activeList: [],
            activetype: 0,
            customer: '',
            customerType: 0,
            isReg: false,
            turntableType: 0,
            signCover: 0,
            closeMasking: false,
            saveMasking: false,
            saveType: 0,
            webUrl: '',
        }
    },
    created() {
        window.addEventListener('resize', this.handleResize);
        this.handleResize();
        this.innerHeight = window.innerHeight - 130
        if(window.innerWidth < 768) this.isShowSlider=false
        let lanCode = localStorage.getItem(md5('__lanCode__'))
        if (lanCode) {
         this.GLOBAL.lanCode = lanCode
        }
        this.lanCode = this.GLOBAL.lanCode
        const lanLocal = loadFile("static/json/" + this.GLOBAL.lanCode + ".json", false);
        this.GLOBAL.lanLocal = lanLocal

        let config = loadFile("static/config.json", false)
        this.webUrl = config.weburl

        const json = loadFile("static/nonce", true);
        if (json != null) {
            this.GLOBAL.apiAesKey = json['api_aes_key'];
            this.GLOBAL.apiUrl = json['ip'];
        }
        let deviceid = localStorage.getItem(md5('__deviceid__'))
        if (deviceid) {
            deviceid = AES.decrypt(deviceid, this.GLOBAL.apiAesKey)
        } else {
            deviceid = uuidv4()
            localStorage.setItem(md5('__deviceid__'), AES.encrypt(deviceid, this.GLOBAL.apiAesKey))
        }
        this.GLOBAL.deviceid = deviceid
        doPost({
            n: 'AppEx',
            a: 'get_lan',
            lan: this.GLOBAL.lanCode
        }).then((res) => {
            if (res.code === 0) {
                this.GLOBAL.lanArr = JSON.parse(res.data[this.GLOBAL.lanCode])
            }
        })
        doPost({
            n: 'AppEx',
            a: 'get_active_config',
        }).then((res) => {
            if (res.code === 0) {
                this.activeList = res.data
                this.activeList.forEach(item => {
                    if(item.active != "")
                    {
                        let suffix = item.active.slice(-4)
                        item.active = item.active.replace(suffix, "_"+this.GLOBAL.lanCode+suffix)
                    }
                });
            }
        })
        this.getCusomter()
        this.reload()
    },
    computed: {

    },
    mounted() {
        window.parent.postMessage(
            {
                cmd: 'jump',
                params: {
                    val: 1
                }
            }, '*'
        )
        window.addEventListener('message', event => {
            let data = event.data
            if (data.cmd === 'jump') {
                localStorage.setItem("only_data", 1)
                this.reload()
            }
        })
    },
    destroyed() {
        window.removeEventListener('resize', this.handleResize);
    },
    methods: {
        handleResize() {
            if(window.innerWidth > 768) this.isShowSlider=true;
            else this.isShowSlider=false;
        },
        changeSaveMasking() {
            this.saveMasking = !this.saveMasking
        },
        saveDesktop() {
            this.changeSlider()
            const ua = navigator.userAgent.toLowerCase()
            if(ua.indexOf('android') != -1)
            {
                this.saveType = 1
            }else if(ua.indexOf('iphone') != -1) 
            {
                this.saveType = 2
            }
            this.changeSaveMasking()
        },
        changeCloseMasking() {
            this.closeMasking = !this.closeMasking
        },
        closeFrame(){
            localStorage.setItem("only_data", 1)
            this.changeCloseMasking()
            this.reload()
        },
        getUserSign() {
            doPost({
                n: 'AppEx',
                a: 'get_user_sign',
                accountid: this.GLOBAL.userInfo.accountid
            }).then((res) => {
                if (res.code === 0) {
                    this.isSign = res.data.isSign
                    this.canSign = res.data.canSign
                    if(this.canSign && !this.isSign)
                    {
                        this.changeSign(1)
                    }else {
                        this.changeNormalT()
                    }
                }
            })
        },
        changeSign(signCover) {
            this.signCover = signCover;
            if(this.signCover == 0)
            {
                this.changeNormalT()
            }
        },
        changeTurntable(type) {
            let turntableType = this.turntableType
            this.turntableType = 0
            if(type == 0 && turntableType == 1)
            {
                this.changeHightT()
            }
        },
        changeNormalT() {
            if(this.turntableType == 1 || this.turntableType == -1)
            {
                return
            }
            doPost({
                n: 'AppEx',
                a: 'user_turntable',
                accountid: this.GLOBAL.userInfo.accountid,
                type: 1,
            }).then((res) => {
                if (res.code === 0) {
                    let normalCount = res.data.count
                    let DayNum = res.data.turntable.DayNum
                    DayNum = DayNum ? DayNum : 0
                    if(normalCount < DayNum)
                    {
                        this.turntableType = 1
                    }else {
                        this.changeHightT()
                    }
                }
            })
        },
        changeHightT() {
            doPost({
                n: 'AppEx',
                a: 'user_turntable',
                accountid: this.GLOBAL.userInfo.accountid,
                type: 2,
            }).then((res) => {
                if (res.code === 0) {
                    let normalCount = res.data.count
                    let DayNum = res.data.turntable.DayNum
                    DayNum = DayNum ? DayNum : 0
                    if(normalCount < DayNum)
                    {
                        this.turntableType = 2
                    }
                }
            })
        },
        getCusomter() {
            doPost({
                n: 'AppEx',
                a: 'get_customer_list',
                accountid: this.GLOBAL.userInfo.accountid
            }).then((res) => {
                if (res.code === 0) {
                    let customerList = res.data['customer']
                    customerList.forEach((item) => {
                        if (this.customer == '')
                        {
                            if(item['CfgType'] == 290){
                                let url = item['CfgValue']
                                if(url.includes("line"))
                                {
                                    this.customerType = 1
                                }else if(url.includes("facebook"))
                                {
                                    this.customerType = 2
                                }else if(url.includes("whatsapp"))
                                {
                                    this.customerType = 3
                                }else if(url.includes("twitter"))
                                {
                                    this.customerType = 4
                                }else{
                                    this.customerType = 5
                                }
                                this.customer = url
                            }
                        }
                        
                    })
                }
            })
        },
        closeActive() {
            if(this.activetype == 1)
            {
                this.activetype = -1
            }else if(this.activetype == 2)
            {
                this.activetype = -2
            }
            if(this.GLOBAL.userInfo.name)
            {
                this.getUserSign()
            }
        },
        toGame(url) {
            this.loading = true
            this.url = url
        },
        reg() {
            this.showPage(51)
        },
        signin() {
            this.showPage(5)
        },
        logout() {
            this.GLOBAL.userInfo.accountid = 0
            this.GLOBAL.userInfo.name = ''
            this.GLOBAL.userInfo.passcode = ''
            localStorage.removeItem(md5('__name__'))
            localStorage.removeItem(md5('__pwd_'))
            clearInterval(this.emailTimer)
            this.showPage(1)
        },
        activityDetail(id) {
            this.id = Number(id)
            localStorage.setItem("back", this.tabIndex)
            if (this.isShowSlider) {
                this.changeSlider()
            }
            this.showPage(21)
        },
        emailDetail(id) {
            this.id = Number(id)
            localStorage.setItem("back", this.tabIndex)
            this.showPage(31)
        },
        func1() { },
        changeLan() {
            this.isShowLan = !this.isShowLan
        },
        // getEmailCount() {
        //     let that = this
        //     let et = localStorage.getItem(md5('___emailtime____'))
        //     let nowTime = parseInt(new Date().getTime() / 1000)
        //     if(et && nowTime - et <= 10)
        //     {
        //         return 
        //     }

        //     // clearInterval(that.emailTimer)
        //     doPost({
        //         n: 'AppEx',
        //         a: 'get_hall_count',
        //         accountid: that.GLOBAL.userInfo.accountid,
        //     }).then((res) => {
        //         localStorage.setItem(md5('___emailtime____'), parseInt(new Date().getTime() / 1000))
        //         that.emailCount = res.data.mail_count
        //         // that.emailTimer = setInterval(() => {
        //         //     that.getEmailCount()
        //         // }, 15000)
        //     })
        // },
        reload() {
            this.loading = true
            this.url = ""
            let that = this
            let name = localStorage.getItem(md5('__name__'))
            let pwd = localStorage.getItem(md5('__pwd_'))
            if (name) {
                name = AES.decrypt(name, this.GLOBAL.apiAesKey)
                if (name) {
                    this.GLOBAL.userInfo.name = name
                }
            }
            if (pwd) {
                pwd = AES.decrypt(pwd, this.GLOBAL.apiAesKey)
                if (pwd) {
                    this.GLOBAL.userInfo.pwd = pwd
                }
            }
            let reg = localStorage.getItem('reg')
            localStorage.removeItem('reg')
            let onlyData = localStorage.getItem("only_data")
            localStorage.removeItem('only_data')
            if (name && pwd) {
                doPost({
                    n: 'AppGame',
                    a: 'do_login',
                    mobile: name,
                    password: pwd,
                    isFirst: reg
                }).then((res) => {
                    if (res.code === 0) {
                        localStorage.setItem(md5('__name__'), AES.encrypt(name, that.GLOBAL.apiAesKey))
                        localStorage.setItem(md5('__pwd_'), AES.encrypt(pwd, that.GLOBAL.apiAesKey))

                        that.GLOBAL.userInfo.accountid = res.data.accountid
                        that.GLOBAL.userInfo.nickname = res.data.nickname
                        that.GLOBAL.userInfo.name = name
                        that.GLOBAL.userInfo.passcode = pwd
                        that.GLOBAL.userInfo.isVirtual = res.data.isVirtual
                        that.amount = res.data.amount

                        // that.getEmailCount(true)
                        if(!onlyData)
                        {
                            that.tabIndex = 1

                            if(that.activetype >= -1)
                            {
                                that.activetype = 2
                                if(that.activeList.length <= 0 || that.activeList[1]['active'] == '')
                                {
                                    that.getUserSign()
                                }
                            }
                        }
                    } else {
                        // const data = decodeApiLan(res, this.GLOBAL.lanArr);
                        // this.$toast.error(data.message, {
                        //     position: "top-center",
                        //     timeout: 2000,
                        //     closeOnClick: true,
                        //     pauseOnFocusLoss: true,
                        //     pauseOnHover: true,
                        //     draggable: true,
                        //     draggablePercent: 0.6,
                        //     showCloseButtonOnHover: false,
                        //     hideProgressBar: true,
                        //     closeButton: false,
                        //     icon: true,
                        //     rtl: false
                        // });
                        
                        if(that.activetype >= 0)
                        {
                            that.activetype = 1
                        }
                        this.logout()
                    }
                    this.loading = false
                })
            } else {
                if(that.activetype >= 0)
                {
                    that.activetype = 1
                }
                this.loading = false
            }

        },
        showPage(index) {
            this.isShowLan = false
            this.signCover = -1
            this.turntableType = -1
            if((!this.GLOBAL.userInfo.name || !this.GLOBAL.userInfo.pwd) & (index == 3 || index ==  4 || index == 6||index == 10))
            {
                index = 5
            }
            if (this.tabIndex != index) {
                    this.tabIndex = index
                    this.closeActive()
                    window.scrollTo(0,0)
                }
        },
        changeSlider() {
            if (this.isShowSlider) {
                this.isShowSlider = !this.isShowSlider
                this.isShowLan = false
            } else {
                doPost({
                    n: 'AppEx',
                    a: 'get_activity_list',
                    accountid: this.GLOBAL.userInfo.accountid,
                }).then((res) => {
                    // this.activityList = res.data.list
                    // if(this.activityList != null)
                    // {
                    //     this.activityList.forEach(item => {
                    //         let suffix = item.img.slice(-4)
                    //         item.img = item.img.replace(suffix, "_mini_"+this.GLOBAL.lanCode+suffix)
                    //     });
                    // }
                    this.isShowSlider = !this.isShowSlider
                })
            }
        },
        toDeposit() {
            if (!this.GLOBAL.userInfo.accountid || this.GLOBAL.userInfo.accountid == 0) {
                this.$toast.error(this.GLOBAL.lanLocal['nologin'], {
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
                return
            }
            localStorage.setItem("back", this.tabIndex)
            this.showPage(6)
        },
        toBind() {
            localStorage.setItem("back", this.tabIndex)
            this.showPage(9)
        },
        closeModal() {
            let back = localStorage.getItem("back")
            if (back) {
                this.showPage(Number(back))
                localStorage.removeItem("back")
            } else {
                this.showPage(1)
            }
            this.id = 0
        },
        changeLanCode(lanCode) {
            if (lanCode == this.lanCode) {
                this.showLan = false
                return
            }
            this.loading = true
            this.lanCode = lanCode
            localStorage.setItem(md5('__lanCode__'), this.lanCode)
            setTimeout(() => {
                window.location.reload()
            }, 100)
        },
    }
}
</script>

<style lang="less" scoped>

.background {
    position: fixed;
    z-index: -1;
    width: 100%;
    height: 100vh;
    background: url(../../../static/background.gif);
    background-repeat: no-repeat;
    background-size: cover;
}

.container {
    display: flex;
    margin-top: 60px;
}

.slider-menu-masking {
    position: fixed;
    top: 70px;
    left: 0;
    z-index: 4;
    display: flex;
    justify-content: center;
}

@media only screen and (max-width: 450px) {
    .slider-menu-masking {
        display: none;
    }

    .main-wrap {
        margin-left: 0px !important;
        width: 100% !important;
    }
}

.slider-menu {
    background-color:rgb(20, 18, 18);
    height: 100vh;
    padding: 22px 18px 30px;
    text-align: left;
    box-sizing: border-box;
    transition: all 0.1s;

    .activity-title {
        color: rgb(233, 222, 124);
        font-size: 16px;
    }

    .activity-list {
        margin: 10px 0 30px;

        .activity-item {
            margin-bottom: 6px;

            img {
                width: 100%;
                height: auto;
                border-radius: 6px;
            }
        }
    }
    .menu-list {
        margin-bottom: 30px;
        :hover {
            background-color: rgb(126, 123, 123);
        }
        transition: all 0.3s;
    }
    .menu-item {
        width: 100%;
        height: 50px;
        display: flex;
        align-items: center;
        justify-content: flex-start;
        border-radius: 3px;
        position: relative;
        padding: 0px 2px;
        box-sizing: border-box;
        text-decoration: none;
        position: relative;
        margin-bottom: 20px;
        .icon {
            height: 30px;
            width: auto;
            margin-right: 8px;
        }

        div {
            color: #fff;
            font-size: 14px;
            font-family: Arial;
            line-height: 1;
        }

        .down {
            width: 18px;
            height: auto;
        }

        .lan-desc {
            display: flex;
            align-items: center;
            justify-content: space-between;
            flex: 1;
        }

        .lan-box {
            position: absolute;
            left: 0;
            top: 52px;
            width: 100%;
            background-color: rgb(39, 44, 50);
            border-radius: 6px;

            .lan-item {
                width: 100%;
                height: 50px;
                display: flex;
                align-items: center;
                justify-content: flex-start;
                position: relative;
                padding: 0 20px;
                box-sizing: border-box;
                // border-bottom: 1px solid rgb(44, 50, 57);

                .icon {
                    height: 30px;
                    width: 30px;
                    margin-right: 8px;
                }

                div {
                    color: rgb(144, 151, 161);
                    font-size: 16px;
                }
            }
        }
    }

    .menu-top {
        margin-top: 6px;
    }
}


.active-content {
    width: calc(100vw - 40px);
    max-height: 100%;
    .img {
        max-width: 100%;
        max-height: 100%;
        border-radius: 10px;
        overflow: hidden;
    }
    .topage {
        background: rgb(233, 116, 20);
        color: #fff;
        width: 115px;
        margin: 15px auto;
        border-radius: 20px;
    }
    .close {
        width: 16px;
        height: 16px;
        border-radius: 16px;
        border: 1px solid #fff;
        padding: 5px;
    }
}

.page-masking {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 998;
    height: 100%;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
}
.frameBack {
    position: fixed;
    top: 20px;
    left: 24px;
    z-index: 9999;
    >img {
        height: 40px;
        width: 40px;
    }
}
.save-masking {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 99999;
    height: 100%;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    .save-box {
        background-color: rgba(0,0,0,0.8);
        border: 1px solid rgba(200,200,200,0.8);
        width: 80%;
        border-radius: 6px;
        padding: 6px;
        text-align:right;
        color: #fff;
        font-family: Arial;
        .close-icon {
            height: 24px;
            width: auto;
        }
        .save-title {
            text-align:center;
            font-size: 18px;
        }
        .save-content {
            padding: 14px;
            .tips {
                font-size: 16px;
                text-align:left;
                line-height:1.3;
            }
            .save-guide {
                position: relative;
                margin-top: 16px;
                .guide-bg {
                    width: 100%;
                    height: auto;
                }
                >div {
                    position: absolute;
                    top: 14px;
                    left: 0;
                    width: 100%;
                    font-size: 14px;
                    text-align:center;
                }
            }
            .android {
                >div {
                    color: #333;
                    top: calc(50% - 10px);
                    text-align: left;
                    padding-left: 62px;
                    width: 110px;
                    overflow: hidden;
                }
            }
            .tutorial {
                margin-top: 8px;
                width: 100%;
                height: auto;
            }
        }
        .save-desc {
            padding: 6px 26px 12px;
            font-size: 14px;
            color: #55abff;
            text-align: left;
            line-height:1.3;
        }
    }
}
.close-masking {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 99999;
    height: 100%;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    .close-box {
        background-color: rgb(41,72,154);
        border: 2px solid rgb(78,109,173);
        width: 80%;
        border-radius: 6px;
        padding: 0 26px;
        box-sizing: border-box;
        .close-title {
            text-align:center;
            color: #fff;
            font-size: 18px;
            padding: 20px 0 10px;
        }
        .close-content {
            background: rgb(0,39,118);
            border-radius: 6px;
            padding: 8px 12px;
            font-size: 14px;
            text-align:left;
            color: #fff;
        }
        .close-btn {
            display: flex;
            align-items:center;
            justify-content: space-between;
            .cancel, .confirm {
                border-radius:42px;
                height: 42px;
                line-height: 42px;
                width: 120px;
                font-size: 16px;
                text-align:center;
                margin: 20px 0;
                font-family: Arial;
                font-weight: 400;
                letter-spacing: 1.2px;
            }
            .cancel {
                background-color:#fff;
                border: 2px solid rgb(82,208,73);
                color: rgb(82,208,73);
            }
            .confirm {
                background-color:rgb(82,208,73);
                color: #fff;
            }
        }
    }
}
.gameFrame {
    height: 100%;
    width: 100%;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 999;
    border: 0;
    background-color: #201f27;
}
.empty {
    padding-top: 10px;
    width:100%;
    display: flex;
    align-items: center;
    justify-content: center;
    img {
        height: 50px;
        width: auto;
    }
    .tips {
        text-align:center;
        font-size: 14px;
        color: #efcf7f;
    }
}
</style>