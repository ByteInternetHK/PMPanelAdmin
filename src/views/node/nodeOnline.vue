<template>
  <div>
    <a-row :gutter="24">
      <a-col>
        <a-card :body-style="{padding: '0'}" :bordered="false">
          <s-table ref="table" size="default" :rowKey="(record, index) => index" :columns="ipColumns" :data="loadIpData">
          </s-table>
        </a-card>
      </a-col>
    </a-row>
  </div>
</template>

<script>
import { getNodeInfoByNodeId } from '@/api/node'
import { STable } from '@/components'

export default {
  name: 'NodeOnline',
  components: {
    STable
  },
  data () {
    return {
      info: {},
      ipColumns: [
        {
          title: this.$i18n.t('node.online.userId'),
          align: 'center',
          dataIndex: 'userId'
        },
        {
          title: this.$i18n.t('node.online.ip'),
          align: 'center',
          dataIndex: 'ip'
        },
        {
          title: this.$i18n.t('node.online.time'),
          align: 'center',
          dataIndex: 'time'
        },
        {
          title: this.$i18n.t('node.online.country'),
          align: 'center',
          dataIndex: 'country'
        },
        {
          title: this.$i18n.t('node.online.region'),
          align: 'center',
          dataIndex: 'region'
        },
        {
          title: this.$i18n.t('node.online.city'),
          align: 'center',
          dataIndex: 'city'
        },
        {
          title: this.$i18n.t('node.online.isp'),
          align: 'center',
          dataIndex: 'isp'
        }
      ],
      loadIpData: parameter => {
        return getNodeInfoByNodeId(Object.assign(parameter), this.$route.params.nodeId)
          .then(res => {
            console.log(res.data)
            return res.data
          })
      }
    }
  },
  watch: {
    '$i18n.locale' () {
      this.ipColumns = [
        {
          title: this.$i18n.t('node.online.userId'),
          align: 'center',
          dataIndex: 'userId'
        },
        {
          title: this.$i18n.t('node.online.ip'),
          align: 'center',
          dataIndex: 'ip'
        },
        {
          title: this.$i18n.t('node.online.time'),
          align: 'center',
          dataIndex: 'time'
        },
        {
          title: this.$i18n.t('node.online.country'),
          align: 'center',
          dataIndex: 'country'
        },
        {
          title: this.$i18n.t('node.online.region'),
          align: 'center',
          dataIndex: 'region'
        },
        {
          title: this.$i18n.t('node.online.city'),
          align: 'center',
          dataIndex: 'city'
        },
        {
          title: this.$i18n.t('node.online.isp'),
          align: 'center',
          dataIndex: 'isp'
        }
      ]
    }
  },
  async created () {},
  methods: {}
}
</script>

<style scoped>

</style>
