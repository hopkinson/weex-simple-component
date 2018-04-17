<!-- 上传图片 -->
<style lang='css' scoped>
.cs-upload {
  justify-content: center;
  align-items: center;
  margin: 10px;
}
.cs-upload--inner {
  align-items: center;
  justify-content: center;
  border-width: 1px;
  border-color: #666;
  background-color: #fff;
  margin-bottom: 10px;
}
.cs-upload--image{

}
</style>
<template>
<div class='cs-upload'>
  <div class='cs-upload--inner' :style='setStyle' @click='handleClick'>
    <wxc-icon name='add' size='large' v-if='!src'></wxc-icon>
    <image :style='setStyle' v-if='src' :src='src' @click='handleClick'></image>
  </div>
</div>
</template>
<script>
/**
 * Name
 * eros - 通过七牛云上传图片
 * <c-upload-pic :src.sync=""></c-upload-pic>
 *
 * props
 * boderStyle     边框样式
 * src            图片地址
 * width          宽度
 * height         高度
 */
import {
  WxcIcon
} from 'weex-ui'
var axios = weex.requireModule('bmAxios')
export default {
  data: () => ({
    Form: {
      token: '',
      key: '',
      file: ''
    }
  }),
  props: {
    boderStyle: {
      type: String,
      default: 'dotted'
    },
    src: {
      type: String
    },
    width: {
      type: Number,
      default: 250
    },
    height: {
      type: Number,
      default: 250
    },
  },
  computed: {
    setStyle() {
      return {
        borderStyle: this.boderStyle,
        width: `${this.width}px`,
        height: `${this.height}px`
      };
    }
  },
  methods: {
    // 1.上传图片
    handleClick() {
      this.$image
        .pick({
          maxCount: 1
        })
        .then(
          resData => {
            //    2.获取token
            this.loadToken(resData);
          },
          error => {
            this.$notice.toast({
              message: '选择图片失败'
            });
          }
        );
    },
    // 获取token
    loadToken(pics) {
      this.$notice.loading.show('图片上传中');
      this.$fetch({
        method: 'POST',
        name: 'sys/file/upload/param'
      }).then(data => {
        this.Form = {
          ...data,
          token: data.uploadToken,
          key: data[0] + Date.now(),
          file: data[0]
        };
        this.handleUplaod(pics)
      });
    },
    // 上传到七牛云
    handleUplaod(pics) {
      axios.uploadImage({
          url: 'http://up-z2.qiniup.com',
          params: this.Form,
          header: {
            'Content-Type': 'multipart/form-data'
          },
          images: pics
        },
        resData => {
          let imgUrl = this.Form.accessDomain + '/' + resData.data[0].key
          this.$emit('update:src', imgUrl)
          this.src = imgUrl;
          this.$notice.loading.hide()
        }
      )
    }
  },
  components: {
    WxcIcon
  }
};
</script>
