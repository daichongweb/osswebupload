<template>
  <div>
    <el-upload
      list-type="picture-card"
      class="avatar-uploader"
      action="http://hibuting.oss-cn-beijing.aliyuncs.com"
      :data="dataObj"
      accept=".jpeg, .png, .jpg"
      :multiple="false"
      :before-upload="beforeUpload"
      :on-success="handleUploadSuccess"
      :limit="limit"
      :show-file-list="false"
      ref="upload"
    >
      <img v-if="imageUrl" :src="imageUrl" class="avatar" />
      <i v-else class="el-icon-plus avatar-uploader-icon"></i>
    </el-upload>
  </div>
</template>
<script>
import { getPolicy } from "@/api/oss/token";
export default {
  name: "ossSingleUpload",
  props: {
    limit: Number,
    imgCover: String
  },
  computed: {},
  data() {
    return {
      dataObj: {
        policy: "",
        signature: "",
        key: "",
        ossaccessKeyId: "",
        dir: "",
        host: "",
        callback: ""
      },
      imageUrl: ""
    };
  },
  created() {
    setTimeout(() => {
      this.imageUrl = this.imgCover;
    }, 500);
  },
  methods: {
    emitInput(val) {
      this.$emit("input", val);
    },
    beforeUpload(file) {
      let _self = this;
      return new Promise((resolve, reject) => {
        getPolicy()
          .then(response => {
            _self.dataObj.policy = response.data.policy;
            _self.dataObj.signature = response.data.signature;
            _self.dataObj.ossaccessKeyId = response.data.accessid;
            _self.dataObj.key = response.data.dir + "${filename}";
            _self.dataObj.dir = response.data.dir;
            _self.dataObj.host = response.data.host;
            _self.dataObj.callback = response.data.callback;
            resolve(true);
          })
          .catch(err => {
            console.log(err);
            reject(false);
          });
      });
    },
    handleUploadSuccess(res, file) {
      this.$refs.upload.clearFiles();
      this.imageUrl = this.dataObj.host + "/" + this.dataObj.dir + file.name;
      this.$emit("getMessage", this.imageUrl);
    }
  }
};
</script>
<style>
.avatar-uploader .el-upload {
  border: 1px dashed #d9d9d9;
  border-radius: 6px;
  cursor: pointer;
  position: relative;
  overflow: hidden;
}
.avatar-uploader .el-upload:hover {
  border-color: #409eff;
}
.avatar-uploader-icon {
  width: 100%;
  font-size: 28px;
  color: #8c939d;
  text-align: center;
}
.avatar {
  width: 100%;
  display: block;
}
</style>


