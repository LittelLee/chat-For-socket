<!--
  存在的问题是如何完美的实现在线人数的管理
-->
<template>

  <div id="app">
    <div class="container">
      <div class="row clearfix">
        <div class="col-xs-12 bordered">
          <div class="messageArea">
              <div class="rowMessage"  v-for="tempMessage in MessageAll" :key="tempMessage.index">
                <div class="username">{{tempMessage.userName}}:</div>
                <div :class="tempMessage.className">{{tempMessage.information}}</div>
              </div>
              <div class="onlinePerson md-icon-button" @click="showOnlineperson">
                <md-avatar class="md-avatar md-primary md-small"></md-avatar>
              </div>
          </div>
          <div class="functionalArea ">
            <div class="input-group">
              <input type="text" @keyup.enter="send" v-model="msg" class="form-control" required="required" placeholder="Typing word......">
              <span class="input-group-addon btn" @click="send"  >
                <div class="sendIcon" ></div>
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Dialog  -->
    <!--

      md-active.sync="false"
    -->
    <md-dialog-prompt
      :md-active.sync="active"
      v-model="username"
      md-title="What is your name?"
      md-input-maxlength="30"
      md-input-placeholder="Type your name......"
      md-confirm-text="OK"
      @md-confirm="registerName"/>

    <!--Dialog --2 -->
    <md-dialog :md-active.sync="showStatus" class="md-scrollbar extendStyle">
      <md-dialog-title>Online Friends</md-dialog-title>

      <div class="internalHeight" >
        <div v-for="tempPerson in friendSet">
        <md-card class="hoverStyle" >
          <md-card-content class="fontStyle">{{tempPerson.name}}</md-card-content>
        </md-card>
        </div>
      </div>
      <md-dialog-actions>
        <md-button class="md-primary" @click="showStatus = false">Close</md-button>
      </md-dialog-actions>
    </md-dialog>

  </div>
</template>

<script>
export default {
  data () {
    return {
        socket : '',
        msg: '',
        MessageAll : [],
        active : true,
        username : '',
        showStatus : false,
        friendSet : []
    }
  },
  beforeMount : function () {
    this.active=true;
    this.connectionServer();
  },
  mounted : function () {
    var $this = this;
    this.socket.on('chatMessage',function (msg) {
      $this.MessageAll.push(
        {
          className:'otherMessage',userName:msg.username,information:msg.msg
        }
      );
    });
    this.socket.on('newFriend',function (newFriend) {
        alert(newFriend + ' has joined');
        $this.friendSet.push({name : newFriend});
    });
  },
  methods : {
    send : function () {
      if(this.msg !== '') {
        this.MessageAll.push({className : 'selfMessage',userName:this.username,information : this.msg});
        this.socket.emit('msg',this.msg);
        this.msg = '';
      }else if(this.username.length === 0 || this.username === '') {
        this.active = true;
      }else if(this.msg.length ===0 || this.msg === ''){
        alert('消息不能为空');
      }
    },
    connectionServer : function(){
      this.socket = io("localhost:3000");
    },
    registerName : function () {
      if(this.username.length != 0 || this.username != null){
        this.socket.emit("username",this.username);
      }
    },
    showOnlineperson : function () {
      this.showStatus = true;
    }
  }
}
</script>

<style lang="less" scoped>
  @messageAreaHeight : 484px;
  .bordered {
    border: 0px solid black;
    height: 518px;
    display: flex;
    flex-flow: column;
    padding: 0px 5px !important;
    padding-top: 5px !important;

  }
  .messageArea {
    width: 100%;
    height: @messageAreaHeight;
    background-color: #e9f0e1;
    box-shadow: #0f0f0f 0px 0px 10px;
    display: flex;
    flex-flow: column;
    overflow: auto;

  }
  .functionalArea {
    width: 100%;
    height: auto;
    background-color: #2b542c;
    box-shadow: #0f0f0f 0px 0px 10px;
    margin-top: 2px;
  }
  .sendIcon {
    width: 35px;
    height: 20px;
    background: url("./assets/send.svg") no-repeat center;
    background-size: contain;
  }
  .rowMessage {
    width: auto;
    height: auto;
    padding: 5px 5px;
  }
  .otherMessage,.selfMessage {
    width: auto;
    height: auto;
    max-width: 208px;
    line-height: 20px;
    font-size: 1.1em;
    -webkit-border-radius: 25px;
    -moz-border-radius: 25px;
    border-radius: 25px;
    word-break: break-all;
    box-shadow: #353e3b 0px 0px 15px;
    padding: 5px 10px;
    margin: 10px 0px 0px 10px;
    display: inline-block;
  }
  .otherMessage {
    background-color: #77978e;
    color: white;
  }
  .selfMessage {
    background-color: #339999;
    color: #FFFFCC;
  }
  .username {
    font-size: 18px;
    display: inline-block;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
  }
  .onlinePerson {
    width: 45px;
    height:40px;
    position: absolute;
    right: 0;
    top: @messageAreaHeight/4;
    border-radius: 100px 0px 0px 100px;
    box-shadow: #77978e 0px 0px 10px;
    padding: 8px 0px 0px 10px;
    transition: all ease .3s;
  }
  .onlinePerson:hover {
    width: 55px;
    background-color: #dee9e9;
    cursor: pointer;
  }
  .extendStyle {
    width: 200px;
    height: auto;
    max-height: 320px;

  }
  .internalHeight {
    width: auto;
    height: auto;
    max-height: 200px;
    overflow: auto;
    padding: 5px 1px;
  }
  .fontStyle {
    font-size: 18px;
    text-align: center;
    color: #3e3036;
    font-family: 微软雅黑;
  }
  .hoverStyle:hover{
    background-color: #e2e9c8;
    cursor: pointer;
  }
</style>
