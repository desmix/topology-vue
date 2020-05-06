<template>
  <div>
    <!-- 选中为空 -->
    <div v-if="!props.node && !props.line && !props.multi">
      <div class="title">欢迎使用le5le-topology！</div>
      <div class="group">
        <a class="star" href="https://github.com/le5le-com/topology" target="_blank">喜欢，点击这里打个star吧</a>
        <a href="https://www.yuque.com/alsmile/topology" target="_blank">使用教程</a>
        <br />
        <a
          href="http://topology.le5le.com/assets/img/topology_wechat.jpg?t=1"
          target="_blank"
        >微信交流群（大群）</a>
        <br />
        <a href="http://topology.le5le.com/assets/img/topology_wechat2.jpg" target="_blank">微信交流群2</a>
        <br />
        <a href="https://www.yuque.com/alsmile/topology/faq#EVbCgt" target="_blank">联系我们</a>
      </div>
      <div class="title">[Todo] 未来规划</div>
      <ul class="group">
        <li>Github issues</li>
        <li>React demo</li>
        <li>Vue3 demo</li>
        <li>系列教程</li>
      </ul>
      <div class="bottom">
        <div class="title">小提示</div>
        <ul class="group">
          <li>方向键：控制节点移动5个像素</li>
          <li>Ctrl + 方向键：控制节点移动1个像素</li>
          <li>Ctrl + 鼠标移动：移动整个画布</li>
          <li>Ctrl + 鼠标滚轮：缩放</li>
          <li>添加或选中节点，右侧属性支持上传各种图片哦</li>
        </ul>
      </div>
    </div>
    <!-- 选中节点 -->
    <div v-if="props.node" class="prop-selected">
      <div class="title title-first">位置和大小</div>
      <div class="items">
        <div class="flex grid">
          <div>X（px）</div>
          <div class="ml5">Y（px）</div>
        </div>
        <div class="flex grid">
          <div>
            <el-input-number
              v-model="props.node.rect.x"
              controls-position="right"
              @change="onChange"
            ></el-input-number>
          </div>
          <div class="ml5">
            <el-input-number
              v-model="props.node.rect.y"
              controls-position="right"
              @change="onChange"
            ></el-input-number>
          </div>
        </div>
      </div>
      <div class="items">
        <div class="flex grid">
          <div>宽（px）</div>
          <div class="ml5">高（px）</div>
        </div>
        <div class="flex grid">
          <div>
            <el-input-number
              v-model="props.node.rect.width"
              controls-position="right"
              @change="onChange"
              :min="0"
            ></el-input-number>
          </div>
          <div class="ml5">
            <el-input-number
              v-model="props.node.rect.height"
              controls-position="right"
              @change="onChange"
              :min="0"
            ></el-input-number>
          </div>
        </div>
      </div>
      <div class="items" v-if="props.node.is3D">
        <div class="flex grid">
          <div>Z（px）</div>
        </div>
        <div class="flex grid">
          <div>
            <el-input-number
              v-model="props.node.z"
              controls-position="right"
              @change="onChange"
            ></el-input-number>
          </div>
        </div>
      </div>
      <div class="items">
        <div class="flex grid">
          <div title="百分比%对应的小数值">圆角（0 - 1）</div>
          <div class="ml5">旋转（°）</div>
        </div>
        <div class="flex grid">
          <div>
            <el-input-number
              v-model="props.node.borderRadius"
              controls-position="right"
              @change="onChange"
              :min="0"
              :max="1"
              :step="0.1"
            ></el-input-number>
          </div>
          <div class="ml5">
            <el-input-number
              v-model="props.node.rotate"
              controls-position="right"
              @change="onChange"
            ></el-input-number>
          </div>
        </div>
      </div>
      <div class="items">
        <div class="flex grid">
          <div title="padding-left">内边距-左</div>
          <div title="padding-right" class="ml5">内边距-右</div>
        </div>
        <div class="flex grid">
          <div>
            <el-input
              size="small"
              v-model="props.node.paddingLeft"
              controls-position="right"
              @change="onChange"
            ></el-input>
          </div>
          <div class="ml5">
            <el-input
              size="small"
              v-model="props.node.paddingRight"
              controls-position="right"
              @change="onChange"
            ></el-input>
          </div>
        </div>
      </div>
      <div class="items">
        <div class="flex grid">
          <div title="padding-top">内边距-上</div>
          <div title="padding-bottom" class="ml5">内边距-下</div>
        </div>
        <div class="flex grid">
          <div>
            <el-input
              size="small"
              v-model="props.node.paddingTop"
              controls-position="right"
              @change="onChange"
            ></el-input>
          </div>
          <div class="ml5">
            <el-input
              size="small"
              v-model="props.node.paddingBottom"
              controls-position="right"
              @change="onChange"
            ></el-input>
          </div>
        </div>
      </div>
      <div class="items gray" style="line-height: 1.5">
        内边距：输入数字表示像素；输入%表示百分比
      </div>
      <div class="title middle">
        <label>图片</label>
        <el-tooltip content="图片、字体图标同时存在时，图片优先" placement="top-start"><i class="iconfont icon-help-circle"></i>
        </el-tooltip></div>
      <div class="items">
        <div class="flex grid middle">
          <label style="width: .6rem">图片选择</label>
          <div class="icon" @click="showDialog = 1">
            <div v-if="!props.node.image" class="image-upload">
              <i class="iconfont icon-add inline"></i>
            </div>
            <img v-if="props.node.image" :src="props.node.image" style="height: .3rem" />
          </div>
        </div>
        <div v-if="1 == showDialog" class="props-tools">
          <div class="title flex middle">
            <label class="full">选择图片</label>
            <i class="iconfont icon-close hover" @click="showDialog = 0"></i>
          </div>
          <div class="ph20 mt20">
            <el-button type="primary" size="small" class="mr20" @click="onImageUpload()">
              + 上传本地图片
            </el-button>
            <el-button type="primary" size="small" @click="onImageUrl()">
              在线URL图片
            </el-button>
          </div>
          <div class="body">
            <div class="image blank" :class="{'active': !props.node.image}" @click="onClickImage({})" title="空白"></div>
            <div class="image" v-for="(item, index) in images"
              :key="index"
              :class="{'active': item.image === props.node.image}" @click="onClickImage(item)">
              <img :src="item.image" />
              <i class="iconfont icon-close" title="从图片库中移除，但不删除图片" @click="onRemoveImage($event, item, index)"></i>
            </div>
          </div>
        </div>
        <div class="flex grid">
          <div>宽（px）</div>
          <div class="ml5">高（px）</div>
        </div>
        <div class="flex grid">
          <div>
            <el-input-number
              v-model="props.node.imageWidth"
              controls-position="right"
              @change="onChange"
              :min="0"
            ></el-input-number>
          </div>
          <div class="ml5">
            <el-input-number
              v-model="props.node.imageHeight"
              controls-position="right"
              @change="onChange"
              :min="0"
            ></el-input-number>
          </div>
        </div>
      </div>
      <div class="items">
        <label class="checkbox">
          <el-checkbox v-model="props.node.imageRatio">保存图片比例</el-checkbox>
        </label>
      </div>
      
      <div class="title"></div>
      <div class="items">
        <div class="flex grid">
          <div class="custom-data">自定义数据 <i :class="props.expand ? 'el-icon-zoom-out' : 'el-icon-zoom-in'" @click="changeExpand" size='small'>{{props.expand ? '缩小' : '放大'}}</i></div>
        </div>
        <div class="flex grid">
          <div :class="props.expand ? 'expand-data' : ''">
            <el-input
              type="textarea"
              v-model="nodeData"
              :rows="props.expand ? 15 : 3"
              @change="onChange"
            ></el-input>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script >
export default {
  data() {
    return {
      nodeId: null,
      nodeIsJson: false,
      nodeData: '',
      showDialog: 0,
      images: [], // 用户上传的图片
      imagesSearch: {
        pageIndex: 1,
        pageCount: 20,
        count: 0
      }
    }
  },
  props: {
    props: {
      type: Object,
      require: true
    }
  },
  mounted() {
    console.log('CanvasProps mounted.');
    this.getImages();
  },
  updated() {
    if (!this.props.node || this.nodeId === this.props.node.id) {
      return;
    }
    this.props.expand = false;
    this.nodeId = this.props.node.id;
    let originData = this.props.node.data;
    this.nodeIsJson = this.isJson(originData);
    this.nodeData = this.nodeIsJson ?
      JSON.stringify(originData, null, 4) :
      this.nodeData = originData;
  },
  methods: {
    onChange(value) {
      if (this.props.node) {
        this.props.node.data = this.nodeIsJson ? JSON.parse(this.nodeData) : this.nodeData;
      }
      this.$emit('change', this.props.node);
    },
    changeExpand() {
      this.props.expand = !this.props.expand;
    },
    isJson (obj) {
      return typeof(obj) == "object" && Object.prototype.toString.call(obj).toLowerCase() == "[object object]" && !obj.length;
    },
    async getImages() {
      let ret = await this.$axios.$get(
        `/api/user/images?pageIndex=${this.imagesSearch.pageIndex}&pageCount=${this.imagesSearch.pageCount}`
      )
      if (ret.error) {
        return this.images = [];
      }
      this.images = ret.list || [];
    },
    onImageUpload() {
      const input = document.createElement('input');
      input.type = 'file';
      input.onchange = async event => {
        const elem = event.srcElement || event.target;
        if (elem.files && elem.files[0]) {
          const file = await this.imageUpload(elem.files[0], elem.files[0].name);
          if (!file) {
            return;
          }
          this.props.node.image = file.url;
          this.onChange();
          const id = await this.addImage(file.url);
          this.images.unshift({ id, image: file.url });
        }
      };
      input.click();
    },
    async imageUpload(blob, filename) {
      const form = new FormData();
      form.append('path', filename);
      form.append('randomName', '1');
      form.append('public', 'true');
      form.append('file', blob);
      const ret = await this.$axios.$post('/api/image', form);
      if (ret.error) {
        return null;
      }

      return ret;
    },
    async addImage(image) {
      const ret = await this.$axios.$post('/api/user/image', { image: image });
      if (ret.error) {
        return '';
      }

      return ret.id;
    },
    onImageUrl() {
      this.$prompt('输入图片URL', '图片URL', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        inputPattern: /(https?|ftp|file):\/\/[-A-Za-z0-9+&@#/%?=~_|!:,.;]+[-A-Za-z0-9+&@#/%=~_|]/,
        inputErrorMessage: 'URL格式不正确'
      }).then(async ({ value }) => {
        this.props.node.image = value;
        this.onChange();
        const id = await this.addImage(value);
        this.images.unshift({ id, image: value });
      }).catch((e) => {
        this.$message({
          type: 'info',
          message: '取消输入'
        });
      });
    },
    onClickImage(item) {
      this.props.node.image = item.image;
      this.onChange();
      this.showDialog = 0;
    },
    async onRemoveImage(event, item, index) {
      event.stopPropagation();
      if (await this.RemoveImage(item.id)) {
        this.images.splice(index, 1);
      }
    },
    async RemoveImage(id) {
      const ret = await this.$axios.$delete('/api/user/image/' + id);
      if (ret.error) {
        return false;
      }

      return true;
    }
  }
}
</script>

<style lang="scss">
$color-primary: #1890ff !default;
.star {
  display: block;
  color: #f60 !important;
  text-decoration: underline;
  margin-bottom: 0.1rem;
}

.title {
  color: #0d1a26;
  font-weight: 600;
  padding: 0.05rem 0.15rem;
  border-bottom: 1px solid #ccc;
}

.prop-selected {
  .title {
    border-bottom: 0;
    border-top: 1px solid #ccc;
  }
  .title-first {
    border-top: 0;
  }
}

.group {
  margin: 0.1rem 0 0.2rem 0.3rem;
  padding: 0;

  a,
  li {
    line-height: 2;
  }

  li {
    list-style: initial;
  }
}

.blank {
  border: 1px solid #ccc;
  width: 0.3rem;
  height: 0.3rem;
  background: url(/img/bk-parttern.png);
  opacity: 0.5;
}

.bottom {
  position: absolute;
  bottom: 0.1rem;
}

.middle {
  vertical-align: middle !important;
  align-items: center;
}

.icon-help-circle {
  font-size: .14rem;
  font-style: normal;
  font-weight: 400;
  color: #c4c9cd;
}
.items {
  padding: 0.05rem 0.15rem;

  .el-input-number {
    width: 1.02rem;
    line-height: 0.32rem;

    .el-input__inner {
      padding-left: 0;
      padding-right: 40px;
      height: 0.34rem;
      line-height: 0.34rem;
    }

    .el-input-number__increase {
      line-height: 0.16rem;
    }
  }

  .icon {
    height: 0.3rem;
    line-height: 1;
    cursor: pointer;

    .image-upload {
      display: flex;
      width: 0.3rem;
      height: 0.3rem;
      text-align: center;
      border: 1px dashed #d9d9d9;
      border-radius: 2px;
      justify-content: center;
      align-items: center;
    }

    .topology {
      font-size: 0.3rem;
    }
  }

  .el-checkbox, .el-checkbox .el-checkbox__label {
    font-size: inherit;
    color: inherit;
    font-weight: inherit;
  }
  .el-checkbox .el-checkbox__label {
    padding-left: 5px;
  }

  .custom-data i {
    cursor: pointer;
    float: right;
    color: #409eff;
    height: 2em;
    line-height: 2em;
  }

  .expand-data {
    position: absolute;
    right: 0.15rem;
    width: 5rem;
  }
}

.formItem {
  margin-bottom: 0.1rem;
}

.props-tools {
  position: fixed;
  background: #fff;
  right: 2.4rem;
  top: 0.4rem;
  bottom: 0;
  width: 3.2rem;
  border: 1px solid #ddd;

  .title {
    padding: 0.05rem 0.2rem;
    line-height: 0.3rem;
    border-top: 0;
    border-bottom: 1px solid #eee;

    .iconfont {
      font-weight: normal;
    }
  }

  .body {
    display: flex;
    flex-wrap: wrap;
    align-content: flex-start;
    padding: 0.1rem;
    height: calc(100% - 0.4rem);
    overflow: auto;

    .icon {
      width: 0.7rem;
      height: 0.7rem;
      display: inline-block;
      text-align: center;
      padding: 0.1rem;
      margin-bottom: 0.01rem;
      border: 1px solid transparent;

      i {
        font-size: 0.4rem;
        line-height: 1;
      }

      &:hover {
        border-color: $color-primary;
      }

      .blank {
        margin: 0.1rem auto;
      }
    }

    .image {
      margin: 0.1rem;
      width: 0.7rem;
      height: 0.7rem;
      display: inline-block;
      text-align: center;
      border: 1px solid #f0f0f0;
      position: relative;

      img {
        height: 100%;
        max-width: 100%;
      }

      .icon-close {
        line-height: 1;
        position: absolute;
        top: 0.05rem;
        right: 0.05rem;
        cursor: pointer;
        display: none;
        &:hover {
          color: $color-primary;
        }
      }

      &:hover {
        border-color: $color-primary;
        .icon-close {
          display: block;
        }
      }
    }

    .active {
      border: 1px solid $color-primary;
      color: #fff;
    }
  }

  .group {
    padding: 0;
    border-bottom: none;

    .title {
      padding: 0;
      border-bottom: none;
    }
  }
}
</style>
