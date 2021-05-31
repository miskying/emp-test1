<template>
  <div class="select-wrap">
    <el-select
      :class="setBorder"
      :disabled="disabled"
      :placeholder="$t('placeholder.001')"
      :filterable="filterable"
      :filter-method="dataFilter"
      :loading="loading"
      :style="
        `width:${width};height:${height};font-size:${fontSize};background:${background}`
      "
      :value="currentValue"
      @change="changeValue"
    >
      <template v-for="(item, index) in newOptions">
        <el-option :key="index" :value="item.value" :label="item.label">
          <template v-if="showSlot">
            <span style="float: left; color: #8492a6; font-size: 13px">{{
              item.label
            }}</span>
            <span style="float: right; color: #8492a6; font-size: 13px">{{
              item[showSlot]
            }}</span>
          </template>
        </el-option>
      </template>
    </el-select>
  </div>
</template>

<script>
/**下拉控件
 * @param width 宽度
 * @param height 高度
 * @param fontSize 字体
 * @param background 背景色
 * @param value 当前选中值
 * @param setBorder 设置边框样式(none(无边框)，default(直边框)，radius(圆角表框))
 * @param options 下拉项数据
 * @param filterable 启用搜索
 * @callback changeValue 改变选中值方法
 */
export default {
    name: 'ComSelect',
    props: {
        background: {
            type: String,
            default: ''
        },
        loading: {
            type: Boolean,
            default: false
        },
        width: {
            type: String,
            default: '150px'
        },
        height: {
            type: String,
            default: '42px'
        },
        fontSize: {
            type: String,
            default: '16px'
        },
        value: {
            type: [String, Number],
            require: true
        },
        setBorder: {
            type: String,
            default: 'default'
        },
        filterable: {
            type: Boolean,
            default: false
        },
        disabled: {
            type: Boolean,
            default: false
        },
        options: {
            type: Array,
            default: () => {
                return [];
            }
        },
        showSlot: {
            type: String,
            default: ''
        }
    },
    computed: {
        newOptions() {
            let i = this.options.map(item => {
                if (!item.value) {
                    return {
                        ...item,
                        label: item.name,
                        value: item.id.toString()
                    };
                }
                return item;

            });
            return i;
        }
    },
    data() {
        return {
            currentValue: this.value
        };
    },
    methods: {
        changeValue(val) {
            // if (this.filterable) {
            //   this.$emit("update:value", val);
            // } else {
            this.$emit('update:value', val);
            this.$emit('changeValue', val);
            // }
        },
        dataFilter(val) {
            this.currentValue = val;
            if (val && val.length > 0) {
                this.options.filter(item => {
                    if (
                        Boolean(~item.label.indexOf(val)) ||
            Boolean(~item.label.toUpperCase().indexOf(val.toUpperCase()))
                    ) {
                        return true;
                    }
                    if (!this.newOptions[this.newOptions.length - 1].add) {
                        let item = {
                            label: val,
                            value: val,
                            add: true
                        };
                        this.newOptions.push(item);
                    } else {
                        Object.assign(this.newOptions[this.newOptions.length - 1], {
                            label: val,
                            value: val
                        });
                    }
                    console.log(val);

                });
            }
        }
    },
    watch: {
        value(newVal, oldVal) {
            console.log(newVal);
            this.currentValue = newVal;
        },
        currentValue(newVal, oldVal) {
            if (newVal !== oldVal) {
                console.log(newVal);
                this.$emit('update:value', newVal);
            }
        }
    }
};
</script>

<style lang="scss" scoped>
.select-wrap {
  .default ::v-deep input {
    border-radius: 4px;
  }
  .radius ::v-deep input {
    border-radius: 21px;
  }
  .none ::v-deep input {
    background: transparent;
    border: none;
    color: #fff;
  }
  .none ::v-deep .el-icon-arrow-up:before {
    content: "\e78f" !important;
  }
  .radius ::v-deep .el-icon-arrow-up:before {
    content: "\e78f" !important;
  }
  ::v-deep .el-input {
    font-size: inherit;
  }
}
</style>
