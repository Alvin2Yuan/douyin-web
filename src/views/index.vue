<template>
  <page-header-wrapper>
<!--    <template v-slot:content>-->
<!--      <div class="page-header-content">-->
<!--        <div class="avatar">-->
<!--          <a-avatar size="large" :src="avatar"/>-->
<!--        </div>-->
<!--        <div class="content">-->
<!--          <div class="content-title">-->
<!--            {{ timeFix }}，{{ nickname }}<span class="welcome-text"></span>-->
<!--          </div>-->
<!--          <div> {{ postGroup }} | {{ user.dept.deptName }}</div>-->
<!--        </div>-->
<!--      </div>-->
<!--    </template>-->
    <template v-slot:extraContent>
    </template>
    <!-- 致谢项目 -->
    <div>
      <a-row :gutter="24">
        <a-col :xl="16" :lg="24" :md="24" :sm="24" :xs="24">
          <a-card
            class="project-list"
            style="margin-bottom: 24px;"
            :bordered="false"
            title="系统概况"
            :body-style="{ padding: 0 }">
            <a slot="extra">更多</a>
            <div>
              <a-card-grid class="project-card-grid" :key="i" v-for="(item, i) in projects">
                <a-card :bordered="false" :body-style="{ padding: 0 }">
                  <a-card-meta>
                    <div slot="title" class="card-title">
                      <a-avatar size="small" :src="item.logo"/>
                      <a>{{ item.name }}</a>
                    </div>
                    <div slot="description" class="card-description">
                      {{ item.description }}
                    </div>
                  </a-card-meta>
                  <div class="project-item">
<!--                    <a :href="item.website" target="_blank">官网</a>-->
<!--                    <a :href="item.downloadUrl" target="_blank" class="download"><a-icon type="cloud-download" /> 源码下载</a>-->
                  </div>
                </a-card>
              </a-card-grid>
            </div>
          </a-card>
        </a-col>
        <a-col
          style="padding: 0 12px"
          :xl="8"
          :lg="24"
          :md="24"
          :sm="24"
          :xs="24">
          <!-- 导航 -->
          <a-card title="便捷导航" style="margin-bottom: 24px" :bordered="false" :body-style="{padding: 0}">
            <div class="item-group">
              <a href="http://ruoyi.vip/" target="_blank">我的待办</a>
              <a href="https://www.antdv.com/" target="_blank">内容发布</a>
            </div>
          </a-card>
          <!-- Github卡片 -->
<!--          <a-card hoverable style="margin-bottom: 24px">-->
<!--            <a href="https://github.com/fuzui/RuoYi-Antdv" target="_blank" slot="cover">-->
<!--              <img style="width:100%" src="https://github-readme-stats.vercel.app/api/pin/?username=fuzui&repo=RuoYi-Antdv" >-->
<!--            </a>-->
<!--          </a-card>-->
<!--          &lt;!&ndash; Gitee卡片 &ndash;&gt;-->
<!--          <a-card hoverable style="margin-bottom: 24px">-->
<!--            <a href="https://gitee.com/fuzui/RuoYi-Antdv" target="_blank" slot="cover">-->
<!--              <img style="width:100%" src="https://gitee.com/fuzui/RuoYi-Antdv/widgets/widget_card.svg?colors=4183c4,ffffff,ffffff,e3e9ed,666666,9b9b9b" >-->
<!--            </a>-->
<!--          </a-card>-->
        </a-col>
      </a-row>
    </div>
  </page-header-wrapper>
</template>

<script>
import { timeFix } from '@/utils/util'
import { mapGetters } from 'vuex'
import { PageHeaderWrapper } from '@/components/ProLayout'
import ruoyiLogo from '@/assets/projects/ruoyi.png'
import antdvLogo from '@/assets/projects/antdv.png'
import { getUserProfile } from '@/api/system/user'

export default {
  name: 'Index',
  components: {
    PageHeaderWrapper
  },
  data () {
    return {
      // 贡献者
      contributors: [
        {
          name: 'wangze',
          avatar: 'https://portrait.gitee.com/uploads/avatars/user/1662/4988475_fuzui_1586973704.png!avatar60',
          email: '73400@163.com'
        }
      ],
      // 赞助
      sponsorList: [
        {
          key: 'RuoYi',
          tab: 'RuoYi'
        },
        {
          key: 'Antdv',
          tab: 'Antdv'
        }
      ],
      noTitleKey: 'RuoYi',
      timeFix: timeFix(),
      // 用户信息
      user: {
        dept: {
          deptName: ''
        }
      },
      roleGroup: {},
      postGroup: {},
      // 致谢项目
      projects: [
        {
          logo: ruoyiLogo,
          name: '抖音热点',
          description: '查看抖音热点视频',
          website: 'http://ruoyi.vip',
          downloadUrl: 'https://gitee.com/y_project/RuoYi-Vue'
        },
        {
          logo: antdvLogo,
          name: '抖店运营',
          description: '查看抖店运营数据',
          website: 'https://antdv.com',
          downloadUrl: 'https://github.com/vueComponent/ant-design-vue/'
        },
        {
          logo: antdvLogo,
          name: '抖店热销',
          description: '查看抖店热销商品',
          website: 'https://pro.antdv.com',
          downloadUrl: 'https://github.com/vueComponent/ant-design-vue/'
        }
      ]
    }
  },
  computed: {
    ...mapGetters([
      'avatar',
      'nickname'
    ])
  },
  created () {
    this.getUser()
  },
  mounted () {
  },
  methods: {
    // 获取用户信息
    getUser () {
      getUserProfile().then(response => {
        this.user = response.data
        this.roleGroup = response.roleGroup
        this.postGroup = response.postGroup
      })
    },
    onSponsorTabChange (key, type) {
      this[type] = key
    }
  }
}
</script>

<style lang="less" scoped>
  @import "./index.less";
  blockquote {
    padding: 0 1em;
    color: #6a737d;
    border-left: 0.25em solid #dfe2e5;
  }
  .project-list {

    .card-title {
      font-size: 0;

      a {
        color: rgba(0, 0, 0, 0.85);
        margin-left: 12px;
        line-height: 24px;
        height: 24px;
        display: inline-block;
        vertical-align: top;
        font-size: 14px;

        &:hover {
          color: #1890ff;
        }
      }
    }

    .card-description {
      color: rgba(0, 0, 0, 0.45);
      height: 66px;
      line-height: 22px;
      overflow: hidden;
    }

    .project-item {
      display: flex;
      margin-top: 8px;
      overflow: hidden;
      font-size: 12px;
      height: 20px;
      line-height: 20px;

      a {
        color: rgba(0, 0, 0, 0.45);
        display: inline-block;
        flex: 1 1 0;

        &:hover {
          color: #1890ff;
        }
      }

      .download {
        color: rgba(0, 0, 0, 0.25);
        flex: 0 0 auto;
        float: right;
      }
    }

    .ant-card-meta-description {
      color: rgba(0, 0, 0, 0.45);
      height: 44px;
      line-height: 22px;
      overflow: hidden;
    }
  }

  .item-group {
    padding: 20px 0 8px 24px;
    font-size: 0;

    a {
      color: rgba(0, 0, 0, 0.65);
      display: inline-block;
      font-size: 14px;
      margin-bottom: 13px;
      width: 25%;
    }
  }

  .mobile {

    .project-list {

      .project-card-grid {
        width: 100%;
      }
    }

    .more-info {
      border: 0;
      padding-top: 16px;
      margin: 16px 0 16px;
    }

    .headerContent .title .welcome-text {
      display: none;
    }
  }

</style>
