<template>
  <div class="app-container">
    <oss-single-upload @getMessage="coverImg" :imgCover="imgCover" :limit="1"></oss-single-upload>
  </div>
</template>

<script>
import ossSingleUpload from "@/components/Upload/ossSingleUpload";
export default {
  components: { ossSingleUpload },
  data() {
    return {
      imgCover: ""
    };
  },
  created() {},
  methods: {
    coverImg(imgUrl) {
      console.log(imgUrl);
    }
  }
};
</script>