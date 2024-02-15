<template>
    <div>
        <div class="main-page">
            <div class="email-box" v-for="(item, index) in list" :key="index + 1" @click="toDetail(item.MailID)">
                <div class="top">
                    <div class="title">{{ item.Title }}</div>
                    <div class="unread" v-if="item.IsRead == 0">{{ GLOBAL.lanLocal['unread'] }}</div>
                </div>
                <div class="content">
                    {{ item.Content }}
                </div>
                <div class="desc">
                    <div>{{ GLOBAL.lanLocal['validityperiod'] }}: {{ item.Offset }} {{ GLOBAL.lanLocal['days'] }}</div>
                </div>
                <div class="detail">
                    <div>{{ GLOBAL.lanLocal['emaildetails'] }}</div>
                    <img :src="require('../../assets/icon/arrow.png')" />
                </div>
            </div>
            <div class="empty" :style="{height: innerHeight+'px'}" v-if="isEmpty">
                <div>
                    <img :src="require('../../assets/icon/empty.png')" />
                    <div class="tips">{{GLOBAL.lanLocal['noemail']}}</div>
                </div>
            </div>
        </div>
    </div>
</template>
  
<script>
import { doPost } from '../../api/api.js'
import decodeApiLan from '../../api/decodeApiLan.js'
export default {
    name: 'email',
    data() {
        return {
            innerHeight: 0,
            isEmpty: false,
            list: []  //邮箱列表
        }
    },
    created() {
        this.innerHeight = window.innerHeight - 130
        this.getDetail()
    },
    computed: {

    },
    mounted() {
    },
    methods: {
        getDetail() {
            doPost({
                n: 'AppEx',
                a: 'get_mail_list',
                accountid: this.GLOBAL.userInfo.accountid,  //测试时用户ID先传1291966  this.GLOBAL.userInfo.accountid
            }).then((res) => {
                const data = decodeApiLan(res, this.GLOBAL.lanArr) //语言包解析匹配
                this.list = data.data.list
                if(!this.list || this.list.length <= 0)
                {
                    this.isEmpty = true
                }
            })
        },
        toDetail(id) {
            this.$emit("detail", id)
        }
    }
}
</script>
  
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
.main-page {
    height: 100%;
    width: 100%;
    padding: 70px 12px 80px;
    box-sizing: border-box;
    text-align: left;

    .email-box {
        background-color: #2f2063;
        margin-bottom: 16px;

        .top {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 20px 12px;

            .title {
                color: #fff;
                font-size: 16px;
                flex: 1;
            }

            .unread {
                font-size: 10px;
                color: rgb(255, 182, 0);
                padding: 3px 8px;
                border: 1px solid rgb(255, 182, 0);
                border-radius: 16px;
            }
        }

        .content {
            padding: 0 12px;
            color: #a6a6a6;
            font-size: 14px;
            overflow: hidden;
            -webkit-line-clamp: 2;
            text-overflow: ellipsis;
            display: -webkit-box;
            -webkit-box-orient: vertical;
        }

        .desc {
            padding: 20px 12px;
            text-align: right;
            font-size: 12px;
            color: #999;
            padding-bottom: 18px;
            border-bottom: 1px solid rgb(38, 41, 45);
        }

        .detail {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 18px 12px;
            color: #dcdcdc;
            font-size: 14px;

            img {
                height: 21px;
                width: 12px;
            }
        }
    }
}
.empty {
    width:100%;
    display: flex;
    align-items: center;
    justify-content: center;
    img {
        height: 100px;
        width: auto;
    }
    .tips {
        text-align:center;
        font-size: 16px;
        color: #efcf7f;
    }
}
</style>
  