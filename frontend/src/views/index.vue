<template>
  <div class="content">
    <!--个人中心-->
    <div class="Card ant-card ant-card-bordered">
      <div class="ant-card-head">
        <div class="ant-card-head-wrapper">
          <a-icon type="crown" theme="twoTone" />
          <div class="ant-card-head-title">个人中心</div>
        </div>
      </div>
      <div class="ant-card-body">
        <br />
        <div>
          <p>昵称：{{ remarks }}</p>
          <p>更新时间：{{ timestamp }}</p>
          <p>
            状态：
            <a-icon :type="statuss" theme="twoTone" :two-tone-color="color" />{{
              status == 0 ? " 正常" : " 被禁用"
            }}
          </p>
        </div>
        <br />
        <a-input v-model="wskey" placeholder="请输入新的wskey" />
        <div>
          <br />
          <a-space>
            <a-button type="primary" shape="round" @click="updatewskey">
              更新wskey
            </a-button>
            <a-button type="danger" shape="round" @click="logout">
              退出登录
            </a-button>
            <a-button type="danger" shape="round" @click="remove">
              删除账号
            </a-button>
          </a-space>
        </div>
      </div>
    </div>

    <!--推送卡片-->
    <div class="Card ant-card ant-card-bordered">
      <div class="ant-card-head">
        <div class="ant-card-head-wrapper">
          <a-icon type="pushpin" theme="twoTone" />
          <div class="ant-card-head-title">扫码接收通知</div>
        </div>
      </div>
      <div class="ant-card-body">
        <img class="img" :src="push" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      push: "",
      wskey: "",
      remarks: "",
      timestamp: undefined,
      status: 0
    }
  },
  computed: {
    statuss: function () {
      return this.status == 0 ? 'check-circle' : 'close-circle'
    },
    color: function () {
      return this.status == 0 ? '#52c41a' : '#eb2f96'
    },
  },
  created () {
    const uid = localStorage.getItem('uid')
    if (!uid) {
      this.$router.push('/')
    }
  },
  mounted () {
    this.push = localStorage.getItem('push')
    const uid = localStorage.getItem('uid')
    this.$http.get("api/exitst?uid=" + uid).then((response) => {
      if (response.data.code === 200) {
        const time = response.data.msg
        this.remarks = localStorage.getItem('name')
        this.timestamp = this.dateFormat(time)

        if (!response.data.data) {//如果被禁用 更改状态
          this.status = 1
          this.$message.error("用户被管理员禁用,请联系管理员处理", 3);
        }
      } else {
        this.$message.error("用户被删除,请联系管理员处理", 3);
        localStorage.removeItem('uid')
        this.$router.push('/')
      }
    })
    document.title = 'KingFeng - 个人中心'
  },
  methods: {
    //时间转换
    dateFormat (date) {
      let data = new Date(date)
      let y = data.getFullYear()
      let m = data.getMonth() + 1
      m = m < 10 ? ('0' + m) : m
      let d = data.getDate()
      d = d < 10 ? ('0' + d) : d
      let h = data.getHours()
      h = h < 10 ? ('0' + h) : h
      let M = data.getMinutes()
      M = M < 10 ? ('0' + M) : M
      let s = data.getSeconds()
      s = s < 10 ? ('0' + s) : s
      let dateTime = y + '-' + m + '-' + d + ' ' + h + ':' + M + ':' + s;
      return dateTime;
    },
    //退出
    logout () {
      localStorage.removeItem('uid');
      clearInterval(this.timer) // 清除定时器
      this.$message.success("退出成功", 1);
      setTimeout(() => {
        this.$router.push('/')
      }, 1000)
    },
    //删除账号
    remove () {
      const uid = localStorage.getItem('uid')
      this.$http.delete('api/deleteEnv?uid=' + uid).then(response => {
        if (response.data.code == 200) {
          localStorage.removeItem('uid');
          this.$message.success('删除账号成功', 2)
          this.$router.push('/')
          localStorage.removeItem('name');
        } else {
          this.$message.error('删除失败,请联系管理员处理')
        }
      })
    },
    //更新环境变量
    updatewskey () {
      const pin =
        this.wskey.match(/pin=(.*?);/) && this.wskey.match(/pin=(.*?);/)[1];
      pin == decodeURIComponent(pin);
      const wskey =
        this.wskey.match(/wskey=(.*?);/) && this.wskey.match(/wskey=(.*?);/)[1];
      if (pin && wskey) {
        this.$http.post('api/updateEnv?uid=' + localStorage.getItem('uid') + '&wskey=' + this.wskey).then(response => {
          if (response.data.code == 200) {
            this.wskey = ''
            this.$message.success('更新wskey成功', 2)
          } else {
            this.$message.error('更新失败,请联系管理员处理')
          }
        })
      } else {
        this.$message.error('请检查wskey格式是否正确')
      }

    }
  }
}
</script>

<style>
.content {
  margin: auto;
  width: 91.666667%;
  max-width: 64rem;
}
.img {
  border: 0px solid;
  width: 100%;
  height: 300px;
}
.Card {
  margin: auto;
  margin-top: 1.25rem;
  border-radius: 0.5rem;
  --tw-bg-opacity: 1;
  background-color: rgba(255, 255, 255, var(--tw-bg-opacity));
  --tw-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1),
    0 4px 6px -2px rgba(0, 0, 0, 0.05);
  box-shadow: var(--tw-ring-offset-shadow, 0 0 #0000),
    var(--tw-ring-shadow, 0 0 #0000), var(--tw-shadow);
}
.card-.ant-card-head {
  font-size: 1.125rem;
  line-height: 1.75rem;
  font-weight: 500;
}
.ant-card-body {
  padding: 5px;
  margin: 8px;
  line-height: 15px;
}
.ant-card-head-title {
  font-size: 14px;
  font-weight: bold;
}
</style>