<template>
  <div class="container">
    <el-row :gutter="24">
      <el-col :span="13">
        <div class="grid-content bg-purple title">
          <h2>Welcome To</h2>
          <h2>Seletion System</h2>
        </div>
      </el-col>
      <el-col :span="2">
        <div class="grid-content bg-purple title"></div>
      </el-col>
      <el-col :span="8">
        <div class="grid-content bg-purple-light login-input">
          <form ref="form">
            <label>User Number</label>
            <el-input type="text" v-model="number"></el-input>
            <br />
            <br />
            <label>Password</label>
            <el-input type="password" v-model="password"></el-input>
            <br />
            <el-button @click="login" round size="medium" type="success">
              Login
            </el-button>
          </form>
        </div>
      </el-col>
      <el-col :span="2">
        <div class="grid-content bg-purple title"></div>
      </el-col>
    </el-row>
    <el-row>
      <el-col :span="20">
        <div class="grid-content bg-purple add-div"></div>
      </el-col>
    </el-row>
  </div>
</template>

<style scoped>
.container {
  height: 100%;
  background-image: url("../image/login2.jpg");
  background-size: cover;
}
.el-col {
  padding-top: 150px;
}
.title {
  margin-top: 80px;
  text-align: center;
  font-size: 30px;
}
.login-input {
  padding: 50px 50px 150px 50px;
  background-color: aliceblue;
  border-radius: 20px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.12), 0 0 6px rgba(0, 0, 0, 0.04);
  /* text-align: center; */
}
h2 {
  color: white;
}
.el-input {
  margin-bottom: 20px;
}
.el-button {
  margin: 35px 0px;
  float: right;
}
.add-div {
  background-color: darkgrey;
  /* background-color: gray; */
  border-radius: 10px;
  height: 260px;
  opacity: 0.7;
}
</style>

<script>
import { LOGIN } from "@/store/types.js";
export default {
  data: () => ({
    number: null,
    password: null
  }),
  methods: {
    login() {
      this.$store
        .dispatch(LOGIN, {
          number: this.number,
          password: this.password
        })
        .then(() => {
          this.$router.push("/homepage");
        });
    }
  }
};
</script>
