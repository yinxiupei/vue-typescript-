<template>
  <article>
    <div class="form-container">
      <div class="form-item">
        <h2>## 关于我</h2>
        <div class="content-item">
          <p>Hi, 你好！我是南方姑娘。</p>
          <p>很高兴你来到这里，这是一个简单个人网站。现在内容东西比较少，以后可能会有关前端技术的小文章，也可能会有关于个人的闲言碎语，也可能会有你驻足的痕迹，当然可能还有你留下的文字。。。</p>
          <p>如果对网站、me或者anything有任何的建议和意见，欢迎你的留言。</p>
        </div>
      </div>
      <div class="form-item">
        <h2>## 关于网站</h2>
        <div class="content-item">
          <p>这是一个前端使用 Vue + Router + Vue-Resource，服务器端使用 koa，数据库使用 MongoDB的SPA网站。后续更多功能开发中。。。</p>
        </div>
      </div>
      <!-- <div class="form-item">
        <h2>## 关于背景音乐</h2>
        <div class="content-item">
          <p>《独家记忆》第一次听到这首歌曲，就念念不忘。我喜欢你，是我独家的记忆...</p>
          <p>《独家记忆》个人是很喜欢古风类的音乐和歌曲，当然不会忽略掉它。大家有什么好听的古风歌曲，欢迎推荐</p>
        </div>
      </div> -->
      <div class="form-item">
        <h2>## 关于杂句</h2>
        <div class="content-item">
          <p>岁月流转，念念不忘，群碑未成，来日方长。-- 蔡康永  来自《纪念碑谷》</p>
          <p>不念过去，不畏将来。</p>
        </div>
      </div>
      <div class="form-item">
        <h2>## 留言板</h2>
        <div class="comment-container" v-if="payload.length > 0">
          <div class="comment-item" v-for="(item, index) in payload" :key="index">
            <p class="desc">{{ item.content }}</p>
            <p class="tips">#{{ item.name }} · {{ item.time.substring(0, 10) }}</p>
          </div>
        </div>
        <div v-else class="point">暂无留言</div>
      </div>
      <div class="form-item">
        <p>新增留言</p>
          <comment-input @onClick="onSubmit"></comment-input>
      </div>
    </div>
  </article>
</template>

<script>
import CommentInput from '@/components/blog/CommentInput'
export default {
  components: {
    CommentInput
  },
  data () {
    return {
      payload: []
    }
  },
  mounted () {
    this.getCommentInfo()
  },
  methods: {
    onSubmit (options) {
      this.$request.post({
        url: api => api.comment.create,
        params: {
          ...options,
          type: 1
        }
      }).then(response => {
        let body = response.body
        if (body.code === 1) {
          window.toast(body.msg)
          this.getCommentInfo()
        } else {
          window.alert(body.msg)
        }
      })
    },
    getCommentInfo () {
      this.$request.get({
        url: api => api.comment.read
      }).then(response => {
        let body = response.body
        if (body.code === 1) {
          this.payload = body.payload
        }
      })
    }
  }
}
</script>

<style lang="less" scoped>
.form-container {
  width: 780px;
  padding: 50px 20px;
  margin: 0 auto;
  box-sizing: border-box;
  border-left: 1px solid #f6f6f6;
  border-right: 1px solid #f6f6f6;
  .form-item {
    background: #ffffff;
    padding: 5px 30px 15px;
    margin-bottom: 30px;
    .point {
      text-align: center;
    }
    .form-input {
      width: 100%;
      margin: 15px auto;
      label {
        display: block;
        color: #888;
        font-size: 14px;
        margin: 5px 0;
      }
      .input-content {
        display: block;
        width: 80%;
        border-color: #eee;
        outline: none;
        border: 0;
        padding: 8px;
        border-radius: 3px;
        box-shadow: 0 0 0 1px rgba(0,0,0,.1), 0 1px 2px 0 rgba(0,0,0,.16);
        &:hover {
          box-shadow: 0 0 0 1px rgba(0,0,0,.1), 0 2px 6px 0 rgba(0,0,0,.16);
        }
      }
      textarea {
        height: 120px;
      }
      input {
        height: 20px;
      }
      .submit {
        width: 100px;
        display: inline-block;
        line-height: 1;
        padding: 8px 0;
        font-size: 14px;
        text-align: center;
        background: #dfdfdf;
        &:hover {
          background: #bbbbbb
        }
      }
    }
    h2 {
      color: #dfdfdf;
    }
    .content-item {
        font-size: 15px;
        margin: 5px;
        p {
            line-height: 1.5;
            margin-bottom: 8px;
        }
    }
    .comment-container {
      margin: 15px 0 0;
      .comment-item {
        padding: 5px 10px;
        background: #f5f5f5;
        color: #888;
        margin-bottom: 10px;
        font-size: 13px;
        border-radius: 2px;
        .tips {
          font-size: 12px;
          color: #cfcfcf;
          line-height: 20px;
          text-align: right;
        }
        &:last-child {
          margin-bottom: 0
        }
      }
    }
  }
}
</style>
