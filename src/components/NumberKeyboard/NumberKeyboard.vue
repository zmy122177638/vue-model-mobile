<template>
  <div :class="['number-keyboard-container',{on:isShow}]">
    <div class="keyboard-title">
      <div class="determine" @click="handleConfirmChange">确定</div>
    </div>
    <div class="keyboard-list">
      <div class="keyboard-item" 
        v-for="keyItem in keyList" 
        @touchstart="addKeyVal(keyItem,$event)"
        :key="keyItem">{{keyItem === '.' && !isSpotShow ? '' : keyItem}}</div>
      <div class="keyboard-item" @touchstart="deleteKeyVal()"><i class="delete-icon"></i></div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    // 是否显示 v-model
    value:{
      type: Boolean,
      default: false
    },
    // 返回val
    keyVal: {
      type: String | Number,
      default: ''
    },
    // 是否显示小数点
    isSpotShow:{
      type: Boolean,
      default: true
    },
    // 数字最大长度
    maxLength:{
      type: Number,
      default: -1
    },
    // 数字最小长度
    minLength:{
      type: Number,
      default: -1
    },
    // 最大数字
    maxNumber: {
      type: Number,
      default: Infinity
    }
  },
  data () {
    return {
      keyList: [1,2,3,4,5,6,7,8,9,'.',0],
      keyPressList: []
    }
  },
  computed: {
    isShow:{
      get(){
        return this.value;
      },
      set(val){
        this.$emit('input',val)
      }
    },
    keyboardVal:{
      get(){
        return this.keyVal;
      },
      set(val){
        this.$emit('update:keyVal',val)
      }
    },

  },
  watch: {
    keyPressList(now){
      const keyVal = now.join('') || '0'; 
      if(!/(^[1-9]([0-9]+)?(\.[0-9]{1,2})?$)|(^(0){1}$)|(^0\.$)|(^[0-9]\.[0-9]([0-9])?$)/.test(keyVal)){
        if(this.keyPressList.length > 0){
          if(/^0[1-9]$/.test(keyVal)){ 
            this.keyPressList.shift();
            return;
          }
          this.keyPressList.splice(this.keyPressList.length - 1,1);
        }
        return;
      }
      if(keyVal > this.maxNumber){
        this.$toast(`最多不能超于${this.maxNumber}`)
        this.keyboardVal = this.maxNumber + '';
        this.keyPressList = this.maxNumber.toString().split('')
        return;
      }
      this.keyboardVal = keyVal;
    },
    keyboardVal(now){
     this.keyPressList = now.toString().split('') || [];
    }
  },
  methods: {
    addKeyVal(keyItem,event){
      event.stopPropagation();
      if(keyItem === '.' && !this.isSpotShow){
        return;
      }
      if(this.maxLength && this.keyPressList.length === this.maxLength){
        return;
      }
      this.keyPressList.push(keyItem);
    },
    deleteKeyVal(){
      if(this.keyPressList.length > 0)this.keyPressList.splice(this.keyPressList.length - 1,1);
    },
    handleConfirmChange(){
      if(this.minLength && this.keyPressList.length < this.minLength){
        this.$toast(`最少不能少于${this.minLength}位`)
        return;
      }
      this.isShow = false;
    }
  }

}
</script>

<style lang="less" scoped>
  .number-keyboard-container{
    background-color:#ffffff;
    max-height: 0;
    transition: 0.35s all ease-out;
    &.on{
      max-height: 6rem;
    }
    .keyboard-title{
      display: flex;
      justify-content: flex-end;
      .determine{
        height:0.8rem;
        line-height: 0.8rem;
        padding:0 0.4rem;
        color:#1989fa;
        font-size:0.32rem;
      }
    }
    .keyboard-list{
      display: flex;
      flex-wrap: wrap;
      align-items: center;
      .keyboard-item{
        width:calc(100% / 3 + 1px);
        height:1rem;
        line-height: 1rem;
        font-size:.46rem;
        color:#000000;
        text-align: center;
        border:1px solid #ebedf0;
        margin-right:-1px;
        margin-bottom:-1px;
        .delete-icon{
          width: .42rem;
          height: .27rem;
          background:url('./delete_one.png') no-repeat;
          background-size:100% 100%;
          display: inline-block;
        }
        &:active{
          background-color:rgba(0,0,0,0.2);
        }
      }
    }
  }
</style>
