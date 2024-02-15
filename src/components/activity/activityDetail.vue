<template>
    <div>
        <div class="main-page">
            <img :src="detail.img" />
            <div class="title">{{ detail.name }}</div>
            <div class="content" v-html="detail.content"></div>
        </div>
    </div>
</template>
  
<script>
import { doPost } from '../../api/api.js'
import decodeApiLan from '../../api/decodeApiLan.js'
export default {
    name: 'activityDetail',
    data() {
        return {
            detail: [],  //活动列表
            innerHeight: 0,
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
        if(this.id != 0)
        {
            doPost({
            n: 'AppEx',
            a: 'get_activity_detail',
            accountid: this.GLOBAL.userInfo.accountid,
            ActivityID: this.id
        }).then((res) => {
            const data = decodeApiLan(res, this.GLOBAL.lanArr) //语言包解析匹配
            let detail = data.data.info
            let suffix = detail.img.slice(-4)
            detail.img = detail.img.replace(suffix, "_"+this.GLOBAL.lanCode+suffix)
                let names = detail.name.split("||")
                names.forEach(name => {
                    let arr = name.split("|")
                    if(arr[0] == this.GLOBAL.lanCode)
                    {
                        detail.name = arr[1]
                    }
                });
                let contents = detail.content.split("||")
                contents.forEach(content => {
                    let arr = content.split("|")
                    let code = arr[0].replaceAll("</p><p>", "").replaceAll("<p>", "")
                    if(code == this.GLOBAL.lanCode)
                    {
                        let content = "<p>" + arr[1] + "</p>"
                        detail.content = content
                    }
                });
                this.detail = detail
        })
        }
        
    },
    computed: {

    },
    mounted() {
    },
    methods: {
    }
}
</script>
  
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>
.main-page {
    height: 100%;
    width: 100%;
    padding: 60px 10px 100px;
    box-sizing: border-box;

    img {
        width: 100%;
        height: auto;
        border-radius: 14px;
    }

    .title {
        font-size: 16px;
        color: white;
        padding: 14px 0 8px;
    }

    .content {
        font-size: 14px;
        color: #a6a6a6;
    }
}
</style>
  