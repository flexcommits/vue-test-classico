<template>
    <div class="main-page">
        <div class="header">
            <img src="../../assets/login/back.png" @click="close" />
            {{GLOBAL.lanLocal['vipbenefits']}}
        </div>
        <div class="content">
            <div class="vip-pross">
                <div class="my-vip">
                    <img src="../../assets/vip/vip_icon.png" />
                    <div class="vip-num">{{myVip}}</div>
                </div>
                <div class="pross-box">
                    <div class="pross-title">
                        <div class="hd">{{GLOBAL.lanLocal['topup']}}</div>
                        <div class="ft"><div class="pross-num">{{rechargeAmount}}/{{nextRechargeAmount}}</div></div>
                    </div>
                    <div class="pross-item">
                        <div :style="{width: rechargePross + '%'}" class="pross-bg"></div>
                    </div>
                    <div class="pross-title" style="margin-top: 5px;">
                        <div class="hd">{{GLOBAL.lanLocal['bet']}}</div>
                        <div class="ft"><div class="pross-num">{{scoreAmount}}/{{nextScoreAmount}}</div></div>
                    </div>
                    <div class="pross-item">
                        <div style="background-color: rgb(48,178,58);" :style="{width: scorePross + '%'}" class="pross-bg"></div>
                    </div>
                </div>
                <div class="next-vip">
                    <img src="../../assets/vip/vip_icon.png" />
                    <div class="vip-num">{{nextVip}}</div>
                </div>
            </div>
            <!-- <div class="condition-title">{{GLOBAL.lanLocal['conditiontitle']}}</div>
            <div class="condition-text">1.{{GLOBAL.lanLocal['topup']}} {{GLOBAL.currencySymbol}} {{nextVipDetail['Recharge']}}</div>
            <div class="condition-text">2.{{GLOBAL.lanLocal['bet']}} {{GLOBAL.currencySymbol}} {{nextVipDetail['Score']}}</div>
            <div class="condition-text">{{GLOBAL.lanLocal['upgradereminder']}}</div> -->
            <div class="update-btn" @click="toDeposit">{{GLOBAL.lanLocal['toupdate']}}</div>
            <div class="benefits-title">{{GLOBAL.lanLocal['vipbenefits']}}</div>
            <div class="benefits-list">
                <div class="benefits-item" :class="vipId == i ? 'active' : ''" :key="i+1" @click="changeVip(i)" v-for="(item,i) in vipList">
                    <img v-if="vipId == i" src="../../assets/vip/vip_bg_active.png" />
                    <img v-else src="../../assets/vip/vip_bg.png" />
                    <div>V{{item.LevelID}}</div>
                </div>
            </div>
            <div class="amount-detail">
                <div class="detail-item">
                    <img src="../../assets/vip/draw.png" />
                    <div>
                        <div class="detail-title">{{GLOBAL.lanLocal['totaldeposit']}}</div>
                        <div class="detail-content">{{vipDetail['Recharge']}} {{GLOBAL.currency}}</div>
                    </div>
                </div>
                <div class="detail-item" style="justify-content:center;">
                    <img src="../../assets/vip/score.png" />
                    <div>
                        <div class="detail-title">{{GLOBAL.lanLocal['totalbet']}}</div>
                        <div class="detail-content">{{vipDetail['Score']}} {{GLOBAL.currency}}</div>
                    </div>
                </div>
            </div>
            <div class="condition-title">{{GLOBAL.lanLocal['withdrawalpermissions']}}</div>
            <div class="condition-text">{{GLOBAL.lanLocal['fee']}}: {{vipDetail['DrawRate']}}</div>
            <div class="condition-text">{{GLOBAL.lanLocal['withdrawallimit']}}: {{vipDetail['Draw']}} {{GLOBAL.currency}}({{GLOBAL.lanLocal['perrequest']}})</div>
            <div class="condition-text">{{GLOBAL.lanLocal['numberofwithdrawals']}}: {{vipDetail['DrawNum']}} {{GLOBAL.lanLocal['timeday']}}</div>
            <div class="condition-text" style="color: #ffd045" v-if="vipDetail['Bonus'] > 0">{{GLOBAL.lanLocal['promotionalbonus']}}: {{vipDetail['Bonus']}} {{GLOBAL.currency}}</div>
            
            <div class="condition-title" style="margin-top: 30px;">{{GLOBAL.lanLocal['viprules']}}</div>
            <div class="condition-text">{{GLOBAL.lanLocal['rule1']}}</div>
            <div class="condition-text">{{GLOBAL.lanLocal['rule2']}}</div>
            <div class="condition-text">{{rule3}}</div>
        </div>
    </div>
</template>
  
<script>
import md5 from 'js-md5';
import AES from "../../common/AES.js"
import { doPost } from '../../api/api.js'
export default {
    name: 'vip',
    data() {
        return {
            innerHeight: 0,
            vipList: [],
            scoreAmount: '0.00',
            rechargeAmount: '0.00',
            nextScoreAmount: '0.00',
            nextRechargeAmount: '0.00',
            myVip: '',
            nextVip: '',
            vipId: 0,
            vipDetail: [],
            nextVipDetail: [],
            scorePross: 0,
            rechargePross: 0,
            rule3: '',
        }
    },
    watch: {
    },
    props: {
    },
    computed: {
    },
    created() {
        this.innerHeight = window.innerHeight - 70
        this.getVipConfig()
        this.getVipDays()
    },
    methods: {
        getVipDays() {
            doPost({
                    n: 'AppEx',
                    a: 'get_vip_days',
                }).then((res) => {
                    let rule3 = this.GLOBAL.lanLocal['rule3']
                    let vipdays = res.data
                    this.rule3 = rule3.replace("%d", vipdays)
                })
        },
        close() {
            this.$emit('close')
        },
        toDeposit() {
            this.$emit('toDeposit')
        },
        changeVip(i) {
            if(i != this.vipId)
            {
                this.vipId = i
                this.vipDetail = this.vipList[i]
            }
        },
        getVipConfig() {
            doPost({
                n: 'AppEx',
                a: 'get_vip_config',
                lan: this.GLOBAL.lanCode
            }).then((res) => {
                if (res.code === 0) {
                    this.vipList = res.data
                    this.vipDetail = this.vipList[0]
                    this.getUserVip()
                }
            })
        },
        getUserVip() {
            doPost({
                n: 'AppEx',
                a: 'get_user_vip',
                accountid: this.GLOBAL.userInfo.accountid
            }).then((res) => {
                if (res.code === 0) {
                    let vip = Number(res.data.level)
                    let nextLevel = res.data.nextLevel
                    this.scoreAmount = Number(res.data.score).toFixed(2)
                    this.rechargeAmount = Number(res.data.charge).toFixed(2)
                    this.myVip = this.GLOBAL.lanLocal['vip'] + vip
                    if(vip < this.vipList.length)
                    {
                        this.nextVip = this.GLOBAL.lanLocal['vip'] + nextLevel.LevelID
                        this.nextVipDetail = nextLevel
                        this.nextScoreAmount = Number(this.nextVipDetail.Score).toFixed(2)
                        this.nextRechargeAmount = Number(this.nextVipDetail.Recharge).toFixed(2)
                        this.scorePross = this.scoreAmount / this.nextScoreAmount * 100
                        this.rechargePross = this.rechargeAmount / this.nextRechargeAmount * 100
                    }
                }
            })
        },
    }
}
</script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>

.main-page {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 4;
    height: 100%;
    width: 100%;
    text-align: left;
    overflow-y: scroll;
}
.header {
    position: fixed;
    top: 0;
    left: 0;
    z-index: 4;
    height: 70px;
    line-height: 70px;
    width: 100%;
    text-align: center;
    background-color: #0f0e0e;
    color: #fff;
    font-size: 20px;
    text-align: center;
    >img {
        height: 30px;
        width: auto;
        position: absolute;
        top: 22px;
        left: 22px;
    }
}
.content {
    padding: 88px 12px;
    .vip-pross {
        display: flex;
        align-items: flex-end;
        justify-content: space-around;
        box-sizing: border-box;
        .my-vip, .next-vip {
            width: 50px;
            position: relative;
            text-align: center;
            display: flex;
            align-items: flex-end;
            justify-content: center;
            font-size: 0;
            >img {
                    width: 50px;
                    height: auto;
                }
                .vip-num {
                    color: rgb(185, 159, 122);
                    font-size: 11px;
                    font-weight: 600;
                    position: absolute;
                    bottom: -2px;
                    left: 0;
                    width: 100%;
                    text-align : center;
                }
        }
        .pross-box {
            flex: 1;
            width: 100%;
            margin: 0 10px;
            .pross-title {
                color: #e9b375;
                font-size:11px;
                display:flex;
                align-items:flex-end;
                justify-content: space-between;
                padding: 0 8px 0 4px;
            }
            .pross-item {
                height: 8px;
                width: 100%;
                background-color: rgb(46,32,99);
                border-radius: 6px;
                position: relative;
            }
            .pross-bg {
                position: absolute;
                left: 0;
                top: 0;
                z-index: 1;
                height:8px;
                border-radius: 8px;
                background-color: rgb(249, 183, 0);
            }
            .pross-num {
                font-size: 10px;
                color: #e9b375;
                text-align: center;
            }
        }
    }
    .condition-title {
        margin-top: 16px;
        text-align: left;
        font-size: 16px;
        color: rgb(109,77,240);
    }
    .condition-text {
        margin-top: 6px;
        color: rgb(196,199,206);
        font-size: 14px;
    }
    .update-btn {
        margin: 30px 12px;
        background-color: rgb(249,233,58);
        color: #333;
        height: 40px;
        line-height: 40px;
        border-radius: 4px;
        font-size: 16px;
        font-weight: 600;
        text-align: center;
    }
    .benefits-title {
        text-align: left;
        font-size: 16px;
        color: rgb(109,77,240);
    }
    .benefits-list {
        margin-top: 16px;
        height: 60px;
        display: flex;
        align-items:center;
        flex-wrap: nowrap;
        overflow-x: scroll;
        overflow-y: hidden;
        .active {
            height: 65px !important;
            >img {
                height: 65px !important;
            }
            >div {
                height: 65px !important;
                line-height: 65px !important;
            }
        }
        .benefits-item {
            height: 50px;
            position: relative;
            margin-right: 18px;
            >img {
                height: 50px;
                width: auto;
            }
            >div {
                position: absolute;
                left: 0;
                top: 0;
                font-size: 20px;
                font-weight: 700;
                color: rgb(100, 49,6);
                height: 50px;
                line-height: 50px;
                width: 100%;
                text-align:center;
            }
        }
        
    }
    .benefits-list::-webkit-scrollbar { display: none; }
    .amount-detail {
        margin-top: 16px;
        display: flex;
        align-items: center;
        .detail-item {
            width: 50%;
            display: flex;
            align-items: center;
            box-sizing: border-box;
            >img {
                height: 40px;
                width: auto;
            }
            >div{
                margin-left: 10px;
                .detail-title {
                    font-size: 14px;
                    color: #f6f6f6;
                }
                .detail-content {
                    font-size: 14px;
                    color: #ffd045;
                }
            }
        }
    }
}
</style>
  