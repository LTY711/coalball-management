<template>
  <div>
    <el-row>
      <div class="nav-title">
        <div class="line-icon"></div>
        <div>订单数据</div>
      </div>
    </el-row>
    <el-row :gutter="20">
      <div class="tabItems">
        <div class="tabItem1">
          <div class="tag-detail">
            <div>订单总数</div>
            <div>{{ orderCount }}</div>
            <div class="tag-bottom">
              <div class="tag-detail-view" v-if="compareOder > 0">
                <div class="tag-detail-add-icon"></div>
                <div class="tag-detail-add">
                  {{ compareOder.toFixed(4) * 100 + "%" }}
                </div>
              </div>
              <div class="tag-detail-view" v-else>
                <div class="tag-detail-down-icon"></div>
                <div class="tag-detail-down">
                  {{ compareOder.toFixed(4) * 100 + "%" }}
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="tabItem2">
          <div class="tag-detail">
            <div>待处理</div>
            <div>{{ willProcessedCount }}</div>
          </div>
        </div>
        <div class="tabItem3">
          <div class="tag-detail">
            <div>已完成</div>
            <div>{{ processedCount }}</div>
          </div>
        </div>
        <div class="tabItem4">
          <div class="tag-detail">
            <div>无效订单</div>
            <div>{{ noUseCount }}</div>
          </div>
        </div>
        <div class="tab-money">
          <div class="tag-detail">
            <div class="money-icon"><i class="el-icon-s-platform"></i></div>
            <div class="tab-money-text">¥{{ moneyCount }}</div>
            <div class="tab-money-bottom">
              <span class="tab-money-bottom-text">较昨日：</span>
              <div class="tag-detail-view" v-if="compareMoney > 0">
                <div class="tag-detail-add">
                  {{ compareMoney.toFixed(4) * 100 + "%" }}
                </div>
                <div class="tag-detail-add-icon"></div>
              </div>
              <div class="tag-detail-view" v-else>
                <div class="tag-detail-down">
                  {{ compareMoney.toFixed(4) * 100 + "%" }}
                </div>
                <div class="tag-detail-down-icon"></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </el-row>
    <el-row>
      <div class="nav-title">
        <div class="line-icon"></div>
        <div>核验订单</div>
      </div>
    </el-row>
    <el-row>
      <div class="hx-oder">
        <div class="hx-oder-view">
          <span class="hx-oder-view-title">核销码:</span>
          <el-input
            v-model="hxOrderInputText"
            class="hxOrderInput"
            placeholder="请输入内容"
          ></el-input>
        </div>
        <el-button type="primary" @click="hxOrder()">核验</el-button>
        <el-button @click="czOrderInput()">重置</el-button>
      </div>
    </el-row>
    <el-row>
      <div class="nav-title">
        <div class="line-icon"></div>
        <div>订单数据</div>
      </div>
    </el-row>
    <el-row>
      <div class="form-view">
        <el-form ref="form" :model="form" label-width="80px">
          <el-form-item label="日期时间">
            <el-col :span="11">
              <el-date-picker
                type="date"
                placeholder="选择日期"
                v-model="form.date1"
                value-format="yyyy-MM-dd"
                style="width: 100%"
              ></el-date-picker>
            </el-col>
            <el-col class="line" :span="2">-</el-col>
            <el-col :span="11">
              <el-date-picker
                type="date"
                placeholder="选择日期"
                v-model="form.date2"
                value-format="yyyy-MM-dd"
                style="width: 100%"
              ></el-date-picker>
            </el-col>
          </el-form-item>
        </el-form>
      </div>
      <el-button type="primary" @click="downData()">下载数据</el-button>
    </el-row>
  </div>
</template>

<script>
import Schart from "vue-schart";
import bus from "../common/bus";
export default {
  name: "oderHandle",
  data() {
    return {
      name: localStorage.getItem("ms_username"),
      orderCount: 199,
      compareOder: 0.12,
      compareMoney: 0.34,
      willProcessedCount: 22,
      processedCount: 170,
      noUseCount: 20,
      moneyCount: 23100,
      hxOrderInputText: "",
      userColor: ["#3D62D9"],
      form: {
        date1: "",
        date2: "",
      },
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
    downData() {
      console.log("downData", this.form);
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
.form-view {
  width: 50%;
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
  width: 200px;
  height: 120px;
  border-radius: 8px;
  margin-right: 20px;
  flex-shrink: 0;
  background-color: #7f5cf2;
}
.tabItem2 {
  width: 200px;
  height: 120px;
  border-radius: 8px;
  margin-right: 20px;
  background-color: #674eff;
}
.tabItem3 {
  width: 200px;
  height: 120px;
  border-radius: 8px;
  margin-right: 20px;
  background-color: #0b93fe;
}
.tabItem4 {
  width: 200px;
  height: 120px;
  border-radius: 8px;
  margin-right: 20px;
  background-color: #5a83eb;
}
.tab-money {
  width: 200px;
  height: 120px;
  border-radius: 8px;
  margin-right: 20px;
  background-color: #fff;
  flex-shrink: 0;
}
.tab-money-bottom {
  display: flex;
}
.tab-money-text {
  color: #0b93fe;
  font-size: 18px;
}
.tab-money-bottom-text {
  color: #999;
  width: 80px;
}
.tag-detail {
  display: flex;
  flex-direction: column;
  align-content: center;
  align-items: center;
  justify-content: space-between;
  font-size: 18px;
  line-height: 40px;
  color: #fff;
  text-align: center;
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
  margin-top: 10px;
  width: 20px;
  height: 20px;
  background-size: 100% 100%;
  background-image: url(../../assets/img/tag-detail-add.png);
}
.tag-detail-down-icon {
  margin-top: 10px;
  width: 20px;
  height: 20px;
  background-size: 100% 100%;
  background-image: url(../../assets/img/tag-detail-down.png);
}
.hx-oder {
  height: 150px;
  border-radius: 8px;
  margin-right: 20px;
  background-color: #fff;
  padding-top: 10px;
  padding-left: 10px;
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

.hx-oder-title {
  margin-top: 5px;
  margin-left: 20px;
  margin-bottom: 50px;
  display: flex;
}
.hxOrderInput {
  margin-right: 5px;
  width: 30%;
  margin-bottom: 20px;
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
.money-icon {
  color: #0b93fe;
  font-size: 30px;
}
.tag-bottom {
  width: 100px;
  height: 40px;
  margin-left: 15px;
  text-align: center;
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
