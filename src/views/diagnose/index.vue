<template>
  <el-container>
    <el-aside width="250px">
      <el-tag style="width: 180px;height: 30px" align="left">患者列表</el-tag><el-button type="primary" icon="el-icon-refresh" style="height: 30px;width: 30px;padding: 5px" @click="getregisterlist"></el-button>
      <el-table
        ref="singleTable"
        highlight-current-row
        @current-change="handleCurrentChange1"
        :data="tableData.filter(data => !search || data.realname.toLowerCase().includes(search.toLowerCase()))"
        style="width: 100%;">
        <el-table-column align="center">
          <template slot="header" slot-scope="scope">
          <el-input
            v-model="search"
            size="mini"
            placeholder="输入姓名搜索" />
          </template>
          <el-table-column
          label="病历号"
          prop="casenumber">
          </el-table-column>
          <el-table-column
          label="姓名"
          prop="realname">
          </el-table-column>
          <el-table-column
          label="看诊状态"
          prop="visitstate">
          </el-table-column>
        </el-table-column>
      </el-table>
    </el-aside>
    <el-container >
      <el-header style="height: 40px">
        <el-tag style="width: 90px;height: 30px">患者信息显示</el-tag><span style="padding: 8px">患者姓名:&nbsp;{{patientinfo.realname}}&nbsp;&nbsp;&nbsp;病历号:&nbsp;{{patientinfo.casenumber}}&nbsp;&nbsp;&nbsp;年龄:&nbsp;{{patientinfo.age}}&nbsp;&nbsp;&nbsp;性别:&nbsp;{{patientinfo.gender===71?("男"):patientinfo.gender===72?("女"):("")}}</span>
      </el-header>
      <el-tabs type="border-card" style="box-shadow: 0 0 0 #ccc" v-model="activeName" @tab-click="handleClick">
        <el-tab-pane label="病历首页" name="first">
          &nbsp;<el-tag>病史内容</el-tag>&nbsp;&nbsp;&nbsp;
          <el-button type="text" icon="el-icon-edit" @click="diagnose">诊断</el-button>
          <el-button type="text" icon="el-icon-edit" @click="clean">清除</el-button>
          <el-form ref="form" :model="form" label-width="80px">
            <el-form-item label="主诉">
              <el-input type="textarea" v-model="form.readme" placeholder="请输入内容" style="margin-top: 10px;"></el-input>
            </el-form-item>
            <el-form-item label="现病史">
              <el-input type="textarea" v-model="form.present" placeholder="请输入内容" style="margin-top: 10px"></el-input>
            </el-form-item>
            <el-form-item label="既往病史">
              <el-input type="textarea" v-model="form.history" placeholder="请输入内容" style="margin-top: 10px"></el-input>
            </el-form-item>
            <el-form-item label="过敏史">
              <el-input type="textarea" v-model="form.allergy" placeholder="请输入内容" style="margin-top: 10px"></el-input>
            </el-form-item>
            <el-form-item label="体格检查">
              <el-input type="textarea" v-model="form.physique" placeholder="请输入内容" style="margin-top: 10px"></el-input>
            </el-form-item>
          </el-form>
          &nbsp;<el-tag>评估/诊断</el-tag>
          <template>
            <el-table
              ref="multipleTable"
              :data="tableData1"
              tooltip-effect="dark"
              style="width: 100%"
              @selection-change="handleSelectionChange2">
              <el-table-column
                type="selection"
                width="55">
              </el-table-column>
              <el-table-column
                prop="name"
                label="IDC编码"
                width="120">
              </el-table-column>
              <el-table-column
                prop="address"
                label="疾病名称"
                width="240">
              </el-table-column>
              <el-table-column
                label="发病时间"
                show-overflow-tooltip>
                <template slot-scope="scope">{{ scope.row.date }}</template>
              </el-table-column>
            </el-table>
            <div style="margin-top: 20px" >
              <el-button @click="toggleSelection1">增加</el-button>
              <el-button @click="toggleSelection2">删除</el-button>
            </div>
          </template>
        </el-tab-pane>
        <el-tab-pane label="检查申请" name="second"></el-tab-pane>
        <el-tab-pane label="校验申请*" name="third"></el-tab-pane>
        <el-tab-pane label="门诊确认*" name="fourth"></el-tab-pane>
        <el-tab-pane label="处置申请*" name="fifth"></el-tab-pane>
        <el-tab-pane label="成药处方" name="sixth">
          &nbsp;<el-tag>门诊诊断</el-tag>
          <div style="padding: 0 20px" >
            <el-button type="text" icon="el-icon-edit" @click="addprescription">增方</el-button>
            <el-button type="text" icon="el-icon-edit">删方</el-button>
            <el-button type="text" icon="el-icon-edit" @click="addprescriptiondetaild">开立</el-button>
            <el-button type="text" icon="el-icon-edit">刷新</el-button>
            <el-row :gutter="20">
              <el-col :span="6"><div class="grid-content bg-purple">
                <template>
                  <el-tag style="width: 180px;height: 30px" align="left">处方模板</el-tag><el-button type="primary" icon="el-icon-refresh" style="height: 30px;width: 30px;padding: 5px" @click="getregisterlist1"></el-button>
                  <el-table
                    ref="singleTable"
                    :data="tableData2"
                    tooltip-effect="dark"
                    highlight-current-row
                    @current-change="handleCurrentChange3"
                    style="width: 100%"
                    border>
                    <el-table-column
                      property="name"
                      label="处方名称"
                      show-overflow-tooltip>
                    </el-table-column>
                  </el-table>
                </template>
              </div></el-col>
              <el-col :span="18"><div class="grid-content bg-purple">
                <template>
                  <el-table
                    ref="singleTable"
                    :data="tableData3"
                    tooltip-effect="dark"
                    highlight-current-row
                    style="width: 100%"
                    :header-cell-style="{background:'#ffffff'}"
                    @current-change="handleSelectionChange4">
                    <el-table-column label="处方模板明细">
                      <el-table-column
                        prop="drugsid"
                        label="药品名称"
                        width="120">
                      </el-table-column>
                      <el-table-column
                        prop="drugsusage"
                        label="用法"
                        width="120">
                      </el-table-column>
                      <el-table-column
                        prop="dosage"
                        label="用量"
                        width="120">
                      </el-table-column>
                      <el-table-column
                        prop="frequency"
                        label="频次"
                        show-overflow-tooltip>
                      </el-table-column>
                    </el-table-column>
                  </el-table>
                </template>
              </div></el-col>
            </el-row>
          </div>
        </el-tab-pane>
      </el-tabs>
    </el-container>
  </el-container>
</template>

<script>
  import qs from 'qs'

  export default {
    data() {
      return {
        patientinfo: {

        },
        drugstemplate: {

        },
        prescription:{

        },
        activeName: 'first',
        form: {
          readme: '',
          present: '',
          history: '',
          allergy: '',
          physique: '',
        },
        tableData: [{
          casenumber: '',
          realname: '',
          visitstate: ''
        }],
        search: '',
        tableData1: [{
          date: '2016-05-03',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }, {
          date: '2016-05-02',
          name: '王小虎',
          address: '上海市普陀区金沙江路 1518 弄'
        }],
        multipleSelection: [],
        tableData2: [{
          name: '',
        }],
        tableData3: [{
          drugsid: '',
          drugsusage: '',
          dosage: '',
          frequency: ''
        }],
        multipleSelection1: [],
      }
    },
    methods: {
      getregisterlist(){
        var vm = this;
        this.axios({
          method:'GET',
          url:'http://localhost:80/addregister/list',
        }).then(function(resp) {
          vm.tableData = resp.data
        })
      },
      diagnose(){
        var vm = this;
        this.axios({
          method:'POST',
          url:'http://localhost:80/medicalreocrd/addmedicalrecord',
          transformRequest:[function(data) {
            return qs.stringify(data)
          }],
          data: {
            casenumber:vm.patientinfo.casenumber,
            registid:vm.patientinfo.id,
            readme: vm.form.readme,
            present: vm.form.present,
            history: vm.form.history,
            allergy: vm.form.readme,
            physique: vm.form.physique,
          }
        })
        this.$message({
          message: '诊断成功!',
          type: 'success'
        })
      },
      getregisterlist1(){
        var vm = this;
        this.axios({
          method:'GET',
          url:'http://localhost:80/drugstemplate/getdrugstemplate',
        }).then(function(resp) {
          vm.tableData2 = resp.data
        })
      },
      handleCurrentChange1(val){
        this.patientinfo = val;
      },
      handleCurrentChange3(val) {
        this.drugstemplate = val;
        var vm = this;
        this.axios({
          method:'GET',
          url:'http://localhost:80/drugsdetailed/getdetail',
          params: {
            drugstempid: vm.drugstemplate.id
          }
        }).then(function(resp) {
          vm.tableData3 = resp.data
        })
      },
      clean(){
        this.form.readme=''
        this.form.present=''
        this.form.history=''
        this.form.allergy=''
        this.form.physique=''
        this.$message({
          message: '清除!',
          type: 'warning'
        })
      },
      addprescription(){
        var vm = this;
        this.axios({
          method:'POST',
          url:'http://localhost:80/prescription/addprescription',
          transformRequest:[function(data) {
            return qs.stringify(data)
          }],
          data: {
            medicalid: vm.patientinfo.casenumber,
            registid: vm.patientinfo.id,
            prescriptionname: vm.drugstemplate.name,
            prescriptionstate: 1,
          }
        })
      },
      addprescriptiondetaild(){

      },
      handleSelectionChange4(val){
        this.currentRow = val
        var vm = this;
        this.axios({
          method:'POST',
          url:'http://localhost:80/prescriptiondetailed/adddetail',
          transformRequest:[function(data) {
            return qs.stringify(data)
          }],
          data: {
            drugsid: vm.currentRow.drugsid,
            drugsusage: vm.currentRow.drugsusage,
            dosage: vm.currentRow.dosage,
            frequency:vm.currentRow.frequency,
            state : 2
          }
        })
      },
      handleEdit(index, row) {
        console.log(index, row);
      },
      handleDelete(index, row) {
        console.log(index, row);
      },
      handleClick(tab, event) {
        console.log(tab, event);
      },
      toggleSelection1(){

      },
      toggleSelection2(){

      },
      handleSelectionChange2(val) {
        this.multipleSelection1 = val;
      }
    },
  }
</script>

<style scoped>
  .el-header {
    background-color: #F5F7FA;
    color: #333;
    line-height: 40px;
    height: 40px;
  }

  .el-footer {
    background-color: #F5F7FA;
    color: #ffffff;
    text-align: center;
    line-height: 40px;
  }

  .el-aside {
    background-color: #F5F7FA;
    color: #ffffff;
    text-align: center;
    line-height: 40px;
  }

  .el-main {
    background-color: #E9EEF3;
    color: #333;
    line-height: 40px;
  }

  body > .el-container {
    margin-bottom: 40px;
  }

  .el-container:nth-child(5) .el-aside,
  .el-container:nth-child(6) .el-aside {
    line-height: 40px;
  }

  .el-container:nth-child(7) .el-aside {
    line-height: 40px;
  }
</style>
