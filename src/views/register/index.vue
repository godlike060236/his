<template>
  <div class="app-container">
    <h2>&nbsp;&nbsp;&nbsp;挂号信息</h2>
    <span>&nbsp;&nbsp;&nbsp;</span>
    <el-form ref="form" :model="form" :rules="rules" label-width="120px">
      <el-row :gutter="20">
        <el-col :span="8"><div class="grid-content bg-purple">
        <el-form-item label="病历号">
        <el-input v-model="form.casenumber" />
        </el-form-item>
        </div></el-col>
      </el-row>

      <el-row :gutter="20">
        <el-col :span="5"><div class="grid-content bg-purple">
          <el-form-item label="姓名">
            <el-input v-model="form.realname" />
          </el-form-item>
        </div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple">
          <el-form-item label="性别" >
            <el-select v-model="form.gender" placeholder="请选择您的性别" style="width: 100%">
              <el-option label="男" value=71 />
              <el-option label="女" value=72 />
            </el-select>
          </el-form-item>
        </div></el-col>
        <el-col :span="5"><div class="grid-content bg-purple">
          <el-form-item label="年龄" >
            <el-input v-model="form.age" />
          </el-form-item>
        </div></el-col>
      </el-row>


      <el-row :gutter="20">
        <el-col :span="8"><div class="grid-content bg-purple">
          <el-form-item label="出生日期" prop="birthdate">
            <el-date-picker
              v-model="form.birthdate"
              type="date"
              value-format="yyyy-MM-dd"
              format="yyyy-MM-dd"
              placeholder="选择日期">
            </el-date-picker>
          </el-form-item>
        </div></el-col>
        <el-col :span="8"><div class="grid-content bg-purple">
          <el-form-item label="看诊日期" prop="visitdate">
            <el-date-picker
              v-model="form.visitdate"
              type="date"
              value-format="yyyy-MM-dd"
              format="yyyy-MM-dd"
              placeholder="选择日期">
            </el-date-picker>
          </el-form-item>
        </div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple">
          <el-form-item label="午别" >
            <el-select v-model="form.noon" placeholder="请选择您的午别" style="width: 100%">
              <el-option label="上午" value="上午" />
              <el-option label="下午" value="下午" />
            </el-select>
          </el-form-item>
        </div></el-col>
      </el-row>


      <el-row :gutter="20">
        <el-col :span="8"><div class="grid-content bg-purple">
          <el-form-item label="身份证号" >
            <el-input v-model="form.idnumber" />
          </el-form-item>
        </div></el-col>
        <el-col :span="14"><div class="grid-content bg-purple">
          <el-form-item label="家庭住址" >
            <el-input v-model="form.homeaddress" />
          </el-form-item>
        </div></el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :span="8"><div class="grid-content bg-purple">
          <el-form-item label="挂号科室" >
            <el-select v-model="form.deptid" placeholder="请选择您的挂号科室" style="width: 100%">
              <el-option label="心血管内科" value="1" />
              <el-option label="消化科" value="0" />
            </el-select>
          </el-form-item>
        </div></el-col>
        <el-col :span="8"><div class="grid-content bg-purple">
          <el-form-item label="号别" >
            <el-radio-group v-model="form.registleid">
              <el-radio label="普通号" value="1"/>
              <el-radio label="专家号" value="0"/>
            </el-radio-group>
          </el-form-item>
        </div></el-col>
        <el-col :span="6"><div class="grid-content bg-purple">
          <el-form-item label="看诊医生" >
            <el-select v-model="form.userid" placeholder="请选择您的看诊医生" style="width: 100%">
              <el-option label="张医生" value="1" />
              <el-option label="王医生" value="1" />
            </el-select>
          </el-form-item>
        </div></el-col>
      </el-row>
      <el-row :gutter="20">
        <el-col :span="8"><div class="grid-content bg-purple">
          <el-form-item label="结算类别" >
            <el-select v-model="form.settleid" placeholder="请选择您的结算类别" style="width: 100%">
              <el-option label="市医保" value="1" />
              <el-option label="自费" value="2" />
            </el-select>
          </el-form-item>
        </div></el-col>
        <el-col :span="8"><div class="grid-content bg-purple">
          <el-form-item label="是否需要病历本">
            <el-switch v-model="form.isbook" active-value="1" inactive-value="0"/>
          </el-form-item>
        </div></el-col>
      </el-row>


      <el-form-item>
        <el-button type="primary" @click="onSubmit('form')">挂号</el-button>
        <el-button @click="onCancel">清空</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
  import qs from 'qs'
  export default {
    data() {
      return {
        form: {
          "id": "",
          "casenumber": "",
          "realname": "",
          "gender": "",
          "idnumber": "",
          "birthdate": "",
          "age": "",
          "homeaddress": "",
          "visitdate": "",
          "noon": "",
          "deptid": "",
          "userid": "",
          "registleid": "",
          "settleid": "",
          "isbook": "",
          "registerid": "",
          "visitstate": 1,
        },
        rules: {
          birthdate: [
            { type: 'string', required: true, message: '请选择日期', trigger: 'change' }
          ],
          visitdate: [
            { type: 'string', required: true, message: '请选择时间', trigger: 'change' }
          ]
        }
      };
    },

    methods: {
      onSubmit(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            var vm = this;
            this.axios({
              method:'POST',
              url:'http://localhost:80/addregister/tosubmit',
              transformRequest:[function(data) {
                return qs.stringify(data)
              }],
              data:vm.form,
            });
            this.$message({
              message: '挂号成功!',
              type: 'success'
            })
          } else {
            this.$message({
              message: '挂号失败!',
              type: 'error'
            })
            return false;
          }
        });
      },
      onCancel() {
        this.form.id=''
        this.form.casenumber=''
        this.form.realname=''
        this.form.gender=''
        this.form.idnumber=''
        this.form.birthdate=''
        this.form.age=''
        this.form.homeaddress=''
        this.form.visitdate=''
        this.form.noon=''
        this.form.deptid=''
        this.form.userid=''
        this.form.registleid=''
        this.form.settleid=''
        this.form.isbook=''
        this.form.registerid=''
        this.form.visitstate=''
        this.$message({
          message: '清空!',
          type: 'warning'
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
