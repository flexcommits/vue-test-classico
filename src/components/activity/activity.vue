<template>
        <div class="main-page">
            <div class="activity-box" v-for="(item, index) in list" :key="index" @click="toDetail(item.id)">
                <img :src="item.img" />
                <div class="title">{{ item.name }}</div>
                <div class="more">{{ GLOBAL.lanLocal['clickmore'] }}</div>
            </div>
            <div class="empty" :style="{height: innerHeight+'px'}" v-if="isEmpty">
                <div>
                    <img :src="require('../../assets/icon/empty.png')" />
                    <div class="tips">{{GLOBAL.lanLocal['noactive']}}</div>
                </div>
            </div>
        </div>
</template>
  
<script>
import { doPost } from '../../api/api.js'
import decodeApiLan from '../../api/decodeApiLan.js'
export default {
    name: 'activity',
    data() {
        return {
            id: '',
            isDetail: false,
            innerHeight: 0,
            isEmpty: false,
            list: []  //活动列表
        }
    },
    created() {
        this.innerHeight = window.innerHeight - 130
        doPost({
            n: 'AppEx',
            a: 'get_activity_list',
            accountid: this.GLOBAL.userInfo.accountid,
        }).then((res) => {
            const data = decodeApiLan(res, this.GLOBAL.lanArr) //语言包解析匹配
            let list = data.data.list
            if(list)
            {
                list.forEach(item => {
                    let suffix = item.img.slice(-4)
                    item.img = item.img.replace(suffix, "_"+this.GLOBAL.lanCode+suffix)
                    let names = item.name.split("||")
                    names.forEach(name => {
                        let arr = name.split("|")
                        if(arr[0] == this.GLOBAL.lanCode)
                        {
                            item.name = arr[1]
                        }
                    });
                });
                this.list = list
            }else {
                this.isEmpty = true
            }
        })
    },
    computed: {

    },
    mounted() {
    },
    methods: {
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
    padding: 60px 10px 80px;
    box-sizing: border-box;

    .activity-box:first-child {
        margin-top: 12px;
    }

    .activity-box {
        border-radius: 10px;
        background-color: hsla(0,0%,100%,.1);
        margin-bottom: 12px;
        box-sizing: border-box;

        img {
            padding: 6px 6px 0px;
            box-sizing: border-box;
            width: 100%;
            height: auto;
        border-radius: 14px;
        }

        .title {
            padding: 4px 8px;
            font-size: 16px;
            color: white;
            text-align: left;
        }

        .more {
            padding: 0 8px 8px;
            font-size: 14px;
            color: #999;
            text-align: left;
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
  