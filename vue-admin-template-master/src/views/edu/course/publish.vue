<template>

  <div class="app-container">


    <h2 style="text-align: center;">发布新课程</h2>

    <el-steps :active="3" process-status="wait" align-center style="margin-bottom: 40px;">
      <el-step title="填写课程基本信息"/>
      <el-step title="创建课程大纲"/>
      <el-step title="最终发布"/>
    </el-steps>
    <div class="ccInfo">
      <img :src="courseInfo.cover">
      <div class="main">
        <h2>{{ courseInfo.title }}</h2>
        <p class="gray"><span>共{{ courseInfo.lessonNum }}课时</span></p>
        <p><span>所属分类：{{ courseInfo.oneSubject }} — {{ courseInfo.twoSubject }}</span></p>
        <p>课程讲师：{{ courseInfo.teacherName }}</p>
        <h3 class="red">￥{{ courseInfo.price }}</h3>
      </div>
    </div>
    <el-form>
      <el-form-item>
        <el-button :disabled="saveBtnDisabled" type="primary" @click="previous">上一步</el-button>
        <el-button :disabled="saveBtnDisabled" type="primary" @click="saveOrUpdate">发布</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>

  import publish from '@/api/edu/publish'

  export default {

    data() {
      return {
        saveBtnDisabled: false,
        courseId: '',
        courseInfo: {
          id: '',
          lessonNum: 12,
          teacherName: '撒大大',
          oneSubject: '',
          price: 12,
          title: '',
          twoSubject: ''
        }
      }
    },
    watch: {},
    created() {
      if (this.$route.params && this.$route.params.id) {
        this.courseId = this.$route.params.id
        this.getPublishCourseInfo()
      }
    },

    methods: {
      getPublishCourseInfo() {
        publish.getPublishCourseInfo(this.courseId)
          .then(response => {
            this.courseInfo = response.data.courseInfo
          })
      },
      previous() {
        this.$router.push({ path: '/course/chapter/' + this.courseId })
      },
      saveOrUpdate() {
        publish.savePublish(this.courseId)
        .then(response=>{
          this.$message({
            type: 'success',
            message: '发布成功'
          })
          this.$router.push({ path: '/course/list'})
        })
      }
    }
  }
</script>

<style scoped>
  .ccInfo {
    background: #f5f5f5;
    padding: 20px;
    overflow: hidden;
    border: 1px dashed #DDD;
    margin-bottom: 40px;
    position: relative;
  }

  .ccInfo img {
    background: #d6d6d6;
    width: 500px;
    height: 278px;
    display: block;
    float: left;
    border: none;
  }

  .ccInfo .main {
    margin-left: 520px;
  }

  .ccInfo .main h2 {
    font-size: 28px;
    margin-bottom: 30px;
    line-height: 1;
    font-weight: normal;
  }

  .ccInfo .main p {
    margin-bottom: 10px;
    word-wrap: break-word;
    line-height: 24px;
    max-height: 48px;
    overflow: hidden;
  }

  .ccInfo .main p {
    margin-bottom: 10px;
    word-wrap: break-word;
    line-height: 24px;
    max-height: 48px;
    overflow: hidden;
  }

  .ccInfo .main h3 {
    left: 540px;
    bottom: 20px;
    line-height: 1;
    font-size: 28px;
    color: #d32f24;
    font-weight: normal;
    position: absolute;
  }
</style>
