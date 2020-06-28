<template>
  <div style="width:100%" class="container">
    <el-menu
      default-active="1"
      class="el-menu-vertical-demo"
      @open="handleOpen"
      @close="handleClose"
      background-color="#1A237E"
      text-color="#339999"
      active-text-color="#6666cc"
      router
    >
      <h2>Selection System</h2>
      <!-- default-active 当前激活菜单的 index -->
      <el-menu-item index="1" route="/homepage">
        <i class="el-icon-info"></i>
        <span slot="title">My Imformations</span>
      </el-menu-item>
      <el-menu-item index="2" route="/students" v-if="tutor">
        <i class="el-icon-user"></i>
        <span slot="title">Students</span>
      </el-menu-item>
      <el-menu-item index="3" route="/courses" v-if="tutor">
        <i class="el-icon-s-order"></i>
        <span slot="title">Courses</span>
      </el-menu-item>
      <el-menu-item index="4" route="/directions" v-if="tutor">
        <i class="el-icon-s-promotion"></i>
        <span slot="title">Directions</span>
      </el-menu-item>
      <el-menu-item index="5" route="/choice" v-if="student">
        <i class="el-icon-s-custom"></i>
        <span slot="title">Choose Tutor</span>
      </el-menu-item>
    </el-menu>
    <!-- </el-col>
    </el-row>-->
  </div>
</template>

<style scoped>
.container {
  padding: 0px 0px 20px 20px;
  height: 1000px;
}
h2 {
  color: #f5f5f5;
  background-color: #1a237e;
  padding: 25px;
}
.el-menu {
  height: 700px;
  border-radius: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.12), 0 0 6px rgba(0, 0, 0, 0.04);
}
</style>

<script>
import { mapState } from "vuex";
export default {
  created() {
    if (sessionStorage.getItem("role") == "6983f953b49c88210cb9") {
      // console.log(sessionStorage.getItem("role"));
      this.$store.dispatch("backendindex");
    }
    if (sessionStorage.getItem("role") == "bb63e5f7e0f2ffae845c") {
      this.$store.dispatch("studentindex");
    }
  },
  computed: {
    ...mapState(["tutor"]),
    ...mapState(["student"])
  },
  methods: {
    handleOpen(key, keyPath) {
      console.log(key, keyPath);
    },
    handleClose(key, keyPath) {
      console.log(key, keyPath);
    }
  }
};
</script>
