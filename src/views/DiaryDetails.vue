<template>
  <Layout :style="{ padding: '0 24px 24px' }">
    <Content :style="{ padding: '24px', minHeight: '280px', width: '100%' }">
      <Card :bordered="false" class="write" dis-hover>
        <p slot="title" v-text="title"> </p>
        <a slot="extra" @click.prevent="peve">
          <Icon type="md-arrow-round-back" />
          返回
        </a>
        <div v-html="content"></div>
        <p class="tag">
          <Icon type="ios-pricetags" style="margin-right:20px;" />
          <Tag v-if="item!==''"
            :color="tagsColor[index]"
            v-for="(item, index) of tags"
            :key="index"
            v-text="item"
          ></Tag>
          <i style="float:right" v-text="writeDate"></i>
        </p>
      </Card>
    </Content>
  </Layout>
</template>

<script>
import { constants } from 'crypto';
export default {
  components: {},
  data() {
    return {
      userId: JSON.parse(localStorage.getItem("userInfo")).userId,
      title: "",
      content: "",
      tags: [],
      writeDate: "",
      tagsColor: [
        "primary",
        "success",
        "error",
        "warning",
        "magenta",
        "red",
        "volcano",
        "orange",
        "gold",
        "yellow",
        "lime",
        "green",
        "cyan",
        "blue",
        "geekblue",
        "purple",
        "#FFA2D3"
      ]
    };
  },
  created() {
    this.diaryDetails();
  },
  methods: {
    //根据id查询用户日记内容
    diaryDetails() {
      var diaryId = this.$route.params.id;
      this.request
        .httpGet(this.requestUrl.diaryDetails, {
          diaryId: diaryId,
          userId: this.userId
        })
        .then(res => {
          if (res.code === 200) {
            this.title = res.data.title;
            this.content = res.data.content;
            this.tags = res.data.tags;
            this.writeDate =this.formateDate(res.data.writeDate);
          } else {
            this.$Message.error("根据日记id查询用户日记内容失败")
          }
        });
    },
    formateDate(value) {
      var dateMat = new Date(value);
      var year = dateMat.getFullYear();
      var month = dateMat.getMonth() + 1;
      var day = dateMat.getDate();
      var date = year + "-" + month + "-" + day;
      return date;
    },
    //返回上一页
    peve(){
      this.$router.go(-1)
    }
  }
};
</script>
<style scoped>
.tags {
  margin-top: 10px;
}
.tag {
  margin-top: 50px;
}
</style>



