<template>
  <div class="linkage-wrap">
    <div class="address-box">
      <input class="input" :value="address" disabled type="text" placeholder="请选择所在地区">
      <span class="btn" @click="addressFlag = !addressFlag">点击选择</span>
    </div>
    <div class="pick-mark" v-show="addressFlag">
      <div class="btn-box">
        <a class="btn-cancel" @click="addressFlag = !addressFlag">取消</a>
        <a class="btn-sure" @click="onFillAddress">确定</a>
      </div>
      <mt-picker class="select" :slots="slots" value-key="aname" @change="onValuesChange"></mt-picker>
    </div>
  </div>
</template>

<script>
import { address, slots } from './address';

export default {
  data() {
    return {
      slots,
      address: '',
      tempAddress: '',
      addressFlag: false
    };
  },
  mounted() {
    this.onInitAddress();
  },
  methods: {
    onInitAddress() {
      this.slots[0].values = address.filter((item, index) => {
        if (item.apid == 0) {
          return item;
        }
      });
    },
    onFillAddress() {
      // 填入省市区
      this.address = this.tempAddress;
      this.addressFlag = !this.addressFlag;
    },
    onValuesChange(picker, values) {
      // 防止没有省份时报错
      if (values[0]) {
        this.slots[1].values = address.filter((item, index) => {
          if (item.apid == values[0].aid) {
            return item;
          }
        });
      }
      // 防止没有市时报错
      if (values[1]) {
        this.slots[2].values = address.filter((item, index) => {
          if (item.apid == values[1].aid) {
            return item;
          }
        });
      }
      // 防止没有区时报错
      if (values[2]) {
        // 这里可以指定地址符，此处以空格进行连接
        this.tempAddress = values[0].aname + ' ' + values[1].aname + ' ' + values[2].aname;
      }
    }
  }
};
</script>

<style lang="scss">
.linkage-wrap {
  position: relative;
  .address-box {
    display: flex;
    justify-content: space-between;
    align-items: center;
    height: 30px;
    padding: 10px;
    font-size: 14px;
    color: #333;
    .input {
      display: flex;
      align-items: center;
      flex: 1;
      height: 100%;
      padding: 0;
      box-sizing: border-box;
    }
    .btn {
      flex: 0 0 80px;
      width: 80px;
      height: 30px;
      line-height: 30px;
      text-align: center;
      border: 1px solid #ccc;
      border-left: 0 none;
      box-sizing: border-box;
    }
  }
  .pick-mark {
    position: fixed;
    left: 0;
    top: 0;
    bottom: 0;
    right: 0;
    background: rgba(236, 240, 243, 0.8);
    .btn-box {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 180px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      height: 40px;
      padding: 0 20px;
      font-size: 14px;
      background: #fff;
      .btn-cancel {
        color: #55f;
      }
      .btn-sure {
        color: #e5004d;
      }
    }
    .select {
      position: absolute;
      left: 0;
      bottom: 0;
      width: 100%;
    }
    .picker-items {
      background: #eee;
      .picker-slot {
        font-size: 14px;
      }
      .picker-item {
        &.picker-selected {
          color: #535353;
        }
      }
      .picker-center-highlight {
        &:after,
        &:before {
          background: #fff;
        }
      }
    }
  }
}
</style>
