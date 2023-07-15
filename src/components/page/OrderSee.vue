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
          <span class="hx-oder-view-title">核销码：</span>
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
        <div>GMV月度趋势</div>
      </div>
    </el-row>
  </div>
</template>

<script>
import Schart from "vue-schart";
import bus from "../common/bus";
export default {
  name: "oderSee",
  data() {
    return {
      name: localStorage.getItem("ms_username"),
      orderCount: 199,
      compareOder: 0.12,
      willProcessedCount: 22,
      processedCount: 170,
      noUseCount: 20,
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
  width: 200px;
  height: 150px;
  border-radius: 8px;
  margin-right: 20px;
  flex-shrink: 0;
  background-color: #7f5cf2;
}
.tabItem2 {
  width: 200px;
  height: 150px;
  border-radius: 8px;
  margin-right: 20px;
  background-color: #674eff;
}
.tabItem3 {
  width: 200px;
  height: 150px;
  border-radius: 8px;
  margin-right: 20px;
  background-color: #0b93fe;
}
.tabItem3 {
  width: 200px;
  height: 150px;
  border-radius: 8px;
  margin-right: 20px;
  background-color: #5a83eb;
}
.tag-detail {
  display: flex;
  flex-direction: column;
  align-content: center;
  justify-content: space-between;
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
