<template>
  <div class="app-container">
    <el-table
      :data="list"
      border
      fit
      highlight-current-row>
      <el-table-column align="center" label="ID" width="95">
        <template slot-scope="scope">
          {{ scope.row.id }}
        </template>
      </el-table-column>
      <el-table-column label="Title">
        <template slot-scope="scope">
          {{ scope.row.casenumber }}
        </template>
      </el-table-column>
      <el-table-column align="center" label="ID" width="95">
        <template slot-scope="scope">
          {{ scope.row.realname  }}
        </template>
      </el-table-column>
      <el-table-column label="Title">
        <template slot-scope="scope">
          {{ scope.row.gender }}
        </template>
      </el-table-column>
      <el-table-column align="center" label="ID" width="95">
        <template slot-scope="scope">
          {{ scope.row.idnumber }}
        </template>
      </el-table-column>
      <el-table-column label="Title">
        <template slot-scope="scope">
          {{scope.row.visitdate}}
        </template>
      </el-table-column>
      <el-table-column label="年龄">
        <template slot-scope="scope">
          {{ scope.row.age }}
        </template>
      </el-table-column>
      <el-table-column label="家庭住址">
        <template slot-scope="scope">
          {{scope.row.homeaddress}}
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>

export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'gray',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },
  data() {
    return {
      list: [
        {
          "id": 3,
          "casenumber": "600600",
          "realname": "李白",
          "gender": 71,
          "idnumber": "210103",
          "visitdate": "",
          "age": "",
          "homeaddress" :"",
        }],
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      var vm = this;
      this.axios({
        method:'GET',
        url:'http://localhost:80/addregister/list'
      }).then(function(resp) {
        vm.list = resp.data
      })
    }
  }
}
</script>
