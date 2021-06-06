<template>
    <div class="my">
        <div class="mdui-card">
            <div class="mdui-card-primary">
                <div class="mdui-card-primary-title">个人信息</div>
            </div>
            <div class="mdui-card-media">
                <img class="image" src="card.webp"/>
            </div>
            <div class="mdui-card-content">
                <div class="mdui-typo-body-2 item">昵称：<div class="mdui-chip"><span class="mdui-chip-icon mdui-color-blue"><i class="mdui-icon material-icons">face</i></span>
                <span class="mdui-chip-title">{{nick}}</span>
                </div></div>
                <div class="mdui-typo-body-2 item">状态：<div class="mdui-chip">
                <span :class="nowStatusClass"><i class="mdui-icon material-icons">check_circle</i></span>
                <span class="mdui-chip-title">{{nowStatusStr}}</span>
                </div></div>
                <div class="mdui-typo-body-2 item">节点：<div class="mdui-chip">
                    <span class="mdui-chip-icon mdui-color-blue"><i class="mdui-icon material-icons">check_circle</i></span>
                    <span class="mdui-chip-title">{{nodeName}}</span></div></div>
                <div class="mdui-typo-body-2 item">上次检测：{{timestamp}}</div>
            </div>
            <div class="mdui-card-actions">
                <button @click="logout" class="mdui-btn mdui-ripple">退出登录</button>
                <button @click="del" class="mdui-btn mdui-ripple mdui-color-red">删除账号</button>
            </div>
        </div>

        <div class="mdui-dialog" id="delete">
        <div class="mdui-dialog-title">删除</div>
        <div class="mdui-dialog-content">确认从系统中移除你的cookie吗？</div>
        <div class="mdui-dialog-actions">
        <button @click="checkDel" class="mdui-btn mdui-ripple" mdui-dialog-confirm>确认</button>
        <button class="mdui-btn mdui-ripple" mdui-dialog-confirm>取消</button>
        </div>
    </div>
</div>
</template>

<script>
import axios from 'axios'
import mdui from 'mdui'
export default {
    name:"My",
    data:function(){
        return {
            baseUrl: "",
            nick: "",
            status: 1,
            nodeName: "",
            timestamp:"",
            cid: ""
        }
    },
    methods:{
        getInfo(){
            this.baseUrl= localStorage.getItem("baseUrl")
            console.log(this.baseUrl)
            var self= this
            this.cid= localStorage.getItem("cid")
            //获取账号信息
            axios.post(this.baseUrl+"/checkcookie",{cid:localStorage.getItem("cid")}).then(function(res){
                self.nick= res.data.data.nickname
                self.status= res.data.data.status
                self.timestamp=res.data.data.timestamp
                self.nodeName=localStorage.getItem("nodeName")
            }).catch(function(res){
                mdui.snackbar("服务器连接失败！")
                console.log(res)
            })
        },
        logout(){
            localStorage.removeItem("cid")
            localStorage.removeItem("nodeName")
            localStorage.removeItem("baseUrl")
            this.$router.replace("/")
        },
        del(){
            var instD = new mdui.Dialog('#delete');
            instD.open()
        },
        checkDel(){
            var self= this
            axios.post(this.baseUrl + "/delete",{
                cid : this.cid,
            }).then(function(res){
                mdui.snackbar(res.data.data)
                self.logout()
            })
        }

    },
    mounted:function(){
        this.getInfo()
    },
    computed:{
        nowStatusStr(){
            if(this.status==1){
                return "正常"
            }else{
                return "已失效"
            }
        },
        nowStatusClass(){
            if(this.status==1){
                return ["mdui-chip-icon", "mdui-color-blue"]
            }else{
                return ["mdui-chip-icon", "mdui-color-red"]
            } 
        }
    }
}
</script>

<style scoped>
.my{
    margin-top: 20px;
}
.item{
    margin-top: 10px;
}
</style>