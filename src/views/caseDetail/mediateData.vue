<template>
<div class="tabItem">
  <collapse v-model="activeNames" bind:change="onChange">
    <!-- <collapse-item :name="index+100" :is-link="false" :border="false" v-for="(item,index) in applyedArr" :key="'i'+index" > -->
    <collapse-item name="1" :is-link="false" :border="false">
      <template slot="title">
        <div style="padding:15px 0 0px 0;">
          <h3>{{'调解资料'}}</h3>
        </div>
      </template>
      <div class="tabItemContainer">
        <div>
          <div class="flexInfoBox">
            <div class="infoItem">
              <div>调解申请时间</div>
              <div class="black mt2">{{arbitralMediationResponse.mediationTime}}</div>
            </div>
            <div class="infoLine"></div>
            <div class="infoItem">
              <div>调解申请流程</div>
              <div class="black mt2">{{DICT.getArbStatusName(arbitralMediationResponse.stopArbStatus)}}</div>
            </div>
          </div>
          <div class="flexInfoBox" v-if="arbitralMediationResponse&&arbitralMediationResponse.mediationDocuments">
            <div>
              <div class="fileBox">调解书</div>
              <div class="black mt2 fileBox" v-for="(file, index) in arbitralMediationResponse.mediationDocuments.filter(val => val.documentType === 8)" :key="'n'+index" @click="previewFile(file.documentName,file.attachUrlDes)">
                <span class="icon iconfont" :class="util.getFileIcon(file.documentName)"></span>
                <span class="fileName">{{file.documentName}}</span>
              </div>
            </div>
          </div>
          <div class="flexInfoBox" v-if="arbitralMediationResponse&&arbitralMediationResponse.mediationDocuments">
            <div>
              <div class="fileBox">调解笔录</div>
              <div class="black mt2 fileBox" v-for="(file, index) in arbitralMediationResponse.mediationDocuments.filter(val => val.documentType === 9)" :key="'o'+index" @click="previewFile(file.documentName,file.attachUrlDes)">
                <span class="icon iconfont" :class="util.getFileIcon(file.documentName)"></span>
                <span class="fileName">{{file.documentName}}</span>
              </div>
            </div>
          </div>
          <div class="flexInfoBox" v-if="conciliationEvidence&&conciliationEvidence.length&&conciliationEvidence[0].arbAttachmentDatas">
            <div>
              <div class="fileBox">调解证据</div>
              <div class="black mt2 fileBox" v-for="(file, index) in conciliationEvidence" :key="'o'+index">
                <div v-for="(item, index) in file.arbAttachmentDatas" :key="'p'+index" @click="previewFile(item.attachName,item.attachUrlDes)">
                  <span class="icon iconfont" :class="util.getFileIcon(item.attachName)"></span>
                  <span class="fileName">{{item.attachName}}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </collapse-item>
  </collapse>
</div>
</template>
<script>
import util from '@/utils/util'
import DICT from '@/const/dict'
import { Collapse, CollapseItem } from 'vant'
export default {
  name: 'MediateData',
  props: ['arbitralMediationResponse', 'conciliationEvidence', 'previewFile'],
  components: { Collapse, CollapseItem },
  data () {
    return {
      activeNames: ['1'],
      util,
      DICT
    }
  },
  methods: {
    getstatus (val) {
      var status = ''
      DICT.arbStatus.map(el => {
        if (el.value.split(',').map(el => +el).indexOf(val) > -1) {
          status = el.label
        }
      })
      return status
    }
  }
}
</script>
<style lang="stylus" scoped>
@import "../../stylus/caseDetails.styl";
</style>
