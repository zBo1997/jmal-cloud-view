<template>
  <span>
    <div v-if="item.isFavorite && !public">
        <div v-if="pc">
          <svg-icon v-if="!grid" class="pc list icon-favorite" icon-class="Favoritestarrate"/>
          <svg-icon v-if="grid" class="pc grid icon-favorite" icon-class="Favoritestarrate"/>
        </div>
        <div v-else>
          <svg-icon v-if="!grid" class="mobile list icon-favorite" icon-class="Favoritestarrate"/>
          <svg-icon v-if="grid" class="mobile grid icon-favorite" icon-class="Favoritestarrate"/>
        </div>
    </div>
    <div v-if="item.isShare && item.shareBase && !public">
        <div v-if="pc">
          <svg-icon v-if="!grid" class="pc list icon-share" icon-class="share"/>
          <svg-icon v-if="grid" class="pc grid icon-share" icon-class="share"/>
        </div>
        <div v-else>
          <svg-icon v-if="!grid" class="mobile list icon-share" icon-class="share"/>
          <svg-icon v-if="grid" class="mobile grid icon-share" icon-class="share"/>
        </div>
    </div>
    <svg-icon v-if="item.isFolder" icon-class="folder"/>
    <svg-icon v-else-if="item.contentType.indexOf('video') > -1" icon-class="video"/>
    <div v-else-if="item.contentType.indexOf('audio') > -1" v-on:mousedown="noDrag($event)">
      <div v-if="item.music !== undefined">
        <div v-if="item.music.name !== null">
          <el-image v-if="grid" :style="{'height':details?'110px':(gridWidth-35) + 'px'}" fit="contain"
                    :src="item.fileId ? (audioCoverUrl+item.fileId) : (audioCoverUrl+item.id)">
          <div slot="error" class="image-slot">
            <svg-icon icon-class="audio"/>
          </div>
        </el-image>
        <el-avatar v-if="!grid" shape="square"
                   :src="item.fileId ? (audioCoverUrl+item.fileId) : (audioCoverUrl+item.id)">
          <div slot="default">
            <svg-icon class="avatar-default-image" icon-class="audio"/>
          </div>
        </el-avatar>
        </div>
      </div>
      <svg-icon v-else icon-class="audio"/>
    </div>
    <!--<svg-icon v-else-if="item.contentType.indexOf('text') > -1" icon-class="file-txt"/>-->
    <div v-else-if="item.contentType.startsWith('image')" v-on:mousedown="noDrag($event)">
      <el-image v-if="grid || grid === 'details'" :style="{'height':details?'110px':(gridWidth-35) + 'px'}"
                fit="contain" :src="item.fileId ? (imageUrl+item.fileId) : (imageUrl+item.id)">
        <div slot="error" class="image-slot">
          <svg-icon icon-class="loading-image-error"/>
        </div>
      </el-image>

      <el-avatar v-if="!grid" shape="square"
                 :src="item.fileId ? (imageUrl+item.fileId) : (imageUrl+item.id)"></el-avatar>
    </div>
    <!--<svg-icon v-else-if="item.contentType.indexOf('application/pdf') > -1" icon-class="file-pdf"/>-->
    <!--<svg-icon v-else-if="item.contentType.indexOf('word') > -1" icon-class="file-word"/>-->
    <!--<svg-icon v-else-if="item.contentType.indexOf('zip') > -1" icon-class="zip"/>-->
    <!--<svg-icon v-else-if="item.contentType.indexOf('excel') > -1" icon-class="file-excel"/>-->
    <svg-icon v-else :icon-class="findIconClass"/>
  </span>
</template>
<script>
import {iconClass} from '@/utils/file-type'

export default {
  name: 'IconFile',
  props: {
    imageUrl: {
      type: String,
      default: ''
    },
    audioCoverUrl: {
      type: String,
      default: ''
    },
    grid: {
      type: Boolean,
      default: false
    },
    gridWidth: {
      type: Number,
      default: 120
    },
    details: {
      type: Boolean,
      default: false
    },
    public: {
      type: Boolean,
      default: false
    },
    item: {
      type: Object,
      default: function () {
        return {}
      }
    }
  },
  data() {
    return {
      pc: window.pc,
    }
  },
  mounted() {
  },
  computed: {
    findIconClass() {
      let suffix = this.item.suffix;
      if (!suffix && this.item.fileName) {
        suffix = this.item.fileName.substring(this.item.fileName.lastIndexOf('.') + 1);
      }
      if (iconClass.has(suffix)) {
        return iconClass.get(suffix)
      }
      return 'file'
    },
  },
  methods: {
    noDrag(e) {
      // console.log('noDrag',e)
      // e.preventDefault()
    }
  }
}
</script>
<style lang="scss" scoped>
.avatar-default-image {
  height: 35px;
  width: 35px;
  line-height: 35px;
}

.icon-favorite {
  position: absolute;
  color: #f5222d;
  z-index: 1;
}

.pc.grid.icon-favorite {
  font-size: 1.5rem;
  left: 0;
  top: 0;
}

.mobile.grid.icon-favorite {
  width: 1.3rem;
  height: 1.3rem;
}

.pc.list.icon-favorite {
  font-size: 1rem;
  left: 1.5rem;
}

.mobile.list.icon-favorite {
  font-size: 0.5rem;
  margin-left: -1.5rem;
}

.icon-share {
  color: #52c41a;
  position: absolute;
  z-index: 1;
}

.pc.grid.icon-share {
  font-size: 1.5rem;
  right: 0;
  top: 0;
}

.mobile.grid.icon-share {
  width: 1.3rem;
  height: 1.3rem;
  margin-left: 2.8rem;
}

.pc.list.icon-share {
  font-size: 1rem;
  right: 1.3rem;
}

.mobile.list.icon-share {
  font-size: 0.5rem;
  margin-left: 0.5rem;
}
</style>
