<template>
  <div class="page">
    <div class="gva-card-box">
      <div class="gva-card gva-top-card">
        <div class="gva-top-card-left">
          <div class="gva-top-card-left-title">您好，管理员，请开始一天的工作吧</div>
          <div class="gva-top-card-left-dot">{{ weatherInfo }}</div>
          <div v-show="false" class="gva-top-card-left-rows">
            <el-row>
              <el-col :span="8" :xs="24" :sm="8">
                <div class="flex-center">
                  <el-icon class="dashboard-icon">
                    <sort/>
                  </el-icon>
                  今日流量 (1231231)
                </div>
              </el-col>
              <el-col :span="8" :xs="24" :sm="8">
                <div class="flex-center">
                  <el-icon class="dashboard-icon">
                    <avatar/>
                  </el-icon>
                  总用户数 (24001)
                </div>
              </el-col>
              <el-col :span="8" :xs="24" :sm="8">
                <div class="flex-center">
                  <el-icon class="dashboard-icon">
                    <comment/>
                  </el-icon>
                  好评率 (99%)
                </div>
              </el-col>
            </el-row>
          </div>
        </div>
        <img src="@/assets/dashboard.png" class="gva-top-card-right" alt>
      </div>
    </div>
    <div class="gva-card-box">
      <el-card class="gva-card quick-entrance">
        <template #header>
          <div class="card-header">
            <span>快捷入口</span>
          </div>
        </template>
        <el-row :gutter="20">
          <el-col
              v-for="(card, key) in toolCards"
              :key="key"
              :span="4"
              :xs="8"
              class="quick-entrance-items"
              @click="toTarget(card.name)"
          >
            <div class="quick-entrance-item">
              <div class="quick-entrance-item-icon" :style="{ backgroundColor: card.bg }">
                <el-icon>
                  <component :is="card.icon" :style="{ color: card.color }"/>
                </el-icon>
              </div>
              <p>{{ card.label }}</p>
            </div>
          </el-col>
        </el-row>
      </el-card>
      <!-- <div class="quick-entrance-title"></div> -->
    </div>
    <div class="gva-card-box">
      <div class="gva-card">
        <div class="card-header">
          <span>数据统计</span>
        </div>
        <div class="echart-box">
          <el-row :gutter="20">
            <el-col :xs="24" :sm="18">
              <Echarts用户统计/>
            </el-col>
            <el-col :xs="24" :sm="6">
              <Echarts在线统计/>
            </el-col>
          </el-row>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import Echarts用户统计 from './组件/用户账号统计统计折线.vue'
import Echarts在线统计 from './组件/在线用户统计饼图.vue'
// import DashboardTable from '@/view/dashboard/dashboardTable/dashboardTable.vue'
import {onMounted, ref} from 'vue'
import {useRouter} from 'vue-router'
import {get高德天气接口} from "@/api/分析页Api.js";

const weatherInfo = ref("天气查询中....")

const toolCards = ref([
  {
    label: '在线用户',
    icon: 'monitor',
    name: '在线用户',
    color: '#ff9c6e',
    bg: 'rgba(255, 156, 110,.3)'
  },
  {
    label: '应用列表',
    icon: 'SwitchFilled',
    name: '应用列表',
    color: '#69c0ff',
    bg: 'rgba(105, 192, 255,.3)'
  },
  {
    label: '用户消息',
    icon: 'ChatLineSquare',
    name: '用户消息',
    color: '#b37feb',
    bg: 'rgba(179, 127, 235,.3)'
  },
  {
    label: '卡号列表',
    icon: 'Tickets',
    name: '卡号列表',
    color: '#ffd666',
    bg: 'rgba(255, 214, 102,.3)'
  },
  {
    label: '充值订单',
    icon: 'document-checked',
    name: '支付充值订单',
    color: '#ff85c0',
    bg: 'rgba(255, 133, 192,.3)'
  },
  {
    label: '个人中心',
    icon: 'user',
    name: '个人中心',
    color: '#5cdbd3',
    bg: 'rgba(92, 219, 211,.3)'
  }
])

const router = useRouter()

const toTarget = (name) => {
  router.push({name})
}

onMounted(() => {
  get高德天气接口().then(res=>{
    weatherInfo.value = res.data
  })
})
//获取菜单全部项目,随机获取4个



</script>
<script>
export default {
  name: 'Dashboard'
}
</script>

<style lang="scss" scoped>
@mixin flex-center {
  display: flex;
  align-items: center;
}

.page {
  background: #f0f2f5;
  padding: 0;

  .gva-card-box {
    padding: 12px 16px;

    & + .gva-card-box {
      padding-top: 0px;
    }
  }

  .gva-card {
    box-sizing: border-box;
    background-color: #fff;
    border-radius: 2px;
    height: auto;
    padding: 26px 30px;
    overflow: hidden;
    box-shadow: 0 0 7px 1px rgba(0, 0, 0, 0.03);
  }

  .gva-top-card {
    height: 260px;
    @include flex-center;
    justify-content: space-between;
    color: #777;

    &-left {
      height: 100%;
      display: flex;
      flex-direction: column;

      &-title {
        font-size: 22px;
        color: #343844;
      }

      &-dot {
        font-size: 16px;
        color: #6B7687;
        margin-top: 24px;
      }

      &-rows {
        // margin-top: 15px;
        margin-top: 18px;
        color: #6B7687;
        width: 600px;
        align-items: center;
      }

      &-item {
        + .gva-top-card-left-item {
          margin-top: 24px;
        }

        margin-top: 14px;
      }
    }

    &-right {
      height: 600px;
      width: 600px;
      margin-top: 28px;
    }
  }

  ::v-deep(.el-card__header) {
    padding: 0;
    border-bottom: none;
  }

  .card-header {
    padding-bottom: 20px;
    border-bottom: 1px solid #e8e8e8;
  }

  .quick-entrance-title {
    height: 30px;
    font-size: 22px;
    color: #333;
    width: 100%;
    border-bottom: 1px solid #eee;
  }

  .quick-entrance-items {
    @include flex-center;
    justify-content: center;
    text-align: center;
    color: #333;

    .quick-entrance-item {
      padding: 16px 28px;
      margin-top: -16px;
      margin-bottom: -16px;
      border-radius: 4px;
      transition: all 0.2s;

      &:hover {
        box-shadow: 0px 0px 7px 0px rgba(217, 217, 217, 0.55);
      }

      cursor: pointer;
      height: auto;
      text-align: center;
      // align-items: center;
      &-icon {
        width: 50px;
        height: 50px !important;
        border-radius: 8px;
        @include flex-center;
        justify-content: center;
        margin: 0 auto;

        i {
          font-size: 24px;
        }
      }

      p {
        margin-top: 10px;
      }
    }
  }

  .echart-box {
    padding: 14px;
  }
}

.dashboard-icon {
  font-size: 20px;
  color: rgb(85, 160, 248);
  width: 30px;
  height: 30px;
  margin-right: 10px;
  @include flex-center;
}

.flex-center {
  @include flex-center;
}

//小屏幕不显示右侧，将登录框居中
@media (max-width: 750px) {
  .gva-card {
    padding: 20px 10px !important;

    .gva-top-card {
      height: auto;

      &-left {
        &-title {
          font-size: 20px !important;
        }

        &-rows {
          margin-top: 15px;
          align-items: center;
        }
      }

      &-right {
        display: none;
      }
    }

    .gva-middle-card {
      &-item {
        line-height: 20px;
      }
    }

    .dashboard-icon {
      font-size: 18px;
    }
  }
}
</style>
