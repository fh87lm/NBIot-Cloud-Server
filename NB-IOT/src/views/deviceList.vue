<template>
  <div>
    <el-row class="delistrow">
        <el-breadcrumb separator-class="el-icon-arrow-right">
          <el-breadcrumb-item :to="{ path: '/' }" replace>首页</el-breadcrumb-item>
          <el-breadcrumb-item >设备管理</el-breadcrumb-item>
          <el-breadcrumb-item :to="{ path: '/deviceList'}" replace>设备列表</el-breadcrumb-item>
        </el-breadcrumb>
    </el-row>

      <el-row class="delistrow">
        <el-table border width="1500" :data="tableData" stripe style="width: 150%">

          <el-table-column prop="deviceID" label="设备ID" sortable width="200" height="20"></el-table-column>
          <el-table-column prop="deviceName" label="设备名称" sortable width="150"></el-table-column>
          <el-table-column prop="devicePwd" label="设备密码" sortable width="175"></el-table-column>
          <el-table-column prop="deviceAddr" label="位置" sortable width="300"></el-table-column>
          <el-table-column prop="deviceStatus" label="状态" sortable width="90" height="20"></el-table-column>
          <el-table-column prop="operation" label="操作" width="300">
            <template slot-scope="scope">
              <!--<el-button @click="$router.push({path: '/debugDevice', params: {name: 'row', dataObj: scope.row}})" type="text" size="small">数据传输</el-button>-->
              <!--<router-view></router-view>-->

              <el-button @click="debugDevice(scope.row)" type="text" size="small">数据传输</el-button>
              <el-button @click="editDevice(scope.row)" type="text" size="small">编辑</el-button>
              <!--<el-button @click.native.prevent="deleteDevice(scope.$index, tableData4)" type="text" size="small">删除</el-button>-->
              <!--<template slot-scope="scope">-->
                <el-button @click.native.prevent="deleteDevice(scope.$index, tableData)" type="text" size="small">删除</el-button>
              <!--</template>-->
            </template>
          </el-table-column>
        </el-table>
      </el-row>

  </div>
</template>

<script>
  import axios from 'axios'
  import Bus from './../API/bus'
    export default {
      name: "device-list",
      data() {
        return {
          tableData: [],
          loading: true,
          deInfoRow: {},
          deStatus: "离线"
        }
      },
      methods: {
        indexMethod(index) {
          return index;
        },
        debugDevice(row) {
          //console.log(row);
          this.deInfoRow = row;
          this.$router.push('/debugDevice');
        },
        editDevice(row) {
            this.$router.push("/createDevice");
        },
        deleteDevice(index, rows) {
          // console.log(rows.toString());
          // var whatIndex = null;
          // angular.forEach($scope.rows, function(cb, index) {
          //   if (cb.ID === ID) {
          //     whatIndex = index;
          //   }
          // });
          rows.splice(index, 1);

        }
      },
      mounted() {
        axios.get("/deviceList").then((response) => {
          this.tableData = response.data;
          this.loading = false;
          this.$notify({
            title: '成功',
            message: '成功加载您的设备信息',
            type: 'success'
          });
          // console.log(response.data);
          for(var data in this.tableData){
            this.tableData[data]['deviceStatus'] = this.deStatus;
            // console.log("\t",this.tableData[data]);
          }

        });


      },
      destroyed () {
        setTimeout(() => {
          Bus.$emit('getDeviceInfo', this.deInfoRow);
        }, 300);
        // console.log("list");
        // console.log(this.deInfoRow);
      },
    }
</script>

<style scoped>
  .delistrow {
    margin-bottom: 20px;
    margin-left: 20px;
    &:last-child {
       margin-bottom: 0;
     }
  }
</style>
