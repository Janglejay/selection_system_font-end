<template>
  <div class="container">
    <el-row>
      <el-col :span="2"><div class="grid-content bg-purple"></div></el-col>
      <el-col :span="20">
        <div class="grid-content bg-purple-light">
          <el-card class="box-card">
            <div slot="header" class="clearfix">
              <span>Base Imformations</span>
              <!-- <el-button
                icon="el-icon-edit"
                size="mini"
                circle
                @click="open1 = true"
                style="background-color: #ccccff;float: right;"
              ></el-button> -->
              <el-button
                @click="open1 = true"
                icon="el-icon-edit"
                style="float: right; padding: 3px 0"
                type="text"
                plain
              >
                Modify Password
              </el-button>
            </div>
            <template v-if="istutor">
              <div class="item">
                <el-tag>Name</el-tag>
                {{ tutor.user.name }}
              </div>
            </template>
            <template v-if="isstudent">
              <div class="item">
                <el-tag>Name</el-tag>
                {{ student.user.name }}
              </div>
            </template>
            <template v-if="istutor">
              <div class="item">
                <el-tag>User Number</el-tag>
                {{ tutor.user.number }}
              </div>
            </template>
            <template v-if="isstudent">
              <div class="item">
                <el-tag>User Number</el-tag>
                {{ student.user.number }}
              </div>
            </template>

            <div class="item" v-if="isstudent">
              <el-tag>My Tutor</el-tag>
              {{ stutor }}
            </div>
            <el-dialog title="Modify Password" :visible.sync="open1">
              <el-form>
                <el-form-item
                  label="New Password"
                  :label-width="formLabelWidth"
                >
                  <el-input
                    type="password"
                    v-model="password0"
                    autocomplete="off"
                  ></el-input>
                </el-form-item>
                <el-form-item label="Validation" :label-width="formLabelWidth">
                  <el-input
                    type="password"
                    v-model="password1"
                    autocomplete="off"
                  ></el-input>
                </el-form-item>
              </el-form>
              <div slot="footer" class="dialog-footer">
                <el-button @click="open1 = false">Cancel</el-button>
                <el-button type="primary" @click="updatePwd">
                  Confirm
                </el-button>
              </div>
            </el-dialog>
          </el-card>
        </div>
      </el-col>
      <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
    </el-row>
    <el-row v-if="tutor">
      <el-col :span="2"><div class="grid-content bg-purple"></div></el-col>
      <el-col :span="20">
        <div class="grid-content bg-purple-light">
          <el-card class="box-card">
            <div slot="header" class="clearfix">
              <span>Recruit Students</span>
              <el-button
                @click="open22"
                icon="el-icon-edit"
                style="float: right; padding: 3px 0"
                type="text"
                plain
              >
                Modify Imformation
              </el-button>
            </div>

            <div class="item">
              Seleted Number
              <el-tag>{{ tutor.nowStuNum }}</el-tag>
            </div>
            <div class="item">
              Limited Number
              <el-tag>{{ tutor.maxStuNum }}</el-tag>
            </div>
            <!-- <el-progress
              :percentage="tutor.nowStuNum"
              :format="format"
            ></el-progress> -->
            <div class="item">
              <!-- 招收学生范围：成绩排名在1-{{ tutor.scopeStuNum }}之间 -->
              Recruit students with grades between
              <el-tag>1</el-tag>
              -
              <el-tag>{{ tutor.scopeStuNum }}</el-tag>
            </div>
          </el-card>
        </div>
      </el-col>
      <el-col :span="1"><div class="grid-content bg-purple"></div></el-col>
      <el-dialog title="Conditions of Admission" :visible.sync="open2">
        <el-form>
          <el-form-item label="LimitedNumber" :label-width="formLabelWidth">
            <el-input
              type="text"
              v-model="maxStuNum"
              autocomplete="off"
            ></el-input>
          </el-form-item>
          <el-form-item label="Lowest rank" :label-width="formLabelWidth">
            <el-input
              type="text"
              v-model="scopeStuNum"
              autocomplete="off"
            ></el-input>
          </el-form-item>
        </el-form>
        <div slot="footer" class="dialog-footer">
          <el-button @click="open2 = false">Cancel</el-button>
          <el-button type="primary" @click="updateNote">
            Confirm
          </el-button>
        </div>
      </el-dialog>
    </el-row>
  </div>
</template>

<script>
import { mapState } from "vuex";
import { UPDATE_PWD } from "@/store/types.js";
import { UPDATE_NOTE } from "@/store/types.js";
export default {
  created() {
    //     my.tutor=poAo81Rz2Ng5zoeAk9Tl
    // my.student=BoFWhMpV90bLsIDlPhpi
    if (sessionStorage.getItem("role") == "poAo81Rz2Ng5zoeAk9Tl") {
      this.$store.dispatch("backendindex"),
        this.$store.dispatch("mystudentsindex").then(() => {
          this.istutor = true;
        });
    }
    if (sessionStorage.getItem("role") == "BoFWhMpV90bLsIDlPhpi") {
      this.$store.dispatch("studentindex").then(() => {
        this.isstudent = true;
      });
    }
  },
  data() {
    return {
      open1: false,
      open2: false,
      formLabelWidth: "120px",
      password0: null,
      password1: null,
      maxStuNum: null,
      scopeStuNum: null,
      istutor: false,
      isstudent: false
    };
  },
  computed: {
    ...mapState(["tutor"]),
    ...mapState(["mystudents"]),
    ...mapState(["student"]),
    ...mapState(["stutor"]),
    ...mapState(["isUpdatePwd"])

    // sNumber() {
    //   if (this.mystudents.length() == null) {
    //     return 0;
    //   } else return this.mystudents.length();
    // }
  },
  methods: {
    format(percentage) {
      return percentage === this.maxStuNum
        ? "满"
        : `${percentage}` / this.maxStuNum;
    },
    open22() {
      (this.maxStuNum = this.tutor.maxStuNum),
        (this.scopeStuNum = this.tutor.scopeStuNum),
        (this.open2 = true);
    },
    success() {
      this.$message({
        message: "Modified",
        type: "success"
      });
    },
    updatePwd() {
      if (this.password0 == this.password1) {
        this.$store
          .dispatch(UPDATE_PWD, {
            password: this.password0
          })
          .then(() => {
            this.open1 = false;
            this.success();
          });
      } else {
        this.$message.error();
        ("Two inputs do not agree");
      }
    },
    updateNote() {
      this.$store
        .dispatch(UPDATE_NOTE, {
          maxStuNum: this.maxStuNum,
          scopeStuNum: this.scopeStuNum
        })
        .then(() => {
          this.open2 = false;
          this.success();
        });
      // console.log(this.isUpdateNote);
    }
  }
};
</script>

<style scoped>
.container {
  width: 100%;
  padding-top: 30px;
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
.item {
  margin-bottom: 15px;
}
/* 卡片 */

.add-note {
  margin: 50px;
  padding: 50px;
  background-color: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.12), 0 0 6px rgba(0, 0, 0, 0.04);
  border-radius: 20px;
}
.add-note div {
  padding-bottom: 1px;
  font-size: 16px;
}
.add-note > div {
  padding-bottom: 8px;
  font-size: 16px;
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
