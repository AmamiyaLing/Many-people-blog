<template>
  <div class="sendDemo">
    <h2>demo视频+源码发布</h2>
    <div class="demoItem">
      <video v-show="video_path" id="video" controls="" preload="none" :poster="video_pic">
      <source :src="video_path" type="video/mp4">
      </video>
       <Upload action="http://codelei.cn:3000/upload/sendcontent"
               :on-success="handlePSuccess">
        <Button icon="ios-cloud-upload-outline">选择封面文件</Button>
        </Upload>
      <Upload action="http://codelei.cn:3000/upload/sendcontent"
               :on-success="handleVSuccess">
        <Button icon="ios-cloud-upload-outline">选择录屏文件</Button>
        </Upload>
        <p>{{code_path}}</p>
        <Upload action="http://codelei.cn:3000/upload/sendcontent"
               :on-success="handleCSuccess">
        <Button icon="ios-cloud-upload-outline">选择源码文件</Button>
        </Upload>
      <div class="contentPublish">
        <Input v-model="content" 
            maxlength="300" 
            show-word-limit type="textarea" 
            placeholder="Enter something..." 
            style="display:block;margin:1rem 0;width:30rem" />
        <Button @click.native="sendVideo" type="primary">发表</Button>
      </div>
    </div>
  </div>
</template>

<script>
import { PostMessage } from '../components/NetWork/request'
  export default {
    name:'senddemo',
    data () {
      return {
          video_pic:'',
          video_path:'',
          code_path:'',
          content:''
      };
    },
    methods: {
        handleVSuccess(res) {
             this.video_path = res.url
        },
        handlePSuccess(res) {
            this.video_pic = res.url
        },
        handleCSuccess(res) {
            this.code_path = res.url
        },
        sendVideo() {
            if(this.video_path && this.video_pic && this.code_path && this.content) {
            PostMessage('/upload/uploadDemo',{
                content:this.content, 
                videoPIC: this.video_pic, 
                videoPath: this.video_path, 
                codePath: this.code_path
            }
            )
            .then(res => {
            if(res.data.err === 0) {
              this.$Message.success(res.data.data);
              setTimeout(() => {
                this.$router.replace('/about');
              }, 1500);
            } else {
              this.$Message.error(res.data.data);
            }
          })
          } else {
            this.$Message.error('内容不能为空!')
          }
        }
    },
  }

</script>
<style lang="scss" scoped>
 .sendDemo {
   padding: 2rem;
   h2 {
     margin-bottom: 1rem;
   }
     .demoItem {
         position: relative;
         z-index: 99;
     }
 }
</style>