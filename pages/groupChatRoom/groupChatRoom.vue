<template>
<chat id="groupchat" ref="chat" :username="username" :groupId="groupId" chatType="chatRoom"></chat>
</template>

<script>
let disp = require("../../utils/broadcast");
import chat from "../../comps/chat/chat";

export default {
  data() {
    return {
      username: {
        your: ""
      }
    };
  },

  components: {
    chat
  },

   props: {
    groupId:{
      type:String,
      default:''
    }
  },

  // options = 系统传入的 url 参数
  onLoad(options) {
    let username = JSON.parse(options.username);
    this.setData({
      username: username
    });
    uni.setNavigationBarTitle({
      title: username.your
    });
  },

  onUnload() {
    disp.fire("em.chatroom.leave");
  },

  onPullDownRefresh: function () {
    uni.showNavigationBarLoading();
    // this.selectComponent('#groupchat').$vm.getMore(); 
    // 停止下拉动作
    this.$refs.chat.getMore();
    uni.hideNavigationBarLoading();
    uni.stopPullDownRefresh();
  },
  methods: {}
};
</script>
<style>
@import "./groupChatRoom.css";
</style>