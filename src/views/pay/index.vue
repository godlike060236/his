<template>
  <div class="app-container">
    <h2>&nbsp;&nbsp;&nbsp;收费信息</h2>
    <span>&nbsp;&nbsp;&nbsp;</span>
    <div style="padding-left: 60px">
      <el-row :gutter="20">
        <el-col :span="8"><div class="grid-content bg-purple">
          <el-input v-model="medicalnumber" placeholder="请输入病历号" ></el-input></div>
        </el-col>
        <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
        <el-col :span="4"><div class="grid-content bg-purple">
          <el-button type="primary" icon="el-icon-search" @click="getinfo()">查询</el-button>
        </div></el-col>
      </el-row>
    </div >

    <h4>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;患者信息确认</h4>
    <span>&nbsp;&nbsp;&nbsp;</span>
    <div>
      <el-form ref="form" :data="form" label-width="120px">
        <el-row :gutter="20">
          <el-col :span="6"><div class="grid-content bg-purple">
            <el-form-item label="姓  名" >
              <el-input v-model="form.realname" />
            </el-form-item>
          </div>
          </el-col>
          <el-col :span="6"><div class="grid-content bg-purple">
            <el-form-item label="身份证号" >
              <el-input v-model="form.idnumber" />
            </el-form-item>
          </div></el-col>
          <el-col :span="7"><div class="grid-content bg-purple">
            <el-form-item label="家庭住址" >
              <el-input v-model="form.homeaddress" />
            </el-form-item>
          </div>
          </el-col>
          <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
          <el-col :span="4"><div class="grid-content bg-purple">
            <el-button type="primary" icon="el-icon-search" @click="getinfo1()">查询</el-button>
          </div></el-col>
        </el-row>
      </el-form>
    </div>


    <h4>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;患者挂号信息</h4>
    <span>&nbsp;&nbsp;&nbsp;</span>
    <div style="padding-left: 60px;padding-right: 60px">
    <el-table
      :data="list"
      fit
      highlight-current-row>
      <el-table-column label="病历号">
        <template slot-scope="scope">
          {{ scope.row.casenumber }}
        </template>
      </el-table-column>
      <el-table-column  label="姓名" >
        <template slot-scope="scope">
          {{ scope.row.realname  }}
        </template>
      </el-table-column>
      <el-table-column  label="身份证号" >
        <template slot-scope="scope">
          {{ scope.row.idnumber }}
        </template>
      </el-table-column>
      <el-table-column label="挂号日期">
        <template slot-scope="scope">
          {{scope.row.visitdate }}
        </template>
      </el-table-column>
      <el-table-column  label="挂号午别" >
        <template slot-scope="scope">
          {{ scope.row.noon  }}
        </template>
      </el-table-column>
      <el-table-column  label="看诊状态" >
        <template slot-scope="scope">
          {{ scope.row.visitstate }}
        </template>
      </el-table-column>
      </el-table>
      <div style="margin-top: 20px">
        <el-button type="primary" icon="el-icon-goods" @click="toggleSelection()">收费结算</el-button>
      </div>
    </div>
  </div>
</template>


<script>

import qs from 'qs'

export default {
  data() {
    return {
      medicalnumber:"",
      form: {
          realname: "",
          idnumber: "",
          homeaddress: "",
        },
      multipleSelection: [],
      list: {
        casenumber: '',
        realname: '',
        homeaddress: '',
        idnumber: '',
        visitdate: '',
        noon: '',
        visitstate:''
      },
    }
  },
  methods: {
    /*handleSelectionChange(val) {
      this.multipleSelection = val;
    },*/
    getinfo(){
      var vm = this;
      this.axios({
        method:'GET',
        url:'http://localhost:80/addregister/getinfo',
        params: {
          casenumber: vm.medicalnumber
        }
      }).then(function(resp) {
        vm.form = resp.data
      })
    },
    getinfo1(){
      var vm = this;
      this.axios({
        method:'GET',
        url:'http://localhost:80/addregister/getinfolist',
        params: {
          casenumber: vm.medicalnumber
        }
      }).then(function(resp) {
        vm.list = resp.data
      })
    },
  }
}
</script>
<style scoped>
  .line{
    text-align: center;
  }
  .el-row {
    margin-bottom: 20px;
  }
  .el-col {
    border-radius: 4px;
  }
  .bg-purple-dark {
    background: #ffffff;
  }
  .bg-purple {
    background: #ffffff;
  }
  .bg-purple-light {
    background: #ffffff;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #ffffff;
  }
</style>
