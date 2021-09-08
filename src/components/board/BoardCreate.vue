<template>
  <div>
    <b-input v-model="subject" placeholder="제목을 입력해주세요."></b-input>
    <b-form-textarea
        v-model="context"
        placeholder="내용을 입력해 주세요"
        rows="3"
        max-rows="6"
    ></b-form-textarea>
    <br>
    <b-button @click="updateMode ? updateContent() : uploadContent()">저장</b-button>&nbsp;
    <b-button @click="cancle">취소</b-button>
  </div>
</template>

<script>
import data from '@/data'

export default {
  name: 'BoardCreate',
  data() {
    return {
      subject: '',
      context: '',
      user_id: 1,
      created_at: "2018-09-11",
      updated_at: null,
      updateObject: null,
      updateMode: this.$route.params.contentId > 0 ? true : false,
    }
  },
  created() {
      if(this.$route.params.contentId > 0) {
        const contentId = Number(this.$route.params.contentId)
        this.updateObject = data.Content.filter(contentItem => contentItem.content_id === contentId)[0]
        this.subject = this.updateObject.title;
        this.context = this.updateObject.context;
      } 
  },
  methods: {
    uploadContent() { // 저장

        let contentItems = data.Content.sort((a, b) => { return b.content_id - a.content_id });
        const content_id = contentItems[0].content_id + 1;  // 마지막 데이터의 id + 1

        data.Content.push({
          content_id: content_id,
          user_id: this.user_id,
          title: this.subject,
          context: this.context,
          created_at: this.created_at,
          updated_at: this.updated_at
        })

        this.$router.push({
          path: '/'
        })
    },
    updateContent() { // 수정
        this.updateObject.title = this.subject;
        this.updateObject.context = this.context;
        this.$router.push({
          path: '/'
        })
    },
    cancle() {
      this.$router.push({
        path: '/'
      })
    }
  }
}
</script>

<style>

</style>