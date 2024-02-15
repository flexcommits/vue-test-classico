<template>
    <div>
        <div class="main-page" v-if="!allGameCover">
            <div class="line-box" style="bottom: 160px;">
                <a class="contack-link" :href="newLineCustomer" target="_blank">
                    <img class="line-icon" :src="require('../../assets/index/service.png')" />
                </a>
            </div>
            <div class="line-box">
                <a class="contack-link" :href="lineCustomer" target="_blank">
                    <img class="line-icon" v-if="customerType == 1" :src="require('../../assets/line.png')" />
                    <img class="line-icon" v-if="customerType == 2" :src="require('../../assets/fb.png')" />
                    <img class="line-icon" v-if="customerType == 3" :src="require('../../assets/whats.png')" />
                    <img class="line-icon" v-if="customerType == 4" :src="require('../../assets/twitter.png')" />
                    <img class="line-icon" v-if="customerType == 5" :src="require('../../assets/telegram.png')" />
                </a>
            </div>
            <div class="banner">
                <div class="recommendPage">
                    <swiper :options="swiperOption" ref="mySwiper">
                        <swiper-slide v-for="(item, index) in bannerList" :key="index + 1">
                            <img :src="item">
                        </swiper-slide>
                    </swiper>
                </div>
                <div class="stopSliderPage">
                    <div>
                        <img class="back" :src="require('../../assets/background/back-1.png')" />
                    </div>
                    <div>
                        <img class="back" :src="require('../../assets/background/back-2.png')" />
                    </div>
                    <div>
                        <img class="back" :src="require('../../assets/background/back-3.png')" />
                    </div>
                </div>
            </div>
            <div class="nav-menu">
                <div class="nav-item" v-for="(item, index) in navmenu" :style="{background: item.type == selectedNavItem ? 'linear-gradient(180deg,#6892a4,#416375)': ''}" :key="index + 1" @click="changnav(item.type)">
                    <img class="nav-img" :src="item.img">
                    <div class="text-wrap" >{{ item.name }}</div>
                </div>
            </div>
            <!-- <div class="menu-title">
                <div v-for="(item, index) in navmenu">
                    <div v-if="navid == item.id" :key="index + 1" class="title-box">
                        <img class="title-img" v-if="item.id == 0" :src="require('../../assets/icon/hot.png')" />
                        <div class="title-text">{{ item.name }}</div>
                    </div>
                </div>
            </div> -->
            <div class="wrap-container">
                <div class="item-content">
                    <div class="view-game-container">
                        <div class="wrap-game-show">
                            <div class="container-list-game-slot">
                                <div class="list-game-bacarat">
                                    <template v-for="(game, index) in gameList">
                                        <div v-if="selectedNavItem==game.type" class="game-menu">
                                            <img class="menu-icon" :src="sourceUrl+game.title+'.png'" />
                                            <span>{{game.title}}</span>
                                            <div class="right">
                                                <div class="all" @click="showAllGame(game.type)">{{GLOBAL.lanLocal['all']}} {{game.list.length}}</div>
                                            </div>
                                        </div>
                                        <template v-if="selectedNavItem==game.type" v-for="(item,i) in game.list">
                                            <div class="game-item no-animate" v-if="i < 6" @click="toGame(item.gameid, item.type, item.roomid, item.open, item)">
                                                <img class="game-img" v-if="game.type == 3" :src="'https://lucas999.prerelease-env.biz/game_pic/square/200/'+item.gameid+'.png'" />
                                                <img class="game-img" v-else :src="sourceUrl+item.roomid+'_'+GLOBAL.lanCode+'.png'" />
                                            </div>
                                        </template>
                                    </template>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="page-bottom">
                <div class="cop-icon">
                    <img :src="item" v-for="(item, i) in copIcon" :key="i+1" />
                </div>
                <div class="sup-icon">
                    <img :src="item" v-for="(item, i) in supIcon" :key="i+1" />
                </div>
                <div class="service-title">{{GLOBAL.lanLocal['serviceterms']}}</div>
                <div class="service-content">{{serviceContent}}</div>
            </div>
            <div class="copyright">{{copyrightContent}}</div>
        </div>
        <div class="main-page game-cover" :style="{'height': innerHeight + 'px'}"  @scroll="handleScroll" v-else>
            <div class="wrap-container">
                <div class="item-content">
                    <div class="view-game-container">
                        <div class="wrap-game-show">
                            <div class="container-list-game-slot">
                                <div class="list-game-bacarat">
                                    <div class="game-menu" style="background-color: hsla(0,0%,100%,.1) !important;">
                                        <img class="menu-icon" v-if="allGame.title" :src="sourceUrl+allGame.title+'.png'" />
                                        <span>{{allGame.title}}</span>
                                        <div class="right" @click="showAllGame(999)">
                                            <img :src="require('../../assets/index/close.png')" />
                                        </div>
                                    </div>
                                    <template v-for="(item,i) in allGame.list">
                                        <div class="game-item no-animate" @click="toGame(item.gameid, item.type, item.roomid, item.open, item)">
                                            <img class="game-img" v-if="allGame.type == 3" :src="'https://lucas999.prerelease-env.biz/game_pic/square/200/'+item.gameid+'.png'" />
                                            <img class="game-img" v-else :src="sourceUrl+item.roomid+'_'+GLOBAL.lanCode+'.png'" />
                                        </div>
                                    </template>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="loading-masking" v-if="loading">
            <div class="loading-page">
                <vue-simple-spinner size="large" line-fg-color="#f69b09"></vue-simple-spinner>
            </div>
        </div>
        <div class="close-masking" v-if="closeMasking">
            <div class="close-box">
                <div class="close-title">{{GLOBAL.lanLocal['hint']}}</div>
                <div class="close-content">{{GLOBAL.lanLocal['insufficientbalance']}}</div>
                <div class="close-btn">
                    <div class="cancel" @click="changeCloseMasking">{{GLOBAL.lanLocal['ok']}}</div>
                    <div class="confirm" @click="toRecharge">{{GLOBAL.lanLocal['recharge']}}</div>
                </div>
            </div>
        </div>
    </div>
</template>
  
<script>
import { swiper, swiperSlide } from "vue-awesome-swiper";
import "swiper/dist/css/swiper.css";
import md5 from 'js-md5';
import AES from "../../common/AES.js"
import { doPost } from '../../api/api.js'
import { v4 as uuidv4 } from 'uuid'
import decodeApiLan from '../../api/decodeApiLan.js'
import loadFile from "../../api/loadFile.js"
export default {
    name: 'home',
    components: {
        swiper,
        swiperSlide
    },
    data() {
        return {
            copIcon: [
                require('../../assets/index/in.png'),
                require('../../assets/index/fb.png'),
                require('../../assets/index/ins.png'),
                require('../../assets/index/YT.png'),
                require('../../assets/index/T.png'),
                require('../../assets/index/whats.png'),
                require('../../assets/index/DY.png')
            ],
            supIcon: [
                require('../../assets/index/google.png'),
                require('../../assets/index/PG.png'),
                require('../../assets/index/pp.png'),
                require('../../assets/index/FB-.png'),
            ],
            swiperOption: {
                loop: true,
                autoplay: {
                    delay: 3000,
                    stopOnLastSlide: false,
                    disableOnInteraction: false
                }
            },
            navmenu: [],
            gameList: [],
            allGame: {
                title: '',
                list: []
            },
            allGameCover: false,
            isSearch: false,
            searchValue: '',
            navid: 1,
            isMasking: false,
            isLoginPage: false,
            isRegisterPage: false,
            isServicePage: false,
            isMyPage: false,
            isForgotPage: false,
            isDepositPage: false,
            isWithdrawPage: false,
            fromW: 0,
            url: '',
            favList: [],
            favStr: '',
            lineCustomer: '',
            newLineCustomer: '',
            scrollTop: undefined, // 距离顶端的值
            className: 'modalOpen', // 类名
            isShowLan: false,
            loading: false,
            account: '',
            password: '',
            isBindBack: false,
            activityList: [],
            innerHeight: 0,
            showLogin: false,
            bannerList: [],
            config: [],
            serviceContent: '',
            copyrightContent: '',
            customerType: 0,
            sourceUrl: '',
            gameNo: 0,
            allGameLib: [],
            closeMasking: false,
            selectedNavItem: 0,
        }
    },
    mounted(){
        const count = loadFile("static/banner/count", false);
        for (let index = 1; index <= count; index++) {
            let item = '../../../static/banner/' + index + "_" +this.GLOBAL.lanCode + '.png' 
            this.bannerList.push(item)
        }
    },
    created() {
        this.setData()
        this.setShareid()
        this.getCustomer()
        // this.reload(true)
        this.gameList = loadFile("static/game.json", false);

        this.config = loadFile("static/config.json", false);
        document.title = this.config.website
        this.sourceUrl = this.config.sourceUrl
        this.GLOBAL.currencySymbol = this.config.currencySymbol
        this.GLOBAL.currency = this.config.currency
        this.serviceContent = this.GLOBAL.lanLocal['servicecontent'].replace("%s", this.config['website'])
        let copyright = this.GLOBAL.lanLocal['copyright']
        let year = new Date().getFullYear()
        this.copyrightContent = copyright.replace("%d", year).replace("%s", this.config['weburl'])
    },
    computed: {
        swiper() {
            return this.$refs.mySwiper.swiper;
        }
    },
    methods: {
        changeCloseMasking() {
            this.closeMasking = !this.closeMasking
        },
        handleScroll() {
            const container = document.querySelector('.game-cover');
            const { scrollTop, scrollHeight, clientHeight } = container;
            this.innerHeight = window.innerHeight
            if (scrollTop + clientHeight + 100 >= scrollHeight) {
                this.pushGame()
            }    
        },
        showAllGame(type) {
            if(this.GLOBAL.userInfo.name && this.GLOBAL.userInfo.pwd)
                {
                    this.allGame = {
                    title: '',
                    type: 0,
                    list: []
                }
                this.gameNo = 0
                if(type == 999) {
                    this.allGameCover = false
                }else {
                    this.gameList.forEach(item => {
                    if(item.type == type)
                        {
                            this.allGameLib = item
                            this.pushGame()
                            this.allGameCover = true
                        }
                    })
                }
            } else {
                    this.toSignin()
                }
        },
        pushGame() {
            if(this.gameNo == 0)
            {
                this.allGame.title = this.allGameLib.title
                this.allGame.type = this.allGameLib.type
                this.innerHeight = window.innerHeight
                setTimeout(() => {
                    const scrollEle = document.querySelector('.game-cover')
                    scrollEle.scrollTo({ top: 0, behavior: 'smooth' })
                }, 500);
            }
            for (let index = 0; index < 30; index++) {
                if(this.gameNo < this.allGameLib.list.length)
                {
                    this.allGame.list.push(this.allGameLib.list[this.gameNo++])
                }
            }
        },
        toForgot() {
            this.$emit('toForgot')
        },
        toReg() {
            this.$emit("reg")
        },
        toRecharge() {
            this.$emit("recharge")
        },
        toEmail() {
            this.$router.push({ path: '/email' })
        },
        toActivity() {
            this.$router.push({ path: '/activity' })
        },
        logout() {
            this.GLOBAL.userInfo.accountid = 0
            this.GLOBAL.userInfo.name = ''
            this.GLOBAL.userInfo.passcode = ''
            localStorage.removeItem(md5('__name__'))
            localStorage.removeItem(md5('__pwd_'))
        },
        setData() {
            this.navmenu = [
                { 'name': this.GLOBAL.lanLocal['hot'], 'img': require('../../assets/index/hotsicon.png'), 'type': 0 },
                { 'name': this.GLOBAL.lanLocal['pp'], 'img': require('../../assets/index/ppicon.png'), 'type': 3 },
                { 'name': this.GLOBAL.lanLocal['lucas'], 'img': require('../../assets/index/CLASSICO777.png'), 'type': 1 },
                { 'name': this.GLOBAL.lanLocal['pg'], 'img': require('../../assets/index/pgicon.png'), 'type': 4 },
                { 'name': this.GLOBAL.lanLocal['vip'], 'img': require('../../assets/index/vipicon.png'), 'type': -1 },
            ]
        },
        setShareid() {
            var url = window.location.href
            var dz_url = url.split('#')[0]
            var cs = dz_url.split('?')[1]
            if (cs) {
                var cs_arr = cs.split('&')
                var cs = {}
                for (let index = 0; index < cs_arr.length; index++) {
                    cs[cs_arr[index].split('=')[0]] = cs_arr[index].split('=')[1]
                }
                let shareid = cs['accountid']
                if (shareid) {
                    localStorage.setItem(md5("__shareid___"), shareid)
                    doPost({
                        n: 'AppShare',
                        a: 'add_share',
                        accountid: shareid,
                        udid: this.GLOBAL.deviceid
                    }).then((res) => {

                    })
                }
            }
        },
        // 打开弹层 要做的事
        afterOpen() {
            this.scrollTop = document.scrollingElement.scrollTop;
            document.body.classList.add(this.className);
            document.body.style.top = `-${this.scrollTop}px`;
        },
        // 弹层关闭之前 要做的事
        beforeClose() {
            document.body.classList.remove(this.className);
            document.scrollingElement.scrollTop = this.scrollTop;
        },
        func1() {
        },
        toGame(gameid, type, roomid, open, game) {
            if(this.GLOBAL.userInfo.isVirtual && game.v_gameid && game.v_type && game.v_roomid)
            {
                gameid = game.v_gameid
                type = game.v_type
                roomid = game.v_roomid
            }
            if (!open || ((type == 1 || type == 2) && this.GLOBAL.userInfo.isVirtual)) {
                this.$toast.warning(this.GLOBAL.lanLocal['undermaintenance'], {
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
            if (this.GLOBAL.userInfo.name && this.GLOBAL.userInfo.pwd) {
                this.loading = true
                let name = this.GLOBAL.userInfo.name
                let pwd = this.GLOBAL.userInfo.pwd
                let callback = window.location.origin
                doPost({
                    n: 'AppGame',
                    a: 'get_game_url',
                    mobile: name,
                    password: pwd,
                    type: type,
                    gameid: gameid,
                    callback: callback,
                    roomid: roomid,
                    lan: this.GLOBAL.lanCode
                }).then((res) => {
                    if (res.code === 0) {
                        let url = res.data.url
                        if(url)
                        {
                            if(type == 1)
                            {
                                window.location.replace(url)
                            }else{
                                this.$emit("toGame", url)
                            }
                        }else {
                            this.$toast.warning(this.GLOBAL.lanLocal['undermaintenance'], {
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
                    }else if(res.code == -2){
                        this.changeCloseMasking()
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
                    this.loading = false
                })
            } else {
                this.toSignin()
            }
        },
        changnav(type) {
            this.selectedNavItem = type
            if(type == -1)
            {
                if(this.GLOBAL.userInfo.name && this.GLOBAL.userInfo.pwd)
                {
                    this.$emit("vip")
                }else {
                    this.toSignin()
                }
            }
        },
        toSignin() {
            this.$emit("signin")
        },
        getCustomer() {
            doPost({
                            n: 'AppEx',
                            a: 'get_customer_list',
                            accountid: this.GLOBAL.userInfo.accountid
                        }).then((res) => {
                            if (res.code === 0) {
                                let customerList = res.data['customer']
                                customerList.forEach((item) => {
                                    if (this.lineCustomer == '')
                                    {
                                        if(item['CfgType'] == 292){
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
                                            this.lineCustomer = url
                                            this.GLOBAL.lineCustomer = this.lineCustomer
                                        }
                                    }
                                    if (this.newLineCustomer == '')
                                    {
                                        if(item['CfgType'] == 290){
                                            let url = item['CfgValue']
                                            this.newLineCustomer = url
                                        }
                                    }
                                    
                                })
                            }
                        })
        },
        reload(isAuto) {
            return ;
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
            } else if (this.account) {
                name = this.account
            }
            if (pwd) {
                pwd = AES.decrypt(pwd, this.GLOBAL.apiAesKey)
                if (pwd) {
                    this.GLOBAL.userInfo.pwd = pwd
                }
            } else if (this.password) {
                pwd = md5(this.password)
            }
            let reg = localStorage.getItem('reg')
            localStorage.removeItem('reg')
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
                        that.amount = res.data.amount

                        this.$emit("reload")
                    } else {
                        if (!isAuto) {
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
                        } else {
                            this.logout()
                        }
                    }
                    this.loading = false
                })
            } else {
                
                this.logout()
                this.loading = false
            }
            // }
        }
    }
}
</script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
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


.line-box:focus-visible {
    outline: none;
}

.line-box {
    position: fixed;
    right: 12px;
    bottom: 100px;
    z-index: 10;
    .line-icon:focus-visible {
        outline: none;
    }

    .line-icon {
        height: 45px;
        width: 45px;
    }
}

.account-box {
    margin: 4px;

    .forgot {
        color: rgb(255, 182, 0);
        padding-left: 8px;
        text-align: left;
    }

    .login-box {
        display: flex;
        align-items: center;
        box-sizing: border-box;
        padding: 4px 6px;

        .account,
        .password {
            margin-right: 6px;
            border-radius: 4px;
            height: 20px;
            border: 1px solid rgb(255, 182, 0);
            display: flex;
            box-sizing: border-box;
        }

        .account {
            width: 30%;
        }

        .password {
            width: 30%;
        }

        .account-input:focus-visible,
        .password-input:focus-visible {
            outline: 0px;
        }

        .account-input,
        .password-input {
            text-align: center;
            border-radius: 4px;
            font-size: 14px;
            width: 100%;
            background-color: #000;
            color: #fff;
            border: none;
            line-height: 1;
        }

        .account-input::placeholder,
        .password-input::placeholder,
        .account-input::-webkit-input-placeholder,
        .password-input::-webkit-input-placeholder,
        .account-input:-moz-placeholder,
        .password-input:-moz-placeholder,
        .account-input::-moz-placeholder,
        .password-input::-moz-placeholder,
        .account-input:-ms-input-placeholder,
        .password-input:-ms-input-placeholder {
            text-align: center;
            color: #999;
            font-size: 10px;
            line-height: 1px;
        }
        ::-webkit-input-placeholder { 
            text-align: center;
            color: #999;
            font-size: 10px;
            line-height: 1px;
        }
        :-moz-placeholder { 
            text-align: center;
            color: #999;
            font-size: 10px;
            line-height: 1px;
        }
        ::-moz-placeholder { 
            text-align: center;
            color: #999;
            font-size: 10px;
            line-height: 1px;
        }
        :-ms-input-placeholder { 
            text-align: center;
            color: #999;
            font-size: 10px;
            line-height: 1px;
        }

        .login-btn {
            margin-right: 6px;
            background-color: rgb(255, 182, 0);
            color: #333;
        }

        .reg-btn {
            color: rgb(255, 182, 0);
            background-color: #000;
        }

        .login-btn,
        .reg-btn {
            border: 1px solid rgb(255, 182, 0);
            width: 20%;
            font-size: 10px;
            height: 20px;
            line-height: 1px;
            border-radius: 4px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
    }
}

.banner {
    width: 94%;
    height: auto;
    font-size: 0;
    margin: 12px auto 22px;
    box-sizing: border-box;
    border-radius: 8px;
    overflow:hidden;

    .stopSliderPage {
        display: flex;
        img {
            width: 90%;
            height: auto;
            margin: 5px;
        }
    }

    .recommendPage {
        display: none;
        margin-top: 10px;
        img {
            width: 300px;
        }
    }


}

@media only screen and (max-width: 450px) {
    .banner {
        .stopSliderPage {
            display: none;
        }

        .recommendPage {
            display: block;
        }
    }
}

.swiper-container {
    z-index: 0 !important;
}

.nav-menu::-webkit-scrollbar {
    display: none;
}

.nav-menu {
    display: flex;
    align-items:center;
    padding-left: 60px;
    .nav-item {
        text-align:center;
        position:relative;
        border-radius: 27px;
        width: 180px;
        height: 48px;
        display: flex;
        justify-content: center;
        align-items: center;
        margin-right: 10px;
        margin-top: 20px;

        @media only screen and (max-width: 425px) {
            .nav-item {
                width: 30px;
            }
        }

        .nav-img {
            height: 30px;
            width: 30px;
            margin-right: 5px;
        }

        .text-wrap {
            color: hsla(0,0%,100%,.7);
            font-size: 12px;
            font-weight: 400;
            font-family: Arial;
        }
    }
}

@media only screen and (max-width: 450px) {
    .nav-menu {
        padding-left: 10px;
        .nav-item {
            border-radius: 5px;
            flex-direction: column;
        }
    }
}

.menu-title {
    padding: 0 20px 11px;
    text-align: left;

    .title-box {
        display: flex;
        align-items: center;
    }

    .title-img {
        width: 19px;
        height: 22px;
    }

    .title-text {
        margin-left: 4px;
        color: #fff;
        font-size: 16px;
    }
}

.wrap-container {
    display: flex;
    margin: 0px 40px;
    .item-content {
        width: 100%;
        padding: 0 8px;
        margin: auto;

        .view-game-container {
            display: flex;
            flex-wrap: wrap;
            flex-direction: column;

            .side-bar-category {
                width: 100%;
                font-size: 0;

                .menu-side-bar {
                    width: 100%;
                    padding: 0;

                    .nav-menu-side-bar {
                        .nav-side-bar-menu {
                            justify-content: start;
                            flex-wrap: wrap;
                            max-width: 100%;
                            width: 100%;
                            white-space: nowrap;
                            display: flex;
                            list-style: none;
                            padding-left: 0;

                            .width-auto {
                                width: 20% !important;
                                font-size: 12px !important;
                            }

                            .nav-item {
                                margin-top: 0;
                                margin-bottom: 0.7rem;
                                padding: 0 3px;
                                width: 25%;
                                box-sizing: border-box;

                                .nav-link {
                                    width: 100%;
                                    position: relative;
                                    align-items: center;
                                    justify-content: flex-start;
                                    box-shadow: 0 4px 10px rgba(0, 0, 0, .75);
                                    border-radius: 8px;
                                    border: none;
                                    text-decoration: none;
                                    color: hsla(0, 0%, 100%, .8);
                                    transition: all .4s;
                                    padding: 0;
                                    display: flex;
                                    flex-direction: column;

                                    .nav-img {
                                        filter: drop-shadow(0 0 5px #000);
                                        transition: all .2s ease-in-out;
                                        width: 50px;
                                        height: 50px;
                                        -o-object-fit: contain;
                                        object-fit: contain;
                                        z-index: 2;
                                    }

                                    .text-wrap {
                                        z-index: 2;
                                        margin-left: 0;
                                        display: flex;
                                        flex-direction: column;

                                        .text-span {
                                            font-size: 14px;
                                            white-space: nowrap;
                                            width: 100%;
                                            overflow: hidden;
                                            text-overflow: ellipsis;
                                        }
                                    }
                                }

                                .nav-link.active:before {
                                    transform: scaleX(1);
                                    transform-origin: top right;
                                }

                                .nav-link:before {
                                    transform: scaleX(0);
                                    transform-origin: top right;
                                    border-radius: 8px;
                                    content: " ";
                                    display: block;
                                    position: absolute;
                                    top: 0;
                                    right: 0;
                                    bottom: 0;
                                    left: 0;
                                    background: linear-gradient(180deg, #ffdf46, #ffdc42 23.96%, #feeb52 49.48%, #fbc316 71.87%, #f69b09);
                                    z-index: 0;
                                    transition: transform .3s ease;
                                }

                                .nav-link:after {
                                    content: "";
                                    position: absolute;
                                    top: 40%;
                                    right: 0;
                                    transform: translateY(-50%);
                                    width: 3px;
                                    height: 55%;
                                    background: hsla(0, 0%, 100%, .3);
                                    border-radius: 0 20px 20px 0;
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}

@media only screen and (max-width: 450px) {
    .wrap-container {
        margin: 0px;
    }
}

.wrap-banner {
    z-index: 2;
}

.wrap-game-show {
    width: 100%;
}

.container-list-game-slot {
    position: relative;
    min-height: auto;
    display: flex;
    flex-wrap: wrap;
}

.searching-game {
    display: flex;
    justify-content: space-between;
    width: calc(100% - 60px);
    margin-left: auto;
    margin-top: 0.8rem;
    margin-bottom: 0.2rem;
}

.v-input {
    width: 95%;
    margin: auto;
    max-width: 350px;
    align-items: flex-start;
    display: flex;
    flex: 1 1 auto;
    font-size: 16px;
    letter-spacing: normal;
    text-align: left;
    border-radius: 28px;
    position: relative;
}

.v-input__control {
    min-height: 38px;
    display: flex;
    flex-direction: column;
    height: auto;
    flex-grow: 1;
    flex-wrap: wrap;
    min-width: 0;
    width: 100%;
    border-radius: 28px;
}

.v-input__slot {
    box-shadow: 0 3px 1px -2px rgba(0, 0, 0, .2), 0 2px 2px 0 rgba(0, 0, 0, .14), 0 1px 5px 0 rgba(0, 0, 0, .12);
    min-height: 40px;
    background-color: #1e1e1e;
    border-radius: inherit;
    cursor: text;
    align-items: stretch;
    color: inherit;
    display: flex;
    margin-bottom: 0;
    position: relative;
    transition: .3s cubic-bezier(.25, .8, .5, 1);
    width: 100%;
    padding: 0 24px;
    border-radius: 28px;
    box-sizing: border-box;
}

.v-text-field__slot {
    display: flex;
    flex: 1 1 auto;
    position: relative;
    align-items: center;
}

.v-label {
    top: 11;
    transform-origin: top left;
    color: hsla(0, 0%, 100%, .7);
    height: 20px;
    line-height: 20px;
    letter-spacing: normal;
    max-width: 90%;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    pointer-events: none;
}

.search-input {
    caret-color: auto;
    padding: 4px 0 2px;
    color: #fff;
    max-height: 32px;
    flex: 1 1 auto;
    line-height: 20px;
    max-width: 100%;
    min-width: 0;
    width: 100%;
    background-color: transparent;
    border-style: none;
    border: none;
}

.search-input:focus-visible {
    border-style: none;
    border: none;
    outline: none;
}

.v-input__append-inner {
    margin-left: auto;
    padding-left: 4px;
    align-self: center;
    margin-top: 0;
    display: inline-block;
    line-height: 1;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
}

.v-input__icon {
    align-items: center;
    display: inline-flex;
    height: 24px;
    flex: 1 0 auto;
    justify-content: center;
    min-width: 24px;
    width: 24px;
}

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
    color: #fff;
}

.noshow {
    display: none;
}

.list-game-bacarat {
    display: flex;
    flex-wrap: wrap;
    align-content: baseline;
    width: 100%;
}

.game-menu {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: flex-end;
    padding: 0 7px 12px;
    margin-top: 16px;

    .menu-icon {
        width: auto;
        height: auto;
        max-height: 20px;
        max-width: 80px;
        padding-bottom: 2px;
    }
    >span {
        font-family: Arial;
        font-size: 14px;
        color: white;
        font-weight: 700;
        margin-left: 4px;
    }
    .right {
        flex: 1;
        .all {
            float: right;
            padding: 0 8px;
            height: 30px;
            line-height: 20px;
            text-align:center;
            font-size: 10px;
            font-family: Arial;
            background: linear-gradient(180deg,#6892a4,#416375);
            border-radius: 10px;
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        >img {
            height: 20px;
            width: auto;
            float:right;
        }
    }
}

.game-item {
    width: 100px;
    height: auto;
    position: relative;
    box-sizing: border-box;
    margin: 10px 12px;

    .v-btn {
        border: none;
        align-items: center;
        border-radius: 4px;
        display: inline-flex;
        flex: 0 0 auto;
        font-weight: 500;
        letter-spacing: .0892857143em;
        justify-content: center;
        outline: 0;
        position: relative;
        text-decoration: none;
        text-indent: 0.0892857143em;
        text-transform: uppercase;
        transition-duration: .28s;
        transition-property: box-shadow, transform, opacity;
        transition-timing-function: cubic-bezier(.4, 0, .2, 1);
        -webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
        vertical-align: middle;
        white-space: nowrap;
        height: 36px;
        min-width: 64px;
        position: absolute;
        z-index: 2;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        background: linear-gradient(180deg, #ffdf46, #ffdc42 23.96%, #feeb52 49.48%, #fbc316 71.87%, #f69b09);
        padding: 0.5rem 0;
        border-radius: 5px;
        width: 70%;
        text-align: center;
        font-weight: 700;
        font-size: 16px;
        color: #212121;
        visibility: hidden;
        opacity: 0;
        transition: visibility .2s, opacity .2s linear;

        .v-btn__content {
            align-items: center;
            color: inherit;
            display: flex;
            flex: 1 0 auto;
            justify-content: inherit;
            line-height: normal;
            position: relative;
            transition: inherit;
            transition-property: opacity;
        }
    }

    .v-btn--is-elevated {
        box-shadow: 0 3px 1px -2px rgba(0, 0, 0, .2), 0 2px 2px 0 rgba(0, 0, 0, .14), 0 1px 5px 0 rgba(0, 0, 0, .12);
    }

    .v-btn:before {
        background-color: currentColor;
        border-radius: inherit;
        bottom: 0;
        color: inherit;
        content: "";
        left: 0;
        opacity: 0;
        pointer-events: none;
        position: absolute;
        right: 0;
        top: 0;
        transition: opacity .2s cubic-bezier(.4, 0, .6, 1);
    }
}

.game-item:hover {
    .card-hover {
        display: block;
        visibility: visible;
        opacity: 1;
    }
}

.card-save {
    position: absolute;
    z-index: 2;
    top: 0;
    right: 0;
    border-radius: 8px;
    transition: visibility .2s, opacity .2s linear;
    background: none !important;
    width: 30px;
    height: 30px;
    cursor: pointer;
}

.fav-img {
    width: 100%;
    height: 100%;
    -o-object-fit: contain;
    object-fit: contain;
    filter: grayscale(.9) drop-shadow(0 0 5px #000000);
    mix-blend-mode: screen !important;
    background: none !important;
    border-radius: 10px;
}

.faving-img {
    filter: unset !important;
}

.game-img {
    width: 100%;
    height: 100%;
    border-radius: 10px;
    -o-object-fit: fill;
    object-fit: fill;
    // filter: drop-shadow(3px 3px 3px #000);
}

.game-name {
    position: absolute;
    bottom: -7px;
    left: 50%;
    padding: 3px 0 !important;
    border-bottom-right-radius: 10px;
    border-bottom-left-radius: 10px;
    transform: translate(-50%, -50%);
    text-align: center;
    z-index: 3;
    background-color: rgba(0, 0, 0, .7);
    white-space: nowrap;
    width: 93%;
    overflow: hidden;
    text-overflow: ellipsis;
    font-family: Noto Sans Thai, Roboto !important;
    font-size: 14px !important;
    color: #fff;
    line-height: 1.5;
}

.profile-content {
    padding-right: 0.5rem;
    display: flex;
    flex-wrap: nowrap;

    .item-container {
        display: flex;

        .item {
            position: relative;
            font-weight: 700;
            font-size: 14px;
            border-radius: 30px;
            margin: 2px;
            padding: 5px 0.5rem 5px 1.5rem;
            margin-right: 0.5rem;
            color: #fff;

            .item-img {
                position: absolute;
                top: 50%;
                left: 5px;
                width: 25px;
                height: 25px;
                -o-object-fit: contain;
                object-fit: contain;
                transform: translate(-50%, -50%);
                filter: drop-shadow(2px 2px 2px rgba(0, 0, 0, .658));
            }

            .v-btn {
                background: none !important;
                box-shadow: none !important;
                width: 20px;
                height: 20px;
                font-size: .875rem;
                color: #fff;
                align-items: center;
                border: none;
                display: inline-flex;
                flex: 0 0 auto;
                font-weight: 500;
                letter-spacing: .0892857143em;
                justify-content: center;
                outline: 0;
                position: relative;
                text-decoration: none;
                text-indent: 0.0892857143em;
                text-transform: uppercase;
                transition-duration: .28s;
                transition-property: box-shadow, transform, opacity;
                transition-timing-function: cubic-bezier(.4, 0, .2, 1);
                -webkit-user-select: none;
                -moz-user-select: none;
                -ms-user-select: none;
                user-select: none;
                vertical-align: middle;
                white-space: nowrap;
            }

            .v-btn:before {
                background-color: currentColor;
                border-radius: inherit;
                bottom: 0;
                color: inherit;
                content: "";
                left: 0;
                opacity: 0;
                pointer-events: none;
                position: absolute;
                right: 0;
                top: 0;
                transition: opacity .2s cubic-bezier(.4, 0, .6, 1);
            }

            .v-btn:hover:before {
                opacity: .08;
            }

            .v-btn__content {
                align-items: center;
                color: inherit;
                display: flex;
                flex: 1 0 auto;
                justify-content: inherit;
                line-height: normal;
                position: relative;
                transition: inherit;
                transition-property: opacity;

                .v-icon {
                    color: #fff;
                    font-size: 18px;
                    height: 24px;
                    width: 24px;
                    align-items: center;
                    display: inline-flex;
                    font-feature-settings: "liga";
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
    }
}

.login-page,
.register-page,
.service-page,
.my-page {
    max-height: 90%;
    min-height: 50%;
    width: 100%;
    padding-bottom: 20px;
    background-color: #000;
}

.bind-page,
.deposit-page,
.withdraw-page {
    height: 100%;
    width: 100%;
}

.masking {
    position: fixed;
    top: 60px;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 998;
    height: -moz-calc(100vh - 130px);
    height: -webkit-calc(100vh - 130px);
    height: calc(100vh - 130px);
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}
.page-bottom {
    margin: 40px 0.6rem 0;
    border-top: 2px solid rgb(23,26,30);
    border-bottom: 2px solid rgb(23,26,30);
    padding: 14px 0;
    .cop-icon {
        display: flex;
        align-content:center;
        justify-content:space-between;
        padding: 0 40px;
        >img {
            height: 30px;
            width: 30px;
        }
    }
    .sup-icon {
        // display: flex;
        // align-content:center;
        // justify-content:space-between;
        box-sizing: border-box;
        padding: 20px 14px 0;
        text-align: left;
        >img {
            height: auto;
            width: 22%;
            margin-left: 3%;
        }
        >img:first-child {
            margin-left: 0 !important;
        }
    }
    .service-title {
        padding: 20px 18px 0;
        font-size: 14px;
        color: rgb(119,119,119);
        text-align: left;
    }
    .service-content {
        padding: 10px 18px 0;
        font-size: 14px;
        color: rgb(119,119,119);
        text-align: left;
        line-height: 1.3;
    }
}
.copyright {
    margin-top: 10px;
    color: rgb(119,119,119);
}
.game-cover {
    overflow-y: scroll;
    .list-game-bacarat {
        padding-top: 60px;
        .game-menu {
            position:fixed;
            top: 70px;
            left: 0;
            width: 100%;
            z-index: 3;
            background-color: #191138;
            box-sizing:border-box;
            padding: 27px 14px 7px;
            margin-top: 0;
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
        background-color: rgba(0,0,0,0.8);
        width: 80%;
        border-radius: 6px;
        padding: 0 26px;
        box-sizing: border-box;
        .close-title {
            text-align:center;
            color: #fff;
            font-size: 18px;
            padding: 16px 0 10px;
        }
        .close-content {
            border-radius: 6px;
            padding: 18px 12px;
            font-size: 14px;
            text-align:left;
            color: #fff;
        }
        .close-btn {
            display: flex;
            align-items:center;
            justify-content: space-between;
            .cancel, .confirm {
                border-radius:4px;
                height: 42px;
                line-height: 42px;
                width: 120px;
                font-size: 16px;
                text-align:center;
                margin: 18px 0;
                font-family: Arial;
                font-weight: 400;
                letter-spacing: 1.2px;
            }
            .cancel {
                background-color:rgb(204,75,25);
                color: #fff;
            }
            .confirm {
                background-color:rgb(58,140,37);
                color: #fff;
            }
        }
    }
}
/deep/ .mdi-earth::before {
    display: flex !important;
    align-items: center !important;
}

@-webkit-keyframes pulsepulse {
    0% {
        transform: scaleX(1)
    }

    50% {
        transform: scale3d(1.05, 1.05, 1.05)
    }

    to {
        transform: scaleX(1)
    }
}

@keyframes pulsepulse {
    0% {
        transform: scaleX(1)
    }

    50% {
        transform: scale3d(1.05, 1.05, 1.05)
    }

    to {
        transform: scaleX(1)
    }
}

@-webkit-keyframes x-bounceIn-data-v-5966ab82 {

    0%,
    20%,
    40%,
    60%,
    80%,
    to {
        transition-timing-function: cubic-bezier(.215, .61, .355, 1)
    }

    0% {
        transform: scale3d(.7, .7, .7)
    }

    20% {
        transform: scale3d(1.1, 1.1, 1.1)
    }

    40% {
        transform: scale3d(.9, .9, .9)
    }

    60% {
        opacity: 1;
        transform: scale3d(1.03, 1.03, 1.03)
    }

    80% {
        transform: scale3d(.97, .97, .97)
    }

    to {
        opacity: 1;
        transform: scaleX(1)
    }
}

@keyframes x-bounceIn-data-v-5966ab82 {

    0%,
    20%,
    40%,
    60%,
    80%,
    to {
        transition-timing-function: cubic-bezier(.215, .61, .355, 1)
    }

    0% {
        transform: scale3d(.7, .7, .7)
    }

    20% {
        transform: scale3d(1.1, 1.1, 1.1)
    }

    40% {
        transform: scale3d(.9, .9, .9)
    }

    60% {
        opacity: 1;
        transform: scale3d(1.03, 1.03, 1.03)
    }

    80% {
        transform: scale3d(.97, .97, .97)
    }

    to {
        opacity: 1;
        transform: scaleX(1)
    }
}
</style>
  