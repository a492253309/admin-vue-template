<template>
  <div>
    <div class="gva-search-box">
      <el-form :model="searchInfo" :inline="true">
        <el-row>
          <el-form-item prop="游戏" style="width: 230px"  :rules="[{ required: true, message: '请选择游戏', trigger: 'change' }]">
            <el-select v-model="searchInfo.region_id" placeholder="游戏">
              <el-option
                v-for="(game, index) in gameOptions"
                :key="index"
                :value="game[0]"
                :label="game[1]"
              />
            </el-select>
          </el-form-item>
          <el-form-item label="CDK" prop="cdk">
            <el-input v-model="searchInfo.cdk"></el-input>
          </el-form-item>
          <el-button type="primary" @click="goQuery">查询</el-button>
        </el-row>
      </el-form>
    </div>
    <div>
    </div>
    <div class="gva-table-box">
      <vue-json-pretty :data="data" />
      <el-table :data="jsonData" >
        <el-table-column  v-for="column in tableColumns" :key="column.prop" :prop="column.prop" :label="column.label" :formatter="formatDate" >
          <template v-slot="scope">
            <!-- 判断字段是否是对象类型 -->
            <span v-if="typeof scope.row[column.prop] === 'object'">{{ scope.row[column.prop].property }}
              <span >{{ scope.row[column.prop] }}</span>
                </span>
          </template>
        </el-table-column>
      </el-table>
      <el-row style="margin: 0; padding: 0; justify-content: end;">
        <el-pagination layout="total, sizes, prev, pager, next, jumper" :current-page="page" :page-size="pageSize"
                       :page-sizes="[8, 10, 30, 50]" :total="total" @current-change="handleCurrentChange"
                       @size-change="handleSizeChange" style="padding: 20px 0 0 0 !important;" />
      </el-row>
    </div>
  </div>
</template>
<script>
  import { defineComponent } from "vue";
  import moment from "moment";
  import VueJsonPretty from "vue-json-pretty";
  import { getServiceInfoList } from "@/api/serviceInfo";

  import { getGameCDKList } from "@/api/gm";
  import { ElMessage } from "element-plus";

  export default {
    name: "GameCDK",
    components: {
      VueJsonPretty,
    },
    data() {
      return {
        page: 1,
        pageSize: 10,//每页显示条目个数
        total: 12,
        data:[],
        jsonData: [
          {
            "id": 1,
            "cdk": "124ggsg",
            "start_time": 1694240712,
            "end_time": 1698240712,
            "cdkdata": {
              "account": "",
              "gbid": 0
            }
          },
          {
            "id": 2,
            "cdk": "asfasf1241gsg",
            "start_time": 1694240712,
            "end_time": 1698270712,
            "cdkdata": {
              "account": "",
              "gbid": 0
            }
          },
          {
            "id": 276811,
            "cdk": "r54E4xZTkEbsfRhv",
            "start_time": 1670256000,
            "end_time": 1701878399,
            "cdkdata": {
              "account": "",
              "gbid": 0
            }
          },{
            "id": 276812,
            "cdk": "mMEhDTAWhM2aZEg9",
            "start_time": 1670256000,
            "end_time": 1701878399,
            "cdkdata": {
              "account": "",
              "gbid": 0
            }
          },
          {
            "id": 276813,
            "cdk": "dEbPL6RmQ9D6QTuQ",
            "start_time": 1670256000,
            "end_time": 1701878399,
            "cdkdata": {
              "account": "",
              "gbid": 0
            }
          },
          {
            "id": 276814,
            "cdk": "25bmvAgb2HytJKrB",
            "start_time": 1670256000,
            "end_time": 1701878399,
            "cdkdata": {
              "account": "",
              "gbid": 0
            }
          },{
            "id": 276815,
            "cdk": "Zu2uN7nDYZY5Pwb2",
            "start_time": 1670256000,
            "end_time": 1701878399,
            "cdkdata": {
              "account": "",
              "gbid": 0
            }
          },{
            "id": 276816,
            "cdk": "HemkKfqYYNvjzDtR",
            "start_time": 1670256000,
            "end_time": 1701878399,
            "cdkdata": {
              "account": "",
              "gbid": 0
            }
          },{
            "id": 276817,
            "cdk": "25pfKm53ZnZHCuL5",
            "start_time": 1670256000,
            "end_time": 1701878399,
            "cdkdata": {
              "account": "",
              "gbid": 0
            }
          },{
            "id": 276818,
            "cdk": "vUq4PDPZkYBJvVd8",
            "start_time": 1670256000,
            "end_time": 1701878399,
            "cdkdata": {
              "account": "",
              "gbid": 0
            }
          },{
            "id": 276819,
            "cdk": "R5DfzzhwqVExGPXt",
            "start_time": 1670256000,
            "end_time": 1701878399,
            "cdkdata": {
              "account": "",
              "gbid": 0
            }
          },{
            "id": 276820,
            "cdk": "QmWdK5j2aDe3eNcy",
            "start_time": 1670256000,
            "end_time": 1701878399,
            "cdkdata": {
              "account": "",
              "gbid": 0
            }
          }],
        tableColumns: [
          { prop: 'id', label: 'id' },
          { prop: 'cdk', label: 'cdk' },
          { prop: 'start_time', label: '注册时间' },
          { prop: 'valid', label: '有效时间' },
          { prop: 'end_time', label: '失效时间' },
          { prop: 'cdkdata', label: 'cdkdata' },
        ],
        searchInfo: {
          page: 1,
          pageSize: 10,
          pageTotal: null,
        },
        rules: {},
        tableData: [
          {
            game: "1",
            cdkID: "",
            cdk: "",
            cdk_state: "",
            service_time: "",
            zone_id: "",
            role_name: "",
            role_id: "",
          },
        ],
        data: [],
        regionList: [],
        gameOptions: [],
      };
    },
    watch: {
      data() {
        // 数据变化时重新计算总页数和当前页号
        this.currentPage = 1;
      },
      currentPage() {
        // 当前页号变化时，可以根据需要从服务器获取数据
        // 或者根据数据数组和当前页号计算显示数据的起始索引和截取子数组
        // 这里只是个示例，没有实际操作
        console.log('Current page changed:', this.currentPage);
      },
    },
    methods: {
      formatDate(row,column) {
        if(column.property ==='start_time' ){
          const timestamp = row.start_time;
          const timezone = 'Asia/Shanghai'; // 上海时区
          const formattedDate = moment.tz(timestamp*1000, timezone).format('YYYY-MM-DDTHH:mm:ss.SSSZ');
          return formattedDate;
        }else if(column.property ==='end_time' ){
          const timestamp = row.end_time;
          const timezone = 'Asia/Shanghai'; // 上海时区
          const formattedDate = moment.tz(timestamp*1000, timezone).format('YYYY-MM-DDTHH:mm:ss.SSSZ');
          return formattedDate;
          // return new Date(timestamp*1000).toLocaleString();
        }else{
          return row[column.property];
        }
      },
      handleSizeChange(newSize) {
        this.pageSize = newSize
        this.getNewsList()
      },
      handleCurrentChange(newPage) {
        this.page = newPage
        this.getNewsList()
      },
      chooseRegion(regionName) {
        var findRegion = false;
        for (var i in this.regionList) {
          var region = this.regionList[i];
          if (region.regionName == regionName) {
            this.activeRegion = region;
            this.searchInfo.region = region;
            findRegion = true;
            break;
          }
        }
        if (!findRegion) {
          console.warn("not found region.", regionName);
          return;
        }
        this.setHistoryRegionID(regionName);
      },
      getHistoryRegionID() {
        return sessionStorage.getItem(this.$options.name + "HistoryRegion");
      },
      setHistoryRegionID(regionID) {
        sessionStorage.setItem(this.$options.name + "HistoryRegion", regionID);
      },
      async refreshRegionList() {
        let page = 1;
        let pageSize = 100;
        const table = await getServiceInfoList({
          page: page,
          pageSize: pageSize,
          moduleName: "CDK",
        });
        if (table.code !== 0) {
          return;
        }
        this.regionList = [];
        this.gameOptions = [];

        let dataList = table.data.list;
        for (let i in dataList) {
          let data = dataList[i];
          let regionData = JSON.parse(data.serviceData);
          this.regionList.push(regionData);
          this.gameOptions.push([data.ID, regionData.regionName]);
        }

        var historyRegionID = this.getHistoryRegionID();
        if (historyRegionID != null) {
          this.searchInfo.game = parseInt(historyRegionID);
        }
        console.log(this.gameOptions);
      },
      goQuery() {
        this.refreshData();
      },
      async refreshData() {
        if (
          this.searchInfo.region_id == null ||
          this.searchInfo.region_id === 0
        ) {
          return;
        }
        const table = await getGameCDKList(this.searchInfo);
        if (table.code === 0) {
          this.tableData = table.data.list;
          this.searchInfo.total = table.data.total;
          this.searchInfo.page = table.data.page;
          this.searchInfo.pageSize = table.data.pageSize;
        } else {
          ElMessage.error("查询CDK列表失败");
          return;
        }
        this.data = JSON.parse(JSON.stringify(this.tableData));
        console.log(table);
      },
    },
    mounted() {
      this.refreshRegionList();
      this.refreshData();
    },
    computed: {
      totalPage() {
        return Math.ceil(this.data.length / this.pageSize);  // 总页数
      },
      startIndex() {
        return (this.currentPage - 1) * this.pageSize;  // 显示数据的起始索引
      },
      displayData() {
        return this.data.slice(this.startIndex, this.startIndex + this.pageSize);  // 截取当前页的数据
      },
    },
  }
</script>
<style lang="scss" scoped></style>
