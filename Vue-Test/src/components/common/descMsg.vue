<template>
  <div class="wrapper" v-if="showMsg">
    <div class="shade" @click="hideShade"></div>
    <div class="msgBox">
      <h4 class="msgTitle">详情介绍</h4>
      <table class="table table-hover">
        <tbody class="jobList">
          <tr v-for="(item, index) in jobDesc" v-if="index > 0">
            <td class="title">{{ item.title }}</td>
            <td class="ctn">{{ item.value }}</td>
          </tr>
        </tbody>
      </table>
      <div class="ft">
        <button type="button" class="btn btn-primary" @click="followComp" v-if="descType === 'DO_STAR'">跟踪该公司</button>
        <button type="button" class="btn btn-primary" @click="starJob" v-if="descType === 'DO_STAR'">关注该工作</button>
        <button type="button" class="btn btn-primary" @click="cancleStar" v-else-if="descType === 'CANCLE_STAR'">取消关注</button>
      </div>
    </div>
  </div>
</template>

<script>
import { doStarJob, doCancleStar, doFollowComp } from '../../lib/vueHelper'

export default {
  data () {
    return {
    }
  },
  props: {
    jobDesc: {
      type: Array,
      default: []
    },
    showMsg: {
      type: Boolean,
      default: false
    },
    descType: {
      type: String,
      default: ''
    }
  },
  methods: {
    hideShade () {
      this.$emit('hideMsg')
    },
    starJob () {
      let data = {
        uid: sessionStorage.getItem('uid'),
        jobId: this.jobDesc[0].value
      }
      doStarJob(this, data)
    },
    followComp () {
      let data = {
        uid: sessionStorage.getItem('uid'),
        company: this.jobDesc[2].value
      }
      doFollowComp(this, data)
    },
    cancleStar () {
      let data = {
        uid: sessionStorage.getItem('uid'),
        jobId: this.jobDesc[0].value
      }
      doCancleStar(this, data)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang='less'>

@white: #FFFFFF;
@blue: #20A0FF;
@box-radius: .5em;

.wrapper {
  position: absolute;
  top: 0;
  left: 0;
  margin: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  height: 100%;
}

.shade {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: hsla(0, 0%, 0%, .7);
  z-index: 999;
}

.msgBox {
  width: 400px;
  height: auto;
  background-color: @blue;
  border-radius: @box-radius;
  z-index: 9999;

  h4 {
    color: @white;
  }

  table {
    background-color: #E5E9F2;
    margin-bottom: 0;
    border-top: 1px solid #ccc;
    border-bottom: 1px solid #ccc;
  }

  .title {
    width: 100px;
    border-right: 1px solid #333744;
  }

  .ctn {
    width: 300px;
    max-height: 150px;
    padding: 5px;
    display: -webkit-box;
    overflow: auto;
    -webkit-line-clamp: 6;
    -webkit-box-orient: vertical;
    line-height: 25px;
    word-wrap: break-word;
    word-break: normal;
  }
}

.ft {
  background-color: #E5E9F2;
  border-bottom-left-radius: @box-radius;
  border-bottom-right-radius: @box-radius;

  button {
    margin: 8px 0 8px 0;
  }

  button:nth-child(n+2) {
    margin-left: 10px;
  }
}

</style>
