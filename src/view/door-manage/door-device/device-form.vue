<template>
    <Form :model="form" :rules="ruleValidate" ref="form">
        <FormItem prop="name" label="设备名称">
            <Input placeholder="1-50个字符" v-model="form.name" clearable/>
        </FormItem>
        <FormItem prop="gateIp" label="网络开关地址" class="ivu-form-item-required">
            <Input placeholder="1-50个字符" v-model="form.gateIp" clearable/>
        </FormItem>
        <!-- Video type video -->
        <FormItem prop="url" label="设备地址" v-show="form.deviceType === 'video'" class="ivu-form-item-required">
            <Input placeholder="1-50个字符" v-model="form.url" clearable/>
        </FormItem>
        <!-- 向下区分ftp/sdk -->
        <FormItem prop="subDeviceType" label="设备类型" v-show="form.deviceType === 'image'" class="ivu-form-item-required">
            <Select v-model="form.subDeviceType" :disabled="isEdit">
                <Option :value="item.value" v-for="item in deviceTypeArray" :key="item.value">{{item.text}}
                </Option>
            </Select>
        </FormItem>
        <!-- FTP type image -->
        <FormItem prop="ftpUsername" label="FTP用户名"
                  v-show="form.deviceType === 'image' && form.subDeviceType === 'ftp'"
                  class="ivu-form-item-required">
            <Input placeholder="1-50个字符" v-model="form.ftpUsername" clearable/>
        </FormItem>
        <FormItem prop="ftpPassword" label="FTP密码"
                  v-show="form.deviceType === 'image' && form.subDeviceType === 'ftp'"
                  class="ivu-form-item-required">
            <Input placeholder="1-50个字符" v-model="form.ftpPassword" clearable type="password"/>
        </FormItem>
        <FormItem prop="ftpIpAddr" label="FTP地址" v-show="form.deviceType === 'image' && form.subDeviceType === 'ftp'"
                  class="ivu-form-item-required">
            <Input placeholder="1-50个字符" v-model="form.ftpIpAddr" clearable/>
        </FormItem>
        <FormItem prop="ftpDir" label="FTP文件夹" v-show="form.deviceType === 'image' && form.subDeviceType === 'ftp'"
                  class="ivu-form-item-required">
            <Input placeholder="1-50个字符" v-model="form.ftpDir" clearable/>
        </FormItem>
        <!-- SDK type huimu dahua haikang-->
        <FormItem prop="sdkBrand" label="品牌" v-show="form.deviceType === 'image' && form.subDeviceType === 'sdk'"
                  class="ivu-form-item-required">
            <Select v-model="form.sdkBrand">
                <Option :value="item.value" v-for="item in sdkBrands" :key="item.value">{{item.text}}
                </Option>
            </Select>
        </FormItem>
        <FormItem prop="username" label="SDK用户名" v-show="form.deviceType === 'image' && form.subDeviceType === 'sdk'"
                  class="ivu-form-item-required">
            <Input placeholder="1-50个字符" v-model="form.username" clearable/>
        </FormItem>
        <FormItem prop="password" label="SDK密码" v-show="form.deviceType === 'image' && form.subDeviceType === 'sdk'"
                  class="ivu-form-item-required">
            <Input placeholder="1-50个字符" v-model="form.password" clearable type="password"/>
        </FormItem>
        <FormItem prop="ip" label="SDK IP" v-show="form.deviceType === 'image' && form.subDeviceType === 'sdk'"
                  class="ivu-form-item-required">
            <Input placeholder="1-50个字符" v-model="form.ip" clearable/>
        </FormItem>
        <FormItem prop="port" label="SDK端口" v-show="form.deviceType === 'image' && form.subDeviceType === 'sdk'"
                  class="ivu-form-item-required">
            <Input placeholder="1-50个字符" v-model="form.port" clearable/>
        </FormItem>
    </Form>
</template>

<script>
export default {
  name: 'device-form',
  props: {
    form: {
      type: Object,
      required: true
    },
    isEdit: {
      type: Boolean,
      required: true
    }
  },
  data () {
    const validateUrl = (rule, value, callback) => {
      if (this.form.deviceType === 'video' && !value) {
        callback(new Error('设备地址不能为空'))
      } else {
        callback()
      }
    }
    const validateFtp = (rule, value, callback) => {
      if (this.form.deviceType === 'image' && this.form.subDeviceType === 'ftp' && !value) {
        callback(new Error('FTP信息不完整'))
      } else {
        callback()
      }
    }
    const validateSdk = (rule, value, callback) => {
      if (this.form.deviceType === 'image' && this.form.subDeviceType === 'sdk' && !value) {
        callback(new Error('SDK信息不完整'))
      } else {
        callback()
      }
    }
    return {
      // todo 统一处理报错信息
      ruleValidate: {
        name: [
          {required: true, message: '设备名称不能为空', trigger: 'blur'},
          {max: 50, message: '最多输入50个字符', trigger: 'blur'}
        ],
        gateIp: [
          {required: true, message: '网络开关地址不能为空', trigger: 'blur'},
          {max: 50, message: '最多输入50个字符', trigger: 'blur'}
        ],
        url: [
          {validator: validateUrl, trigger: 'blur'},
          {max: 50, message: '最多输入50个字符', trigger: 'blur'}
        ],
        ftpUsername: [
          {validator: validateFtp, trigger: 'blur'},
          {max: 50, message: '最多输入50个字符', trigger: 'blur'}
        ],
        ftpPassword: [
          {validator: validateFtp, trigger: 'blur'},
          {max: 50, message: '最多输入50个字符', trigger: 'blur'}
        ],
        ftpIpAddr: [
          {validator: validateFtp, trigger: 'blur'},
          {max: 50, message: '最多输入50个字符', trigger: 'blur'}
        ],
        ftpDir: [
          {validator: validateFtp, trigger: 'blur'},
          {max: 50, message: '最多输入50个字符', trigger: 'blur'}
        ],
        sdkBrand: [
          {validator: validateSdk, trigger: 'blur'}
        ],
        username: [
          {validator: validateSdk, trigger: 'blur'},
          {max: 50, message: '最多输入50个字符', trigger: 'blur'}
        ],
        password: [
          {validator: validateSdk, trigger: 'blur'},
          {max: 50, message: '最多输入50个字符', trigger: 'blur'}
        ],
        ip: [
          {validator: validateSdk, trigger: 'blur'},
          {max: 50, message: '最多输入50个字符', trigger: 'blur'}
        ],
        port: [
          {validator: validateSdk, trigger: 'blur'},
          {max: 50, message: '最多输入50个字符', trigger: 'blur'}
        ]
      },
      deviceTypeArray: [
        {
          value: 'ftp',
          text: 'FTP'
        },
        {
          value: 'sdk',
          text: 'SDK'
        }
      ],
      sdkBrands: [
        {
          value: 'huimu',
          text: '慧目'
        },
        {
          value: 'haikang',
          text: '海康'
        },
        {
          value: 'dahua',
          text: '大华'
        }
      ]
    }
  }
}
</script>

<style scoped lang="less">
</style>
