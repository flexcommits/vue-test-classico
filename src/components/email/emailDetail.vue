<template>
    <div>
        <div class="main-page">
            <div class="title">{{ detail.Title }}</div>
            <div class="content">{{ detail.Content }}</div>
            <div class="button" @click="del">{{ GLOBAL.lanLocal['delete'] }}</div>
        </div>
    </div>
</template>
  
<script>
import { doPost } from '../../api/api.js'
import decodeApiLan from '../../api/decodeApiLan.js'
export default {
    name: 'emailDetail',
    data() {
        return {
            innerHeight: 0,
            detail: {}  //邮件详情
        }
    },
    props: {
        id: {
            type: Number,
            default: 0
        }
    },
    created() {
        this.innerHeight = window.innerHeight - 130
        if (this.id != 0) {
            doPost({
                n: 'AppEx',
                a: 'get_mail_detail',
                accountid: this.GLOBAL.userInfo.accountid,  //测试时用户ID先传1291966  this.GLOBAL.userInfo.accountid
                mailid: this.id,
            }).then((res) => {
                const data = decodeApiLan(res, this.GLOBAL.lanArr) //语言包解析匹配
                this.detail = data.data.info
                this.$emit("in")
            })
        }
    },
    computed: {

    },
    mounted() {
    },
    methods: {
        del() {
            doPost({
                n: 'AppEx',
                a: 'del_mail_detail',
                accountid: this.GLOBAL.userInfo.accountid,  //测试时用户ID先传1291966  this.GLOBAL.userInfo.accountid
                mailid: this.id,
            }).then((res) => {
                this.$toast.success(this.GLOBAL.lanLocal['deletesuccess'], {
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
                this.$emit("close")
            })
        }
    }
}
</script>
  
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
.main-page {
    height: 100%;
    width: 100%;
    padding: 60px 12px 80px;
    box-sizing: border-box;
    text-align: left;

    .title {
        color: #fff;
        font-size: 17px;
    }

    .content {
        margin-top: 12px;
        color: #a6a6a6;
        font-size: 15px;
    }

    .button {
        background-color: rgb(255, 181, 0);
        border-radius: 6px;
        height: 40px;
        width: 120px;
        display: flex;
        align-items: center;
        justify-content: center;
        color: #000;
        font-size: 16px;
        font-weight: 500;
        margin: 40px auto 0;
    }
}
</style>
  