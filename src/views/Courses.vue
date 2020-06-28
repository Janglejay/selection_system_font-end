<template>
  <div class="container">
    <el-row>
      <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
      <el-col :span="22">
        <div class="grid-content button-container">
          <el-button @click="changeItem1" icon="el-icon-s-order">
            My Courses
          </el-button>
          <el-button @click="changeItem2" icon="el-icon-user-solid">
            Import Students
          </el-button>
          <el-button @click="changeItem3" icon="el-icon-s-marketing">
            Students ranking
          </el-button>
        </div>
      </el-col>
      <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
    </el-row>
    <el-row :hidden="flag != 1">
      <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
      <el-col :span="22">
        <div class="grid-content bg-purple-light">
          <el-card class="box-card">
            <div slot="header" class="clearfix">
              <span>My Courses</span>
              <el-button
                icon="el-icon-circle-plus-outline"
                style="float: right; padding: 3px 0"
                type="text"
                @click="open2 = true"
              >
                Add Course
              </el-button>
            </div>

            <div>
              <el-table :data="tcourses" style="width: 100%">
                <el-table-column
                  type="index"
                  label="Rank"
                  width="100"
                ></el-table-column>
                <el-table-column
                  prop="name"
                  label="Course Name"
                  width="180"
                ></el-table-column>
                <el-table-column
                  prop="weight"
                  label="Weight(0-1)"
                  width="180"
                ></el-table-column>
                <el-table-column label="Edit" width="300">
                  <template slot-scope="scope">
                    <el-button
                      icon="el-icon-edit"
                      circle
                      @click="handleEdit(scope.$index, scope.row)"
                      style="background-color: #1A237E;"
                    ></el-button>

                    <el-button
                      icon="el-icon-delete"
                      circle
                      @click="handleDelete(scope.$index, scope.row)"
                    ></el-button>
                  </template>
                </el-table-column>
              </el-table>
            </div>

            <el-dialog title="Add Course" :visible.sync="open2">
              <el-form ref="a">
                <el-form-item label="Course Name" :label-width="formLabelWidth">
                  <el-input
                    type="text"
                    v-model="name"
                    autocomplete="off"
                  ></el-input>
                </el-form-item>
                <el-form-item label="Weight" :label-width="formLabelWidth">
                  <el-input
                    type="text"
                    v-model="weight"
                    autocomplete="off"
                  ></el-input>
                </el-form-item>
              </el-form>
              <div slot="footer" class="dialog-footer">
                <el-button @click="open2 = false">Cancel</el-button>
                <el-button type="primary" @click="addCourse">
                  Confirm
                </el-button>
              </div>
            </el-dialog>

            <el-dialog title="Edit Course Imformation" :visible.sync="open1">
              <el-form>
                <el-form-item label="Course Name" :label-width="formLabelWidth">
                  <el-input
                    type="text"
                    v-model="name0"
                    autocomplete="off"
                  ></el-input>
                </el-form-item>
                <el-form-item label="Weight" :label-width="formLabelWidth">
                  <el-input
                    type="text"
                    v-model="weight0"
                    autocomplete="off"
                  ></el-input>
                </el-form-item>
              </el-form>
              <div slot="footer" class="dialog-footer">
                <el-button @click="open1 = false">Cancel</el-button>
                <el-button type="primary" @click="updateCourse">
                  Confirm
                </el-button>
              </div>
            </el-dialog>
          </el-card>
        </div>
      </el-col>
      <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
    </el-row>

    <el-row :hidden="flag != 2">
      <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
      <el-col :span="22">
        <div class="grid-content bg-purple-light">
          <el-card class="box-card">
            <div slot="header" class="clearfix">
              <span>Import Students</span>
              <!-- <el-button
                style="float: right; padding: 3px 0"
                type="text"
                @click="open2 = true"
              >
                添加课程
              </el-button> -->
            </div>

            <div>
              <el-select
                v-model="value"
                filterable
                placeholder="Choose a course"
                style="margin:0px 30px;"
              >
                <el-option
                  v-for="item in tcourses"
                  :key="item.id"
                  :label="item.name"
                  :value="item.id"
                ></el-option>
              </el-select>
              <form>
                <input
                  type="file"
                  @change="readFile"
                  style="margin: 30px 0px 0px 30px;"
                />
              </form>
              <br />
              <el-table :data="students" style="width: 100%">
                <el-table-column
                  type="index"
                  label="Rank"
                  width="100"
                ></el-table-column>
                <el-table-column
                  prop="number"
                  label="Number"
                  width="180"
                ></el-table-column>
                <el-table-column
                  prop="name"
                  label="Name"
                  width="180"
                ></el-table-column>
                <el-table-column
                  prop="score"
                  label="Grade"
                  width="180"
                ></el-table-column>
              </el-table>
              <el-button
                type="primary"
                style="float: right; margin:10px;background-color: #1a237e; border-color: #1a237e;width:90px"
                @click="buildStudent"
              >
                Import
              </el-button>
            </div>
          </el-card>
        </div>
      </el-col>
      <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
    </el-row>

    <el-row :hidden="flag != 3">
      <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
      <el-col :span="22">
        <div class="grid-content bg-purple-light">
          <el-card class="box-card">
            <div slot="header" class="clearfix">
              <span>Comprehensive ranking</span>
              <!-- <el-button
                type="text"
                style="float: right; margin:3px 0px;"
                @click="excuteStudent"
              >
                生成排名
              </el-button> -->
            </div>

            <div>
              <el-table :data="studentUsers" style="width: 100%">
                <el-table-column
                  type="index"
                  label="Rank"
                  width="100"
                ></el-table-column>
                <el-table-column
                  prop="name"
                  label="Name"
                  width="180"
                ></el-table-column>
                <el-table-column
                  prop="number"
                  label="Number"
                  width="180"
                ></el-table-column>
              </el-table>
            </div>
          </el-card>
        </div>
      </el-col>
      <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
    </el-row>
  </div>
</template>

<script>
import { mapState } from "vuex";
import { DELETE_COURSE } from "@/store/types.js";
import { ADD_COURSE } from "@/store/types.js";
import { UPDATE_COURSE } from "@/store/types.js";
import { readStudentsFile } from "@/utils/ExcelUtils.js";
import { BUILD_STUDENT } from "@/store/types.js";
export default {
  created() {
    this.$store.dispatch("coursesindex");
    this.$store.dispatch("excuteStudent");
    console.log(this.tcourses);
  },
  data: () => ({
    open1: false,
    open2: false,
    formLabelWidth: "120px",
    name: null,
    weight: null,
    name0: null,
    weight0: null,
    updateItemRow: null,
    //建立学生
    students: null,
    student: { user: { number: null } },
    course: { id: null },
    buildStudents: [
      {
        score: null,
        student: { user: { number: null, name: null } },
        course: { id: null }
      }
    ],
    value: "", //前端传来的课程id
    cid: null,
    flag: 1
  }),
  computed: {
    ...mapState(["tcourses"]),
    ...mapState(["isBuildStudents"]),
    ...mapState(["studentUsers"])
  },
  methods: {
    changeItem1() {
      this.flag = 1;
    },
    changeItem2() {
      this.flag = 2;
    },
    changeItem3() {
      this.flag = 3;
      this.$store.dispatch("excuteStudent");
    },
    excuteStudent() {
      this.$store.dispatch("excuteStudent");
    },
    handleEdit(index, row) {
      this.updateItemRow = row;
      this.weight0 = row.weight;
      this.name0 = row.name;
      console.log(index + 1, row);
      this.open1 = true;
    },
    handleDelete(index, row) {
      this.$confirm("Confirm delete this student?", "tip", {
        confirmButtonText: "confirm",
        cancelButtonText: "cancle",
        type: "warning"
      })
        .then(() => {
          this.$store
            .dispatch(DELETE_COURSE, {
              id: row.id
            })
            .then(() => {
              this.$message({
                type: "success",
                message: "delete success!"
              });
            })
            .catch(() => {
              this.$message.error("delete failed");
            });
        })

        .catch(() => {
          this.$message({
            type: "info",
            message: "cancle delete"
          });
        });
      console.log(index + 1);
      console.log(row);
    },
    success() {
      this.$message({
        message: "success",
        type: "success"
      });
    },
    addCourse() {
      this.$store
        .dispatch(ADD_COURSE, {
          name: this.name,
          weight: this.weight
        })
        .then(() => {
          this.open2 = false;
          (this.name = null), (this.weight = null), this.success();
        });
    },
    updateCourse() {
      this.$store
        .dispatch(UPDATE_COURSE, {
          id: this.updateItemRow.id,
          name: this.name0,
          weight: this.weight0
        })
        .then(() => {
          this.open1 = false;
          this.success();
        });
    },
    readFile(event) {
      let file = event.target.files[0];
      readStudentsFile(file).then(data => {
        this.students = data;
        // console.log(this.students);
        // console.log(this.students[0].number);
        // console.log(this.buildStudents);
        // console.log(this.value);
      });
    },
    // buildStudent() {
    //   this.$store.dispatch(BUILD_STUDENT, {
    //     score: this.students[0].score,
    //     student: { user: { number: this.students[0].number } }, //students列表第一个对象的分数和学号
    //     course: { id: this.value } //前端选的课程id
    //   });
    // }

    buildStudent() {
      const loading = this.$loading({
        lock: true,
        text: "Loading",
        spinner: "el-icon-loading",
        background: "rgba(0, 0, 0, 0.7)"
      });
      var datas = [];
      for (let index = 0; index < this.students.length; index++) {
        datas.push({
          score: this.students[index].score,
          student: {
            user: {
              number: this.students[index].number,
              name: this.students[index].name
            }
          },
          course: { id: this.value }
        });
      }
      // this.buildStudents = datas;
      console.log(datas);
      this.$store
        .dispatch(BUILD_STUDENT, datas)
        .then(() => {
          if (this.isBuildStudents == true) {
            loading.close();
            this.$message({
              message: "Build Success",
              type: "success"
            });
          }
        })
        .catch(() => {
          loading.close();
          this.$message.error("Build failed");
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
