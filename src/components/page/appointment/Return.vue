<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/main' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>挂号收费</el-breadcrumb-item>
      <el-breadcrumb-item>退费</el-breadcrumb-item>
    </el-breadcrumb>
    <br />
    <br />
    <div class="head-input">
      <el-dropdown placement trigger="click">
        <span class="el-dropdown-link">
          病历号
          <i class="el-icon-arrow-down el-icon--right"></i>
        </span>
        <el-dropdown-menu slot="dropdown">
          <el-dropdown-item>病历号</el-dropdown-item>
          <el-dropdown-item>身份证号</el-dropdown-item>
        </el-dropdown-menu>
      </el-dropdown>
      <el-input v-model="records_num" style="width:195px" placeholder="请输入"></el-input>

      <div>
        <el-button icon="el-icon-search" style="background-image:linear-gradient(134deg, #00e2ff 0%, #bb00ff 100%); color: white" @click="search">搜索</el-button>
        <el-button icon="el-icon-delete" style="background-image:linear-gradient(134deg, #00e2ff 0%, #bb00ff 100%); color: white" @click="clear">清空</el-button>
      </div>
    </div>
    <br />
    <br />
    <el-table :data="tableData" style="width: 100%">
      <el-table-column prop="records_num" label="病历号" width="200"></el-table-column>
      <el-table-column prop="name" label="姓名" width="200"></el-table-column>
      <el-table-column prop="idCardNum" label="身份证号" width="200"></el-table-column>
      <el-table-column prop="itemsName" label="项目名称" width="250"></el-table-column>
      <el-table-column prop="price" label="单价" width="200"></el-table-column>
      <el-table-column prop="num" label="数量" width="200"></el-table-column>
      <el-table-column prop="state" label="状态" width="200"></el-table-column>

      <el-table-column label="操作">
        <template slot-scope="scope">
          <el-button size="small" style="background-image:linear-gradient(134deg, #00e2ff 0%, #bb00ff 100%); color: white" @click="settle(scope.row,scope.$index)">退费</el-button>
        </template>
      </el-table-column>
    </el-table>

    <el-dialog title="退费" :visible.sync="dialogVisible" width="30%" :before-close="handleClose">
      <span>信息核对：</span>
      <br />
      <br />
      <span>姓名：{{name}}</span>
      <br />
      <br />
      <span>身份证号：{{idCard}}</span>
      <br />
      <br />
      <span>退费项目：{{returnItem}}</span>
      <br />
      <br />
      <el-link :underline="false" style="background-image:linear-gradient(134deg, #00e2ff 0%, #bb00ff 100%); color: white">退费金额：{{sum}} ¥</el-link>
      <br />
      <br />
      <span>请在退费完成后点击确定打印发票</span>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button style="background-image:linear-gradient(134deg, #00e2ff 0%, #bb00ff 100%); color: white" @click="sureSettle">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  inject: ["reload"],
  data() {
    return {
      id: "",
      records_num: "",
      name: "",
      idCard: "",
      tableData: [],
      dialogVisible: false,
      returnItem: "",
      sum: 0
    };
  },
  mounted() {},
  methods: {
    clear() {
      this.reload();
      this.$message.success("已清空");
    },
    search() {
      if (this.records_num == "") {
        this.$message.warning("请先输入病历号再搜索");
      } else {
        this.tableData = [
          {
            records_num: this.records_num,
            name: "李白",
            idCardNum: "140227199908090090",
            itemsName: "双黄连",
            price: 30,
            num: 1,
            state: "已缴费"
          },
          {
            records_num: this.records_num,
            name: "李白",
            idCardNum: "140227199908090090",
            itemsName: "板蓝根",
            price: 30,
            num: 1,
            state: "已缴费"
          }
        ];
      }
    },

    settle() {
      this.dialogVisible = true;
      this.sum = 30;
      this.name = "李白";
      this.idCard = "140227199908090090";
      this.returnItem = '双黄连'
    },
    sureSettle() {
      this.dialogVisible = false;
      this.$message.success("退费成功");
    },
    handleClose(done) {
      this.$confirm("确认关闭？")
        .then(_ => {
          done();
        })
        .catch(_ => {});
    }
  }
};
</script>

<style scoped>
.head-input {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  width: 32%;
}
</style>