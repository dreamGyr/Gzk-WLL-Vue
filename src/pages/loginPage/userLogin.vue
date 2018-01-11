<template>
  <div id="gzk_userLogin">
   <!-- <canvasBg parentsClass='gzk_userLogin'
              :config='{
                distance : 70,
                radius : 50
              }'
    ></canvasBg>-->
    <div class="gzk_bg"></div>
    <div class="g_loginFrom" id="login_box">
      <div class="gzk_loginTitle" style="margin-bottom: 20px;color: #000">果真快 . 物流链管理</div>
      <div  style="margin: auto auto;">
        <Form ref="formInline" :model="formInline" :rules="ruleInline" inline>
          <FormItem prop="user" >
            <Input type="text" v-model="formInline.user" placeholder="Username">
            <Icon type="ios-person-outline" slot="prepend"></Icon>
            </Input>
          </FormItem>
          <FormItem prop="password">
            <Input type="password" v-model="formInline.password" placeholder="Password">
            <Icon type="ios-locked-outline" slot="prepend"></Icon>
            </Input>
          </FormItem>
          <p style="text-decoration:underline;margin-left: 120px;cursor: pointer"  @click="instance('info')">
            忘记密码?</p>
          <FormItem style="width: 100%;margin-top: 20px;">
            <Button style="margin-left: 40%" type="primary" @click="handleSubmit('formInline')">登录</Button>
            <!--<template>
              <Button  @click="instance('info')">忘记密码?</Button>
            </template>-->
          </FormItem>

        </Form>
      </div>
    </div>
  </div>
</template>
<script>
  /*储存数据*/
  import Local from 'local'

  /*vuex*/
  import {mapActions} from "vuex"

  /*数据api*/
  import selfGZKMsgCtrl from 'getDataApi/index';
  import GetMsg from 'getDataApi/doMsg';
  /*import canvasBg from 'components/canvasBg';*/
  export default{
    data () {
      return {
        formInline: {
          user: '',
          password: ''
        },
        ruleInline: {
          user: [
            { required: true, message: '请填写用户名', trigger: 'blur' }
          ],
          password: [
            { required: true, message: '请填写密码', trigger: 'blur' },
      /*      { type: 'string', min: 6, message: '密码长度不能小于6位', trigger: 'blur' }*/
            { type: 'string', min: 1, message: '1111', trigger: 'blur' }
          ]
        }
      }
    },
    components: {
    /*  canvasBg*/
    },
    methods: {
      ...mapActions({
        setLoginStatus:"setLoginStatus"
      }),

      handleSubmit(name) {
        this.$refs[name].validate((valid) => {
          if (valid) {
              let user=this.formInline.user;
              let password=this.formInline.password;
//              接口返回
              selfGZKMsgCtrl.doBindMsg(300, (jsonMsg)=>{
                 switch (jsonMsg.intUserLoginStatus){
                     case 0:
                          this.$Message.success('登录成功!');
                         this.setLoginStatus({isLogin:true});
                         this.$router.push({name:'userOnePassword'})
                          break;
                     case 1:
                         this.$Message.success('电话尚未注册!');
                         break;
                     case 2:
                         this.$Message.success('登录密码错误!');
                         break;
                     case 3:
                         break;
                     default:
                         this.$Message.success('登录出错，请稍后再尝试!');
                 }
              });

              /*接口请求*/
              selfGZKMsgCtrl.doSendApp2Msg(GetMsg.doGetMsg_300(0,0,password,user));

          } else {
            this.$Message.error('登录失败!');
          }
        })
      },
      instance (type) {
        const title = '忘记密码?';
        const content = '<p>拨打400-9982-860 联系客服 更改密码</p>';
        const okText='知道了';
        switch (type) {
          case 'info':
            this.$Modal.info({
              title: title,
              content: content,
              okText: okText
            });
            break;
          case 'success':
            this.$Modal.success({
              title: title,
              content: content
            });
            break;
          case 'warning':
            this.$Modal.warning({
              title: title,
              content: content
            });
            break;
          case 'error':
            this.$Modal.error({
              title: title,
              content: content
            });
            break;
        }
      }
    },
    mounted () {

    },
    watch: {}
  }

</script>
