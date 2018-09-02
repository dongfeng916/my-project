<template>
    <div class="container">
        <CommentList type='user' :comments='comments'></CommentList> 
        <!-- type仅仅是自定义user类型而已 -->
    </div>
</template>
<script>
import {get} from '@/util'
import CommentList from '@/components/CommentList'
export default {
    data(){
        return{
            comments:[],
            userinfo:{}
        }
    },
    components:{
        CommentList
    },
    methods:{
        init(){
            wx.showNavigationBarLoading()
            this.getComments()
        },
        async getComments(){
            const comments = await get('/weapp/commentlist',{
                openid:this.userinfo.openId
            })
            this.comments = comments.list
        }
    },
    onShow(){
        if(!this.userinfo.openId){
            let userinfo = wx.getStorageSync('userinfo')
            if(userinfo){
                this.userinfo = userinfo
                this.init()
            }
        }
    }
}
</script>
<style>
</style>
