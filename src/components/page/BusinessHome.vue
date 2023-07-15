<template>
  <div>
    <el-row :gutter="20">
      <!-- <el-col :span="16">
        <el-row :gutter="20" class="mgb20">
          <el-col :span="8">
            <el-card shadow="hover" :body-style="{ padding: '0px' }">
              <div class="grid-content grid-con-1">
                <i class="el-icon-lx-people grid-con-icon"></i>
                <div class="grid-cont-right">
                  <div class="grid-num">1234</div>
                  <div>用户访问量</div>
                </div>
              </div>
            </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="hover" :body-style="{ padding: '0px' }">
              <div class="grid-content grid-con-2">
                <i class="el-icon-lx-notice grid-con-icon"></i>
                <div class="grid-cont-right">
                  <div class="grid-num">321</div>
                  <div>系统消息</div>
                </div>
              </div>
            </el-card>
          </el-col>
          <el-col :span="8">
            <el-card shadow="hover" :body-style="{ padding: '0px' }">
              <div class="grid-content grid-con-3">
                <i class="el-icon-lx-goods grid-con-icon"></i>
                <div class="grid-cont-right">
                  <div class="grid-num">5000</div>
                  <div>数量</div>
                </div>
              </div>
            </el-card>
          </el-col>
        </el-row>
      </el-col> -->
      <div class="tabItems">
        <div class="tabItem1">
          <div class="tag-view"><i class="el-icon-price-tag"></i></div>
          <div class="tag-detail">
            <div class="tag-left">
              <div class="tag-detail-top">销售额</div>
              <div class="tag-detail-bottom">¥ {{ salesVolume }}</div>
            </div>
            <div class="tag-center">
              <div class="tag-detail-top">较昨日</div>
              <div class="tag-detail-bottom">¥ {{ lastdayVolume }}</div>
            </div>
            <div class="tag-right">
              <div class="tag-detail-view" v-if="lastdayVolume > 0">
                <div class="tag-detail-add-icon"></div>
                <div class="tag-detail-add">
                  {{ (lastdayVolume / salesVolume).toFixed(4) * 100 + "%" }}
                </div>
              </div>
              <div class="tag-detail-view" v-else>
                <div class="tag-detail-down-icon"></div>
                <div class="tag-detail-down">
                  {{ (lastdayVolume / salesVolume).toFixed(4) * 100 + "%" }}
                </div>
              </div>
              <div class="tag-detail-grade"></div>
            </div>
          </div>
        </div>
        <div class="tabItem2">
          <div class="tag-view"><i class="el-icon-s-custom"></i></div>
          <div class="tag-detail">
            <div class="tag-left">
              <div class="tag-detail-top">客户</div>
              <div class="tag-detail-bottom">{{ salesPerson }}</div>
            </div>
            <div class="tag-center">
              <div class="tag-detail-top">较昨日</div>
              <div class="tag-detail-bottom">{{ lastdayperson }}</div>
            </div>
            <div class="tag-right">
              <div class="tag-detail-view" v-if="lastdayperson > 0">
                <div class="tag-detail-add-icon"></div>
                <div class="tag-detail-add">
                  {{ (lastdayperson / salesPerson).toFixed(4) * 100 + "%" }}
                </div>
              </div>
              <div class="tag-detail-view" v-else>
                <div class="tag-detail-down-icon"></div>
                <div class="tag-detail-down">
                  {{ (lastdayperson / salesPerson).toFixed(4) * 100 + "%" }}
                </div>
              </div>
              <div class="tag-detail-grade"></div>
            </div>
          </div>
        </div>
        <div class="tabItem3">
          <div class="tag-view"><i class="el-icon-s-custom"></i></div>
          <div class="tag-detail">
            <div class="tag-left">
              <div class="tag-detail-top">今日订单</div>
              <div class="tag-detail-bottom">{{ todayOrder }}</div>
            </div>
            <div class="tag-center">
              <div class="tag-detail-top">较昨日</div>
              <div class="tag-detail-bottom">{{ lastdayOrder }}</div>
            </div>
            <div class="tag-right">
              <div class="tag-detail-view" v-if="lastdayOrder > 0">
                <div class="tag-detail-add-icon"></div>
                <div class="tag-detail-add">
                  {{ (lastdayOrder / todayOrder).toFixed(4) * 100 + "%" }}
                </div>
              </div>
              <div class="tag-detail-view" v-else>
                <div class="tag-detail-down-icon"></div>
                <div class="tag-detail-down">
                  {{ (lastdayOrder / todayOrder).toFixed(4) * 100 + "%" }}
                </div>
              </div>
              <div class="tag-detail-grade"></div>
            </div>
          </div>
        </div>
        <div class="hx-oder">
          <div class="hx-oder-title">
            <div class="line-icon"></div>
            <div>核验订单</div>
          </div>
          <div class="hx-oder-view">
            <span class="hx-oder-view-title">核销码：</span>
            <el-input
              v-model="hxOrderInputText"
              class="hxOrderInput"
              placeholder="请输入内容"
            ></el-input>
            <el-button type="primary" @click="hxOrder()">核验</el-button>
            <el-button @click="czOrderInput()">重置</el-button>
          </div>
        </div>
      </div>
    </el-row>
    <el-row>
      <div class="nav-title">
        <div class="line-icon"></div>
        <div>GMV月度趋势</div>
      </div>
    </el-row>
    <el-row :gutter="12">
      <el-col :span="12">
        <el-card shadow="hover">
          <h4>销售趋势图</h4>
          <ve-line :data="chartData1" :colors="userColor"></ve-line>
        </el-card>
      </el-col>
      <el-col :span="12">
        <el-card shadow="hover">
          <h4>客户趋势图</h4>
          <ve-line :data="chartData2"></ve-line>
        </el-card>
      </el-col>
    </el-row>
    <el-row>
      <div class="nav-view">
        <div class="nav-title">
          <div class="line-icon"></div>
          <div>店铺推广</div>
        </div>
        <div class="look-more">查看更多 ></div>
      </div>
    </el-row>
    <el-row>
      <div class="money-icon-view">
        <div class="money-icon"></div>
        <div class="money-icon-right" @click="goCreate()"></div>
      </div>
    </el-row>
  </div>
</template>

<script>
import Schart from "vue-schart";
import bus from "../common/bus";
export default {
  name: "BusinessHome",
  data() {
    return {
      name: localStorage.getItem("ms_username"),
      todoList: [
        {
          title: "今天要修复1000个bug",
          status: false,
        },
        {
          title: "今天要修复100个bug",
          status: false,
        },
        {
          title: "今天要写100行代码加几个bug吧",
          status: false,
        },
        {
          title: "今天要修复100个bug",
          status: false,
        },
        {
          title: "今天要修复100个bug",
          status: true,
        },
        {
          title: "今天要写100行代码加几个bug吧",
          status: true,
        },
      ],
      data: [
        {
          name: "2018/09/04",
          value: 1083,
        },
        {
          name: "2018/09/05",
          value: 941,
        },
        {
          name: "2018/09/06",
          value: 1139,
        },
        {
          name: "2018/09/07",
          value: 816,
        },
        {
          name: "2018/09/08",
          value: 327,
        },
        {
          name: "2018/09/09",
          value: 228,
        },
        {
          name: "2018/09/10",
          value: 1065,
        },
      ],
      options: {
        type: "bar",
        title: {
          text: "最近一周各品类销售图",
        },
        xRorate: 25,
        labels: ["周一", "周二", "周三", "周四", "周五"],
        datasets: [
          {
            label: "家电",
            data: [234, 278, 270, 190, 230],
          },
          {
            label: "百货",
            data: [164, 178, 190, 135, 160],
          },
          {
            label: "食品",
            data: [144, 198, 150, 235, 120],
          },
        ],
      },
      chartData1: {
        color: "red",
        columns: ["日期", "销售额"],
        rows: [
          { 日期: "08.01", 销售额: 1393 },
          { 日期: "08.05", 销售额: 3530 },
          { 日期: "08.10", 销售额: 2923 },
          { 日期: "08.15", 销售额: 1723 },
          { 日期: "08.20", 销售额: 3792 },
          { 日期: "08.25", 销售额: 4593 },
        ],
      },
      chartData2: {
        columns: ["日期", "客户"],
        rows: [
          { 日期: "08.01", 客户: 393 },
          { 日期: "08.05", 客户: 530 },
          { 日期: "08.10", 客户: 923 },
          { 日期: "08.15", 客户: 723 },
          { 日期: "08.20", 客户: 792 },
          { 日期: "08.25", 客户: 593 },
        ],
      },
      salesVolume: 12312,
      lastdayVolume: 342,
      salesPerson: 1900,
      lastdayperson: -20,
      todayOrder: 120,
      lastdayOrder: -30,
      hxOrderInputText: "",
      userColor: ["#3D62D9"],
    };
  },
  components: {
    Schart,
  },
  computed: {
    role() {
      return this.name === "admin" ? "超级管理员" : "普通用户";
    },
  },
  // created() {
  //     this.handleListener();
  //     this.changeDate();
  // },
  // activated() {
  //     this.handleListener();
  // },
  // deactivated() {
  //     window.removeEventListener('resize', this.renderChart);
  //     bus.$off('collapse', this.handleBus);
  // },
  methods: {
    changeDate() {
      const now = new Date().getTime();
      this.data.forEach((item, index) => {
        const date = new Date(now - (6 - index) * 86400000);
        item.name = `${date.getFullYear()}/${
          date.getMonth() + 1
        }/${date.getDate()}`;
      });
    },
    goCreate() {
      console.log("goCreate");
    },
    hxOrder() {
      console.log("hxOrder", this.hxOrderInputText);
    },
    czOrderInput() {
      this.hxOrderInputText = "";
      console.log("czOrderInput");
    },
    // handleListener() {
    //     bus.$on('collapse', this.handleBus);
    //     // 调用renderChart方法对图表进行重新渲染
    //     window.addEventListener('resize', this.renderChart);
    // },
    // handleBus(msg) {
    //     setTimeout(() => {
    //         this.renderChart();
    //     }, 200);
    // },
    // renderChart() {
    //     this.$refs.bar.renderChart();
    //     this.$refs.line.renderChart();
    // }
  },
};
</script>


<style scoped>
.el-row {
  margin-bottom: 20px;
}

.grid-content {
  display: flex;
  align-items: center;
  height: 100px;
}
.tabItems {
  display: flex;
  margin-left: 10px;
  justify-content: space-between;
}
.tabItem1 {
  width: 300px;
  height: 150px;
  border-radius: 8px;
  margin-right: 20px;
  flex-shrink: 0;
  background-color: #3f65e0;
}
.tabItem2 {
  width: 300px;
  height: 150px;
  border-radius: 8px;
  margin-right: 20px;
  background-color: #1dd2b6;
}
.tabItem3 {
  width: 300px;
  height: 150px;
  border-radius: 8px;
  margin-right: 20px;
  background-color: #6ecafa;
}
.tag-detail {
  display: flex;
  justify-content: space-between;
  padding-right: 20px;
}
.tag-detail-top {
  font-size: 14px;
  color: white;
  margin-left: 20px;
}
.tag-detail-bottom {
  margin-left: 20px;
  font-size: 24px;
  color: white;
}
.tag-view {
  margin-top: 10px;
  width: 40px;
  height: 40px;
  font-size: 30px;
  margin-left: 20px;
  color: #fff;
  margin-bottom: 20px;
}
.tag-right {
  margin-left: 10px;
  padding-top: 25px;
}
.tag-detail-view {
  display: flex;
}
.tag-detail-add {
  margin-left: 5px;
  color: #1afa29;
}
.tag-detail-down {
  margin-left: 5px;
  color: red;
}
.tag-detail-add-icon {
  width: 20px;
  height: 20px;
  background-size: 100% 100%;
  background-image: url(../../assets/img/tag-detail-add.png);
}
.tag-detail-down-icon {
  width: 20px;
  height: 20px;
  background-size: 100% 100%;
  background-image: url(../../assets/img/tag-detail-down.png);
}
.hx-oder {
  width: auto;
  height: 150px;
  border-radius: 8px;
  margin-right: 20px;
  background-color: #fff;
}
.hx-oder-view {
  display: flex;
  font-size: 12px;
  margin-right: 10px;
  margin-left: 10px;
}
.hx-oder-view-title {
  flex-shrink: 0;
  line-height: 36px;
}
.line-icon {
  width: 10px;
  height: 25px;
  background-size: 100% 100%;
  margin-right: 5px;
  background-image: url(../../assets/img/line.png);
}
.money-icon-view {
  display: flex;
  background-color: #f5f7fd;
  width: auto;
  height: 64px;
  border-radius: 5px;
  padding-left: 10px;
  padding-right: 10px;
  justify-content: space-between;
}
.money-icon {
  width: 400px;
  height: 64px;
  background-size: 100% 100%;
  background-image: url(../../assets/img/money-img.png);
}
.money-icon-right {
  width: 80px;
  height: 64px;
  background-size: 100% 100%;
  background-image: url(../../assets/img/money-img-right.png);
}

.hx-oder-title {
  margin-top: 5px;
  margin-left: 20px;
  margin-bottom: 50px;
  display: flex;
}
.hxOrderInput {
  margin-right: 5px;
}
.look-more {
  font-size: 12px;
  color: #999;
}
.nav-view {
  display: flex;
  justify-content: space-between;
  padding-left: 20px;
  padding-right: 20px;
}
.nav-title {
  display: flex;
}
.grid-cont-right {
  flex: 1;
  text-align: center;
  font-size: 14px;
  color: #999;
}

.grid-num {
  font-size: 30px;
  font-weight: bold;
}

.grid-con-icon {
  font-size: 50px;
  width: 100px;
  height: 100px;
  text-align: center;
  line-height: 100px;
  color: #fff;
}

.grid-con-1 .grid-con-icon {
  background: rgb(45, 140, 240);
}

.grid-con-1 .grid-num {
  color: rgb(45, 140, 240);
}

.grid-con-2 .grid-con-icon {
  background: rgb(100, 213, 114);
}

.grid-con-2 .grid-num {
  color: rgb(45, 140, 240);
}

.grid-con-3 .grid-con-icon {
  background: rgb(242, 94, 67);
}

.grid-con-3 .grid-num {
  color: rgb(242, 94, 67);
}

.user-info {
  display: flex;
  align-items: center;
  padding-bottom: 20px;
  border-bottom: 2px solid #ccc;
  margin-bottom: 20px;
}

.user-avator {
  width: 120px;
  height: 120px;
  border-radius: 50%;
}

.user-info-cont {
  padding-left: 50px;
  flex: 1;
  font-size: 14px;
  color: #999;
}

.user-info-cont div:first-child {
  font-size: 30px;
  color: #222;
}

.user-info-list {
  font-size: 14px;
  color: #999;
  line-height: 25px;
}

.user-info-list span {
  margin-left: 70px;
}

.mgb20 {
  margin-bottom: 20px;
}

.todo-item {
  font-size: 14px;
}

.todo-item-del {
  text-decoration: line-through;
  color: #999;
}

.schart {
  width: 100%;
  height: 300px;
}
</style>
