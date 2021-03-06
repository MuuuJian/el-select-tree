<template>
  <div class="app padding-medium text-center">
    <!-- author -->
    <h2>el-select-tree</h2>
    <p class="text-secondary">author: yujinpan - version: v{{ version }}</p>

    <el-divider></el-divider>

    <el-row type="flex">
      <!-- example -->
      <el-col :span="12">
        <pre><code v-html="code1"></code></pre>
      </el-col>
      <el-col class="text-left" :span="12">
        <!-- selector -->
        <div class="flex-center-align">
          <label>choose：</label>
          <el-select-tree
            v-if="show"
            :default-expand-all="defaultExpandAll"
            :multiple="multiple"
            :placeholder="placeholder"
            :disabled="disabled"
            :popover-min-width="100"
            :data="treeData"
            :props="treeProps"
            :disabled-values="disabledValues"
            :check-strictly="checkStrictly"
            :clearable="clearable"
            v-model="value1"
          ></el-select-tree>
          <div class="margin-left-medium">current value：{{ value1 }}</div>
        </div>
        <el-divider></el-divider>

        <!-- options -->
        <div class="flex-center-align">
          <label>choose any level(check-strictly)：</label>
          <el-switch v-model="checkStrictly"></el-switch>
          <b class="margin-left-medium" v-if="checkStrictly"
            >try to choose parent</b
          >
        </div>
        <el-divider></el-divider>
        <div class="flex-center-align">
          <label>disabled(disabled)：</label>
          <el-switch v-model="disabled"></el-switch>
        </div>
        <el-divider></el-divider>
        <div class="flex-center-align">
          <label>multiple choose(multiple)：</label>
          <el-switch v-model="multiple"></el-switch>
        </div>
        <el-divider></el-divider>
        <div class="flex-center-align">
          <label>expand all tree node(default-expand-all)：</label>
          <el-switch v-model="defaultExpandAll" @change="refresh()"></el-switch>
          <b class="margin-left-medium" v-if="defaultExpandAll"
            >You know, this property must be defined when initializing!</b
          >
        </div>
        <el-divider></el-divider>
        <div class="flex-center-align">
          <label>clearable(clearable)：</label>
          <el-switch v-model="clearable"></el-switch>
        </div>
        <el-divider></el-divider>

        <!-- form test -->
        <el-form
          ref="form"
          :model="form"
          :rules="formRule"
          class="flex-center-align"
          inline
        >
          <el-form-item label="with form：" prop="area">
            <el-select-tree
              v-if="show"
              :default-expand-all="defaultExpandAll"
              :multiple="multiple"
              :placeholder="placeholder"
              :disabled="disabled"
              :popover-min-width="100"
              :data="treeData"
              :props="treeProps"
              :disabled-values="disabledValues"
              :check-strictly="checkStrictly"
              :clearable="clearable"
              v-model="form.area"
            ></el-select-tree>
            <el-button
              @click="$refs.form.validate()"
              style="margin-left: 40px"
              type="primary"
              >Validate</el-button
            >
            <el-button
              @click="$refs.form.resetFields()"
              style="margin-left: 20px"
              >Reset</el-button
            >
          </el-form-item>
        </el-form>
        <el-divider></el-divider>
        <el-link
          type="primary"
          href="https://github.com/yujinpan/el-select-tree#attributes"
          >more attributes
          look：https://github.com/yujinpan/el-select-tree#attributes</el-link
        >
      </el-col>
    </el-row>
  </div>
</template>

<script>
import ElSelectTree from '../src';
import '../src/element-ui';

import { highlight } from './plugins/highlight';

const version = require('../package').version;

export default {
  components: {
    ElSelectTree
  },
  computed: {
    valueStr() {
      return String(this.value);
    }
  },
  data() {
    return {
      clearable: true,
      defaultExpandAll: false,
      multiple: false,
      placeholder: 'please choose',
      disabled: false,
      checkStrictly: false,
      version,
      code1: highlight('html', require('./template/example1').example1),
      value1: '2',
      treeData: [
        {
          label: '新疆维吾尔自治区',
          id: '1',
          childrens: [
            {
              label: '乌鲁木齐市',
              id: '2',
              childrens: [
                { label: '达坂城区', id: '7', childrens: [] },
                { label: '头屯河区', id: '8', childrens: [] },
                { label: '乌鲁木齐县', id: '9', childrens: [] }
              ]
            },
            {
              label: '克拉玛依市',
              id: '3',
              childrens: [
                { label: '克拉玛依区', id: '10', childrens: [] },
                { label: '白碱滩区', id: '11', childrens: [] },
                { label: '独山子区', id: '12', childrens: [] }
              ]
            },
            { label: '吐鲁番地区', id: '4', childrens: [] },
            { label: '哈密地区', id: '5', childrens: [] },
            { label: '昌吉回族自治州', id: '6', childrens: [] }
          ]
        }
      ],
      treeProps: {
        value: 'id',
        children: 'childrens',
        label: 'label'
      },
      disabledValues: ['3'],
      show: true,
      form: { area: '' },
      formRule: {
        area: { required: true, message: 'area is required.' }
      }
    };
  },
  methods: {
    refresh() {
      this.show = false;
      setTimeout(() => (this.show = true), 200);
    }
  }
};
</script>

<style lang="scss" scoped>
@import './styles/common-variables';

.app {
  .el-col {
    padding: $spacing-medium;
    border-right: $border-base;
    &:last-of-type {
      border-right: 0;
    }
  }
  pre {
    text-align: left;
    border: $border-base;
    border-radius: $border-radius-base;
    background-color: $background-color-base;
    padding: $spacing-medium;
  }
  code {
    font-size: 12px;
    line-height: 1.8;
    font-family: Menlo, Monaco, Consolas, Courier, monospace;
    -webkit-font-smoothing: auto;
  }
}
</style>
