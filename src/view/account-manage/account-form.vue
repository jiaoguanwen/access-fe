<template>
    <Form :model="form" ref="form" :rules="ruleValidate" label-position="left">
        <FormItem label="账号名称" prop="name">
            <Input placeholder="1-30个字符" v-model="form.name" clearable/>
        </FormItem>
        <FormItem label="密码" prop="passwd">
            <Input placeholder="6-18个字符" v-model="form.passwd" clearable type="password"/>
        </FormItem>
        <FormItem label="备注" prop="remark">
            <Input placeholder="0-20个字符" v-model="form.remark" clearable/>
        </FormItem>
        <FormItem label="账号权限" prop="auth">
            <CheckboxGroup v-model="form.auth">
                <Checkbox :label="1">添加人员</Checkbox>
                <Checkbox :label="2">添加访客</Checkbox>
                <Checkbox :label="3">查看考勤</Checkbox>
                <Checkbox :label="4">查看陌生人</Checkbox>
            </CheckboxGroup>
        </FormItem>
    </Form>
</template>

<script>
export default {
  name: 'account-form',
  props: {
    form: {
      type: Object,
      required: true
    }
  },
  data () {
    const validateName = (rule, value, callback) => {
      const nameReg = /^[a-zA-Z0-9]+$/
      value && !nameReg.test(value) && callback(new Error('账户输入不正确，请重新输入'))
      callback()
    }
    return {
      ruleValidate: {
        name: [
          {required: true, message: '姓名不能为空', trigger: 'blur'},
          {max: 30, message: '最多输入30个字符', trigger: 'change'},
          {validator: validateName, trigger: 'change'}
        ],
        passwd: [
          {required: true, message: '密码不能为空', trigger: 'blur'},
          {max: 18, message: '最多输入18个字符', trigger: 'change'},
          {min: 6, message: '最少输入6个字符', trigger: 'change'}
        ],
        remark: [
          {max: 20, message: '最多20个字符', trigger: 'change'}
        ]
      }
    }
  }
}
</script>

<style scoped lang="less">
    .ivu-form-item:last-child {
        margin-bottom: 0;
    }
</style>
