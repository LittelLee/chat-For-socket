<template>
  <div id="app">
    <div class="container">
      <div class="row clearfix">
        <div class="col-xs-12 bordered">
          <div class="messageArea">
              <div :class="other.className" v-for="other in MessageAll" :key="other.index">
                {{other.information}}
                <!--
                    存在问题 ....能够使得框框的width是随着文字的多少进行变化的
                -->
              </div>
          </div>
          <div class="functionalArea ">
            <div class="input-group">
              <input type="text" v-model="msg" class="form-control" required="required" placeholder="Typing word......">
              <span class="input-group-addon btn" @click="send" >
                <div class="sendIcon" ></div>
              </span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
        msg: '',
        MessageAll : [
          {
            className : 'otherMessage',
            information : "this is ohterMessage"
          },
          {
            className : 'selfMessage',
            information : "a"
          },
          {
            className : 'otherMessage',
            information : "this is ohterMessage"
          }, {
            className : 'otherMessage',
            information : "this is ohterMessage"
          }
        ]
    }
  },
  methods : {
    send : function () {
      if(this.msg !== '') {
        this.MessageAll.push({className : 'selfMessage',information : this.msg});
        this.msg = '';
      }else if(this.msg.length ===0 || this.msg === ''){
        alert('消息不能为空');
      }
    }
  }
}
</script>

<style scoped>
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
    height: 484px;
    background-color: #e9f0e1;
    box-shadow: #0f0f0f 0px 0px 10px;
    display: flex;
    flex-flow: column;
  }
  .functionalArea {
    width: 100%;
    height: auto;
    background-color: #2b542c;
    box-shadow: #0f0f0f 0px 0px 10px;
  }
  .sendIcon {
    width: 35px;
    height: 20px;
    background: url("./assets/send.svg") no-repeat center;
    background-size: contain;
  }
  .otherMessage,.selfMessage {
    width: auto;
    height: auto;
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
</style>
