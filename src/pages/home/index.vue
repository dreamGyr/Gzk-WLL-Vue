<script src="../../../../../gzk_script_msg_ctrl.js"></script>
<template>
    <div id="home" style="text-align: center">
      欢迎来到 果真快物流链平台管理
      <br>
      开始你的操作吧！
    </div>
</template>
<script>

    /*接口 api*/
  import selfGZKMsgCtrl from 'getDataApi/index';
  import GetMsg from 'getDataApi/doMsg';
  export default{
    data () {
      return {

      }
    },
    components: {},
    methods: {},
    mounted () {

    },
    created(){


        /*消息测试*/
        console.log("11111")
      let moment = require('moment');
      console.log(moment().format("dddd, MMMM Do YYYY, h:mm:ss a"))
      selfGZKMsgCtrl.doBindMsg(10000, (data)=>{
        console.log('执行了11111',data)
      });
      selfGZKMsgCtrl.doSendApp2Msg(GetMsg.doGetMsg_10000("18839537516","q"))

    },
    watch: {}
  }

</script>
