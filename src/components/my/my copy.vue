<template>
    <div class="main-page">
        <div class="page-title">
            ชวนเพื่อนแล้วรวย
            <a class="close" @click="close">
                <i aria-hidden="true" class="v-icon notranslate mdi mdi-close-circle theme--dark"></i>
            </a>
        </div>
        <div class="content">
            <div style="background: linear-gradient(rgb(0, 0, 0), rgb(13, 72, 5));border-radius: 8px;">
                <div style="padding: 0 12px;">
                    <div class="data">
                        <div class="data-item">
                            <div>จำนวนเพื่อนทั้งหมด</div>
                            <div class="data-num">0</div>
                        </div>
                        <div style="padding:12px;">
                            <hr role="separator" aria-orientation="vertical" color="#A9A9A9" class="v-divider v-divider--vertical theme--dark">
                        </div>
                        <div class="data-item">
                            <div>รายได้วันนี้</div>
                            <div class="data-num">0.00</div>
                        </div>
                    </div>
                    <div class="income">
                        <div class="income-item">
                            <div class="title">รายได้ทั้งหมด</div>
                            <div class="amount">0.00</div>
                            <div class="tips">ระบบจะคำนวณทุกเที่ยงคืน</div>
                        </div>
                        <div class="income-item">
                            <img :src="require('../../assets/wallet.png')" />
                        </div>
                    </div>
                </div>
                <div class="opt">
                    <div class="opt-item">
				        <input class="login-button button-color1 login-button-disable" type="button" value="ถอนเข้าเครดิต" />
                        <div class="tips">ถอนขั้นต่ำ 1 บาท</div>
                    </div>
                    <div class="opt-item">
				        <input class="login-button login-button-disable" type="button" value="ถอนเข้าบัญชี" />
                        <div class="tips">ปิดใช้งานระบบถอนเข้าบัญชี</div>
                    </div>
                </div>
            </div>
            <div class="info">ลิ้งแนะนำเพื่อน</div>
            <div class="info-tips">คุณจะได้รับรายได้ฟรีจากการแนะนำเพื่อน</div>
            <div class="s-box">
                <div class="link">
					<div class="login-info-item">
                        <div class="v-box">
                            <input class="v-input" type="text" value="https://www.google.com" />
                        </div>
                    </div>
                </div>
                <div class="button">
				    <input class="login-button" type="button" value="คัดลอก" />
                </div>
            </div>
        </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'my',
    data () {
      return {
        money: '',
        moneyfocus: false,
        tabIndex: 0
      }
    },
    computed: {
        loginClass() {
            return  {
                'v-label-active': this.moneyfocus
            }
        },
    },
    methods: {
        moneyFocus () {
            this.$refs.money.focus()
            this.moneyfocus = true
        },
        doWithdraw () {
            alert('success')
        },
        close () {
            this.$emit('close')
        },
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  .main-page {
    height: 100%;
    background-color: #000;
  }
  .login-page {
	height: 100%;
	width: 100%;
	background-color: #000;
}
.login-logo{
	padding-top: 2rem;
	text-align: center;
}
.login-logo-img {
	width: 250px;
	height: 250px;
}
.button-box {
    margin-top: 24px;
}
.login-info-item {
	text-align: left;
	background: hsla(0,0%,100%,.08);
	height: 40px;
	border-radius: 40px;
	position: relative;
	color: hsla(0,0%,100%,.7);
    display: flex;
    align-items: center;
}
.v-label {
	position: absolute;
	left: 20px;
	font-size: 16px;
	top: 17px;
	height: 20px;
    line-height: 20px;
    letter-spacing: normal;
	-webkit-animation:v-shake .6s cubic-bezier(.25,.8,.5,1);
	animation:v-shake .6s cubic-bezier(.25,.8,.5,1);
	transform-origin: top left;
}
.v-input {
	width: auto;
	color: #fff;
	background: transparent;
	padding: 0 20px;
	border: 0;
	font-size: 16px;
	height: 20px;
    line-height: 20px;
    letter-spacing: normal;
}
.login-info-item input:focus {
	outline:none;
}
.msg-box {
    min-height: 20px;
    overflow: hidden;
	margin: 0 40px 8px 40px;
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
.v-label-active {
	transform: translateY(-10px) scale(.75);
}
@-webkit-keyframes v-shake {
	59% {
		margin-left:0
	}
	60%,80% {
		margin-left:2px
	}
	70%,90% {
		margin-left:-2px
	}
}
@keyframes v-shake {
	59% {
		margin-left:0
	}
	60%,80% {
		margin-left:2px
	}
	70%,90% {
		margin-left:-2px
	}
}
.login-button {
    background: linear-gradient(180deg,#ffdf46,#ffdc42 23.96%,#feeb52 49.48%,#fbc316 71.87%,#f69b09);
    color: #212121!important;
    font-weight: 700;
    box-shadow: 0 4px 4px rgba(0,0,0,.25);
    height: 44px;
    padding: 0 19.5555555556px;
    width: 100%;
    font-size: 16px!important;
    letter-spacing: 0!important;
    border-radius: 27px!important;
    transition: all .2s cubic-bezier(.02,.54,.58,1);
    border: none;
    font-family: Noto Sans Thai;
    padding: 0 20px;
}
.button-color1 {
    background: linear-gradient(180deg,#8be0ff,#009dd8) !important;
}
.login-button-disable {
    background-color: hsla(0,0%,100%,.12)!important;
    text-shadow: 0 0 #000;
    opacity: .5!important;
    color: hsla(0,0%,100%,.3)!important;
}
  .page-title {
    padding: 1rem 0;
    font-weight: 500;
    font-size: 30px;
    color: #fff;
    position: relative;
    text-align: left;
    margin-left: 12px;

    .title-img {
        width: 30px;
        aspect-ratio: auto 30 / 30;
        height: 30px;
    }
    .close {
        position: absolute;
        top: 50%;
        bottom: 0;
        transform: translate(-50%,-50%);
        right: 0;
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
            transition: .3s cubic-bezier(.25,.8,.5,1),visibility 0s;
            vertical-align: middle;
            -webkit-user-select: none;
            -moz-user-select: none;
            -ms-user-select: none;
            user-select: none;
        }
    }
  }
  .v-tab:before {
    bottom: 0;
    content: "";
    left: 0;
    opacity: 0.24;
    pointer-events: none;
    position: absolute;
    right: 0;
    top: 0;
    transition: .3s cubic-bezier(.25,.8,.5,1);
}
  .page-tab {
    display: flex;
    justify-content: space-around;
    flex: 1 0 auto;
    position: relative;
    transition: .3s cubic-bezier(.25,.8,.5,1);
    white-space: nowrap;
    color: hsla(0,0%,100%,.6);
    .tab-item {
        align-items: center;
        cursor: pointer;
        display: flex;
        flex: 0 1 auto;
        font-size: .875rem;
        font-weight: 500;
        justify-content: center;
        letter-spacing: .0892857143em;
        line-height: normal;
        min-width: 90px;
        outline: none;
        padding: 0 16px;
        position: relative;
        text-align: center;
        text-decoration: none;
        text-transform: uppercase;
        transition: none;
        -webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
        white-space: normal;
        border-bottom: 1px solid #000;
        .tab-img {
            width: 30px;
            aspect-ratio: auto 30 / 30;
            height: 30px;
            margin-right: 8px!important;
        }
    }
  }
  .tab-active {
    color: #ffe66f!important;
    border-bottom: 1px solid #ffe66f !important;
  }
  .container {
    width: 100%;
    margin-top: 24px;
    margin-right: auto;
    margin-left: auto;
}
.alert-show {
    padding: 0.5rem;
    text-align: center;
    background: #948741;
    border: 1px solid #ffe247;
    box-sizing: border-box;
    border-radius: 3px;
    max-width: 360px;
    width: 100%;
    margin: auto;
    color: #fff;
}
  .profile-content {
        display: flex;
        flex-wrap: nowrap;
        align-items: center;
        justify-content: center;
        .item-container {
            display: flex;
            .item {
                color: #fff;
                position: relative;
                font-weight: 700;
                font-size: 18px;
                border-radius: 30px;
                margin: 2px;
                padding: 5px 0.5rem 5px 1.5rem;
                margin-right: 0.5rem;
                .item-img {
                    position: absolute;
                    top: 50%;
                    left: 5px;
                    width: 30px;
                    height: 30px;
                    -o-object-fit: contain;
                    object-fit: contain;
                    transform: translate(-50%,-50%);
                    filter: drop-shadow(2px 2px 2px rgba(0,0,0,.658));
                }
                .v-btn {
                    background: none!important;
                    box-shadow: none!important;
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
                    transition-property: box-shadow,transform,opacity;
                    transition-timing-function: cubic-bezier(.4,0,.2,1);
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
                    transition: opacity .2s cubic-bezier(.4,0,.6,1);
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
                        transition: .3s cubic-bezier(.25,.8,.5,1),visibility 0s;
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
.dialog-deposit-select {
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-start;
    max-width: 432px;
    margin: auto;
    .item-select {
        text-align: center;
        max-width: 200px;
        width: 45%;
        margin: 0.5rem;
        padding: 5px;
        background: linear-gradient(180deg,#38d851,#0f6a00);
        box-sizing: border-box;
        border-radius: 8px;
        >a {
            display: flex;
            justify-content: flex-start;
            color: #fff;
            cursor: pointer;
            >img {
                width: 40px;
                height: 40px;
                margin: 0;
            }
            >span {
                padding-left: 8px!important;
                margin: auto 0;
            }
        }
    }
}
.content {
    margin-top: 40px;
    padding: 0 24px 20px;
    background-color: #000;
    .data {
        width: 100%;
        background-color: white;
        border-radius: 8px;
        display: flex;
        .data-item {
            width: 41%;
            padding: 12px;
            .data-num {
                font-weight: bolder;
            }
        }
        .v-divider--vertical {
            align-self: stretch;
            border: solid;
            border-width: 0 thin 0 0;
            display: inline-flex;
            height: inherit;
            min-height: 100%;
            max-height: 100%;
            max-width: 0;
            width: 0;
            vertical-align: text-bottom;
            margin: 0 -1px;
            border-color: hsla(0,0%,100%,.12);
        }
    }
    .income {
        margin-top: 24px;
        display: flex;
        align-items: center;
        justify-content: space-around;
        .income-item {
            color: white;
            .title {
                font-weight: 500;
                font-size: 16px;
            }
            .amount {
                color: #ffeb3b!important;
                caret-color: #ffeb3b!important;
                font-weight: 500;
                font-size: 28px;
            }
            >img {
                height: 110px;
                width: 110px;
            }
        }
    }
    .opt {
        margin-top: 12px;
        display: flex;
        align-items: center;
        justify-content: space-around;
        box-sizing: border-box;
        .opt-item {
            width: 50%;
            padding: 12px;
            .tips {
                color: #fff;
                font-size: 12px;
            }
        }
    }
    .info {
        margin-top: 8px;
        padding-left: 12px;
        color: hsla(0,0%,100%,.7);
        font-weight: bold;
        font-size: 16px;
        text-align: left;
    }
    .info-tips {
        margin-top: 4px;
        padding-left: 12px;
        color: hsla(0,0%,100%,.7);
        font-size: 12px;
        text-align: left;
    }
    .s-box {
        margin-top: 20px;
        display: flex;
        padding: 0 20px;
        box-sizing: border-box;
        .link {
            width: 75%;
        }
        .button {
            margin-left: 24px;
            width: 25%;
            >input {
                width: auto !important;
                padding: 0 !important;
                height: 36px !important;
                min-width: 64px;
            }
        }
    }
}
  </style>
  