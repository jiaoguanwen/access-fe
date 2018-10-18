<template>
    <Form :model="form" @submit.native.prevent ref="form">
        <FormItem prop="employee_all">
            <span slot="label" style="font-weight: bold;">允许人员访问</span>
            <RadioGroup v-model="form.employee_all" @on-change="handleChange">
                <Radio label="true">全部人员</Radio>
                <Radio label="false">部分人员</Radio>
            </RadioGroup>
            <!--<a style="float: right;" @click.prevent="importAuthConfig">导入权限配置</a>-->
        </FormItem>
        <p class="margin-bottom-10">以允许的人员才可以访问，请在下方选择可以访问的人员</p>
        <!--<Input placeholder="请输入姓名或部门" v-model="query" @on-enter="search" @on-change="search"
               :disabled="form.employee_all === 'true'" clearable class="margin-bottom-10"/>-->
        <Transfer :data="form.civils" :target-keys="form.accessCivils" :render-format="renderFormat"
                  @on-change="selectChange" :titles="titles"/>
        <FormItem prop="interviewee_all">
            <span slot="label" style="font-weight: bold;">允许访客访问</span>
            <RadioGroup v-model="form.interviewee_all">
                <Radio label="true">是</Radio>
                <Radio label="false">否</Radio>
            </RadioGroup>
        </FormItem>
    </Form>
</template>

<script>
export default {
  name: 'auth-form',
  props: {
    form: {
      type: Object,
      required: true
    }
  },
  data () {
    return {
      query: '',
      titles: ['不可访问人员', '可访问人员'],
      civils: [],
      accessCivils: [],
      addSensorAttrs: [],
      deleteSensorAttrs: []
    }
  },
  methods: {
    /* importAuthConfig() {
                this.$Modal.confirm({
                    title: '导入权限配置',
                    okText: '导入',
                    onOk() {
                    },
                    render(h) {
                        return h('div', [
                            h('Table')
                        ])
                    }
                })
            }, */
    search () {
    },
    selectChange (newTargetKeys) {
      this.form.accessCivils = newTargetKeys
    },
    handleChange (v) {
      if (v === 'true') {
        // 禁止操作
        this.form.civils.forEach(item => {
          item.disabled = true
        })
      } else {
        this.form.civils.forEach(item => {
          item.disabled = false
        })
      }
    },
    renderFormat (item) {
      return `${item.name} - ${item.department}`
    }
  }
}
</script>

<style scoped>
    .ivu-form-item {
        margin-bottom: 0;
    }

    .transfer-wrapper {
        margin-top: 10px;
    }
</style>
