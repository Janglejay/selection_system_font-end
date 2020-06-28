<template>
  <div class="container">
    <el-row>
      <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
      <el-col :span="22">
        <div class="grid-content bg-purple-light button-container">
          <el-button
            @click="changeItem2"
            icon="el-icon-user
"
          >
            My Students
          </el-button>
          <el-button @click="changeItem1" icon="el-icon-circle-plus-outline">
            Add Student
          </el-button>
        </div>
      </el-col>
      <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
    </el-row>
    <el-row :hidden="flag != 1">
      <el-col :span="2"><div class="grid-content bg-purple"></div></el-col>
      <el-col :span="20">
        <div class="grid-content bg-purple-light">
          <el-card class="box-card">
            <div slot="header" class="clearfix">
              <span>Add Student</span>
            </div>
            <el-form :label-position="labelPosition" label-width="80px">
              <el-form-item label="Name">
                <el-input v-model="name"></el-input>
              </el-form-item>
              <el-form-item label="Number">
                <el-input v-model="number"></el-input>
              </el-form-item>
            </el-form>
            <el-button
              type="primary"
              @click="addstudent"
              style="float:right;margin:0px 10px 20px 0px;background-color: #1A237E; border-color: #1A237E"
            >
              Add
            </el-button>
          </el-card>
        </div>
      </el-col>
      <el-col :span="2"><div class="grid-content bg-purple"></div></el-col>
    </el-row>
    <el-row :hidden="flag != 2">
      <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
      <el-col :span="22">
        <div class="grid-content bg-purple-light">
          <el-card class="box-card">
            <div slot="header" class="clearfix">
              <span>My Students</span>
            </div>
            <el-table :data="mystudents" stripe style="width: 100%">
              <el-table-column
                label="Rank"
                type="index"
                width="100"
              ></el-table-column>
              <el-table-column
                prop="user.number"
                label="Number"
                width="180"
              ></el-table-column>
              <el-table-column
                prop="user.name"
                label="Name"
                width="180"
              ></el-table-column>
              <el-table-column
                prop="mydirection"
                label="Direction"
                width="280"
              ></el-table-column>
              <el-table-column label="Edit">
                <template slot-scope="scope">
                  <el-button
                    icon="el-icon-edit"
                    circle
                    @click="handleEdit(scope.$index, scope.row)"
                    style="background-color: #1a237e;"
                  ></el-button>
                  <!-- <el-button
                    size="mini"
                    @click="handleEdit(scope.$index, scope.row)"
                  >
                    <i class="el-icon-edit"></i>
                  </el-button> -->
                  <el-button
                    icon="el-icon-delete"
                    circle
                    @click="handleDelete(scope.$index, scope.row)"
                  ></el-button>
                  <!-- <el-button
                    size="mini"
                    type="danger"
                    @click="handleDelete(scope.$index, scope.row)"
                  >
                    <i class="el-icon-delete"></i>
                  </el-button> -->
                </template>
              </el-table-column>
            </el-table>
            <el-dialog title="Modify Direction" :visible.sync="open1">
              <el-form>
                <el-form-item label="Direction" :label-width="formLabelWidth">
                  <el-input
                    type="text"
                    v-model="dir"
                    autocomplete="off"
                  ></el-input>
                </el-form-item>
              </el-form>
              <div slot="footer" class="dialog-footer">
                <el-button @click="open1 = false">Cancel</el-button>
                <el-button type="primary" @click="updateSdir">
                  Confirm
                </el-button>
              </div>
            </el-dialog>
          </el-card>
        </div>
      </el-col>
      <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
    </el-row>
  </div>
</template>

<script>
import { mapState } from "vuex";
import { ADD_STUDENT } from "@/store/types.js";
import { UPDATE_SDIR } from "@/store/types.js";
import { DELETE_RELATION } from "@/store/types.js";
export default {
  created() {
    this.$store.dispatch("mystudentsindex");
    console.log(this.mystudents);
  },
  computed: {
    ...mapState(["tutor"]),
    ...mapState(["mystudents"])
  },
  data: () => ({
    //表单
    labelPosition: "right",
    name: "",
    number: "",
    open1: false,
    formLabelWidth: "120px",
    updateItemRow: null,
    dir: null,
    flag: 2
  }),
  methods: {
    changeItem1() {
      this.flag = 1;
    },
    changeItem2() {
      this.flag = 2;
    },
    addstudent() {
      console.log(this.name);
      console.log(this.number);
      if (this.mystudents.length == this.tutor.maxStuNum) {
        this.$message.error("Limted Number Can't Add");
      } else {
        this.$store
          .dispatch(ADD_STUDENT, {
            user: {
              name: this.name,
              number: this.number
            }
          })
          .then(() => {
            this.$message({
              message: "Success Add ",
              type: "success"
            });
          })
          .catch(() => {
            this.$message.error(
              "The student has already chosen a tutor. It cannot be repeated"
            );
          });
      }
    },
    success() {
      this.$message({
        message: "Success",
        type: "success"
      });
    },
    handleEdit(index, row) {
      this.updateItemRow = row;
      console.log(index + 1, row);
      this.dir = row.mydirection;
      this.open1 = true;
    },
    handleDelete(index, row) {
      this.$confirm("Make sure to deselect the student?", "Tip", {
        confirmButtonText: "Confirm",
        cancelButtonText: "Cancel",
        type: "warning"
      })
        .then(() => {
          this.$store.dispatch(DELETE_RELATION, {
            id: row.id
          });
          this.$message({
            type: "success",
            message: "Success delete!"
          });
        })
        .catch(() => {
          this.$message({
            type: "info",
            message: "Cancel delete"
          });
        });
      console.log(index + 1);
      console.log(row);
    },
    updateSdir() {
      this.$store
        .dispatch(UPDATE_SDIR, {
          id: this.updateItemRow.id,
          mydirection: this.dir
        })
        .then(() => {
          this.open1 = false;
          this.success();
        });
    }
  }
};
</script>

<style scoped>
.container {
  width: 100%;
  padding-top: 30px;
}
.el-table {
  margin: 0px 30px;
}
/* 卡片 */
.text {
  font-size: 14px;
}
.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}
.clearfix:after {
  clear: both;
}

.box-card {
  width: 100%;
}
/* 卡片 */
/* .button-container > .el-button {
  font-size: 15px;
  background-color: #ccccff;
  color: #5fa1a1;
  width: 150px;
  height: 70px;
  border-radius: 40px;
  margin-right: 10px;
} */
/* 选课列表 */
.el-input {
  width: 80%;
}

/* 布局css */
.el-row {
  margin-bottom: 20px;
}
.el-col {
  border-radius: 4px;
}
.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
.row-bg {
  padding: 10px 0;
  background-color: #f9fafc;
}
/* 布局css */
</style>
