<template>
  <div class="account-settings-info-view">
    <a-row :gutter="16">
      <a-col :md="24" :lg="24">
        <div v-if="loading" style="text-align: center;margin: 40px 0">
          <a-spin size="large" tip="Loading..." :spinning="loading"/>
        </div>
        <a-form v-else layout="vertical">
          <a-form-item :label="$t('settings.site.siteName')">
            <a-input v-model="siteConfig.siteName" @blur="updateConfigByName('siteName', siteConfig.siteName)"/>
          </a-form-item>

          <a-form-item :label="$t('settings.site.siteUrl')">
            <a-input v-model="siteConfig.siteUrl" @blur="updateConfigByName('siteUrl', siteConfig.siteUrl)"/>
          </a-form-item>

          <a-form-item :label="$t('settings.site.subUrl')">
            <a-input v-model="siteConfig.subUrl" @blur="updateConfigByName('subUrl', siteConfig.subUrl)"/>
          </a-form-item>

          <a-divider />

          <a-form-item :label="$t('settings.site.regEnable')" style="display: inline-block; margin-right: 20px">
            <a-switch v-model="siteConfig.regEnable" @change="updateConfigByName('regEnable', siteConfig.regEnable)">
              <a-icon slot="checkedChildren" type="check" />
              <a-icon slot="unCheckedChildren" type="close" />
            </a-switch>
          </a-form-item>

          <a-form-item :label="$t('settings.site.inviteOnly')" style="display: inline-block; margin-right: 20px">
            <a-switch v-model="siteConfig.inviteOnly" @change="updateConfigByName('inviteOnly', siteConfig.inviteOnly)">
              <a-icon slot="checkedChildren" type="check" />
              <a-icon slot="unCheckedChildren" type="close" />
            </a-switch>
          </a-form-item>

          <a-form-item :label="$t('settings.site.mailRegEnable')" style="display: inline-block; margin-right: 20px">
            <a-switch v-model="siteConfig.mailRegEnable" @change="updateConfigByName('mailRegEnable', siteConfig.mailRegEnable)">
              <a-icon slot="checkedChildren" type="check" />
              <a-icon slot="unCheckedChildren" type="close" />
            </a-switch>
          </a-form-item>

          <a-form-item :label="$t('settings.site.mailLimit')">
            <a-input v-model="siteConfig.mailLimit" @blur="updateConfigByName('mailLimit', siteConfig.mailLimit)"/>
          </a-form-item>

          <a-form-item :label="$t('settings.site.mailType')">
            <a-select :value="siteConfig.mailType" @change="handleSelectMailType">
              <a-select-option value="smtp">SMTP</a-select-option>
              <a-select-option value="postalAPI">PostalAPI</a-select-option>
              <a-select-option value="aliyunAPI">AliyunAPI</a-select-option>
              <a-select-option value="mailgunAPI">MailgunAPI</a-select-option>
            </a-select>
          </a-form-item>

          <a-form-item v-if="siteConfig.mailType === 'smtp' " :label="$t('settings.site.mailConfig')">
            <a-input v-model="siteConfig.mailConfig.host" @blur="updateConfigByName('mailConfig', siteConfig.mailConfig)" placeholder="Host" /><br><br>
            <span>SSL </span>
            <a-switch v-model="siteConfig.mailConfig.ssl" @change="updateConfigByName('mailConfig', siteConfig.mailConfig)">
              <a-icon slot="checkedChildren" type="check" />
              <a-icon slot="unCheckedChildren" type="close" />
            </a-switch><br><br>
            <a-input v-model="siteConfig.mailConfig.port" @blur="updateConfigByName('mailConfig', siteConfig.mailConfig)" placeholder="Port" /><br><br>
            <a-input v-model="siteConfig.mailConfig.username" @blur="updateConfigByName('mailConfig', siteConfig.mailConfig)" placeholder="Username" /><br><br>
            <a-input v-model="siteConfig.mailConfig.password" @blur="updateConfigByName('mailConfig', siteConfig.mailConfig)" placeholder="Password" /><br><br>
            <a-input v-model="siteConfig.mailConfig.from" @blur="updateConfigByName('mailConfig', siteConfig.mailConfig)" placeholder="From" /><br><br>
          </a-form-item>
          <a-form-item v-if="siteConfig.mailType === 'postalAPI' " :label="$t('settings.site.mailConfig')">
            <a-input v-model="siteConfig.mailConfig.host" @blur="updateConfigByName('mailConfig', siteConfig.mailConfig)" placeholder="Host" /><br><br>
            <a-input v-model="siteConfig.mailConfig.apiKey" @blur="updateConfigByName('mailConfig', siteConfig.mailConfig)" placeholder="ApiKey" /><br><br>
            <a-input v-model="siteConfig.mailConfig.username" @blur="updateConfigByName('mailConfig', siteConfig.mailConfig)" placeholder="Username" /><br><br>
          </a-form-item>
          <a-form-item v-if="siteConfig.mailType === 'aliyunAPI' " :label="$t('settings.site.mailConfig')">
            <a-input v-model="siteConfig.mailConfig.accessKey" @blur="updateConfigByName('mailConfig', siteConfig.mailConfig)" placeholder="AccessKey" /><br><br>
            <a-input v-model="siteConfig.mailConfig.accessSecret" @blur="updateConfigByName('mailConfig', siteConfig.mailConfig)" placeholder="AccessSecret" /><br><br>
            <a-input v-model="siteConfig.mailConfig.accountName" @blur="updateConfigByName('mailConfig', siteConfig.mailConfig)" placeholder="AccountName" /><br><br>
            <a-input v-model="siteConfig.mailConfig.alias" @blur="updateConfigByName('mailConfig', siteConfig.mailConfig)" placeholder="Alias" /><br><br>
          </a-form-item>
          <a-form-item v-if="siteConfig.mailType === 'mailgunAPI' " :label="$t('settings.site.mailConfig')">
            <a-input v-model="siteConfig.mailConfig.key" @blur="updateConfigByName('mailConfig', siteConfig.mailConfig)" placeholder="Key" /><br><br>
            <a-input v-model="siteConfig.mailConfig.domain" @blur="updateConfigByName('mailConfig', siteConfig.mailConfig)" placeholder="Domain" /><br><br>
            <a-input v-model="siteConfig.mailConfig.sender" @blur="updateConfigByName('mailConfig', siteConfig.mailConfig)" placeholder="Sender" /><br><br>
          </a-form-item>

          <a-form-item :label="$t('settings.site.notifyMailType')">
            <a-select :value="siteConfig.notifyMailType" @change="handleSelectNotifyMailType">
              <a-select-option value="smtp">SMTP</a-select-option>
              <a-select-option value="postalAPI">PostalAPI</a-select-option>
              <a-select-option value="aliyunAPI">AliyunAPI</a-select-option>
              <a-select-option value="mailgunAPI">MailgunAPI</a-select-option>
            </a-select>
          </a-form-item>

          <a-form-item v-if="siteConfig.notifyMailType === 'smtp' " :label="$t('settings.site.notifyMailConfig')">
            <a-input v-model="siteConfig.notifyMailConfig.host" @blur="updateConfigByName('notifyMailConfig', siteConfig.notifyMailConfig)" placeholder="Host" /><br><br>
            <span>SSL </span>
            <a-switch v-model="siteConfig.notifyMailConfig.ssl" @change="updateConfigByName('notifyMailConfig', siteConfig.notifyMailConfig)">
              <a-icon slot="checkedChildren" type="check" />
              <a-icon slot="unCheckedChildren" type="close" />
            </a-switch><br><br>
            <a-input v-model="siteConfig.notifyMailConfig.port" @blur="updateConfigByName('notifyMailConfig', siteConfig.notifyMailConfig)" placeholder="Port" /><br><br>
            <a-input v-model="siteConfig.notifyMailConfig.username" @blur="updateConfigByName('notifyMailConfig', siteConfig.notifyMailConfig)" placeholder="Username" /><br><br>
            <a-input v-model="siteConfig.notifyMailConfig.password" @blur="updateConfigByName('notifyMailConfig', siteConfig.notifyMailConfig)" placeholder="Password" /><br><br>
            <a-input v-model="siteConfig.notifyMailConfig.from" @blur="updateConfigByName('notifyMailConfig', siteConfig.notifyMailConfig)" placeholder="From" /><br><br>
          </a-form-item>
          <a-form-item v-if="siteConfig.notifyMailType === 'postalAPI' " :label="$t('settings.site.notifyMailConfig')">
            <a-input v-model="siteConfig.notifyMailConfig.host" @blur="updateConfigByName('notifyMailConfig', siteConfig.notifyMailConfig)" placeholder="Host" /><br><br>
            <a-input v-model="siteConfig.notifyMailConfig.apiKey" @blur="updateConfigByName('notifyMailConfig', siteConfig.notifyMailConfig)" placeholder="ApiKey" /><br><br>
            <a-input v-model="siteConfig.notifyMailConfig.username" @blur="updateConfigByName('notifyMailConfig', siteConfig.notifyMailConfig)" placeholder="Username" /><br><br>
          </a-form-item>
          <a-form-item v-if="siteConfig.notifyMailType === 'aliyunAPI' " :label="$t('settings.site.notifyMailConfig')">
            <a-input v-model="siteConfig.notifyMailConfig.accessKey" @blur="updateConfigByName('notifyMailConfig', siteConfig.notifyMailConfig)" placeholder="AccessKey" /><br><br>
            <a-input v-model="siteConfig.notifyMailConfig.accessSecret" @blur="updateConfigByName('notifyMailConfig', siteConfig.notifyMailConfig)" placeholder="AccessSecret" /><br><br>
            <a-input v-model="siteConfig.notifyMailConfig.accountName" @blur="updateConfigByName('notifyMailConfig', siteConfig.notifyMailConfig)" placeholder="AccountName" /><br><br>
            <a-input v-model="siteConfig.notifyMailConfig.alias" @blur="updateConfigByName('notifyMailConfig', siteConfig.notifyMailConfig)" placeholder="Alias" /><br><br>
          </a-form-item>
          <a-form-item v-if="siteConfig.notifyMailType === 'mailgunAPI' " :label="$t('settings.site.notifyMailConfig')">
            <a-input v-model="siteConfig.notifyMailConfig.key" @blur="updateConfigByName('notifyMailConfig', siteConfig.notifyMailConfig)" placeholder="Key" /><br><br>
            <a-input v-model="siteConfig.notifyMailConfig.domain" @blur="updateConfigByName('notifyMailConfig', siteConfig.notifyMailConfig)" placeholder="Domain" /><br><br>
            <a-input v-model="siteConfig.notifyMailConfig.sender" @blur="updateConfigByName('notifyMailConfig', siteConfig.notifyMailConfig)" placeholder="Sender" /><br><br>
          </a-form-item>
          <a-form-item :label="$t('settings.site.enableNotifyRenew')" style="display: inline-block; margin-right: 20px">
            <a-switch v-model="siteConfig.enableNotifyRenew" @change="updateConfigByName('enableNotifyRenew', siteConfig.enableNotifyRenew)">
              <a-icon slot="checkedChildren" type="check" />
              <a-icon slot="unCheckedChildren" type="close" />
            </a-switch>
          </a-form-item>
        </a-form>
      </a-col>
    </a-row>
  </div>
</template>

<script>
import { getSiteConfig, updateConfigByName } from '@/api/settings'

export default {
  data () {
    return {
      loading: true,
      siteConfig: {}
    }
  },
  async created () {
    await getSiteConfig().then(res => {
      this.siteConfig = res.data.siteConfig
      this.loading = false
    })
  },
  methods: {
    async updateConfigByName (name, value) {
      // 如果是mailConfig, 转成json字符串
      if (name === 'mailConfig' || name === 'notifyMailConfig') {
        value = JSON.stringify(value)
      }
      const params = {
        'name': name,
        'value': value
      }
      const result = await updateConfigByName(params)
      if (result.code === 200) {
        this.$i18n.locale === 'zh-CN' ? this.$message.success(result.message) : this.$message.success(result.messageEnglish)
      }
    },
    handleSelectMailType (value) {
      this.siteConfig.mailConfig = {
        ssl: false
      }
      this.siteConfig.mailType = value
      this.updateConfigByName('mailType', value)
    },
    handleSelectNotifyMailType (value) {
      this.siteConfig.notifyMailConfig = {
        ssl: false
      }
      this.siteConfig.notifyMailType = value
      this.updateConfigByName('notifyMailType', value)
    }
  }
}
</script>

<style lang="less" scoped>
</style>
