<template>
  <div>
    <div class="container">
      <div class="login-form">
        <h1 class="h1">校运会管理系统注册</h1>
        <el-form class="rgs-form">
          <el-form-item label="账号" class="input">
            <el-input v-model="ruleForm.username" autocomplete="off" placeholder="账号" />
          </el-form-item>
          <el-form-item label="密码" class="input">
            <el-input type="password" v-model="ruleForm.password" autocomplete="off" show-password />
          </el-form-item>
          <el-form-item label="重复密码" class="input">
            <el-input type="password" v-model="ruleForm.repetitionPassword" autocomplete="off" show-password />
          </el-form-item>
          <el-form-item label="用户名称" class="input" v-if="tableName === 'yonghu'">
            <el-input v-model="ruleForm.yonghuName" autocomplete="off" placeholder="用户名称" />
          </el-form-item>
          <el-form-item label="用户手机号" class="input" v-if="tableName === 'yonghu'">
            <el-input v-model="ruleForm.yonghuPhone" autocomplete="off" placeholder="用户手机号" />
          </el-form-item>
          <el-form-item label="用户身份证号" class="input" v-if="tableName === 'yonghu'">
            <el-input v-model="ruleForm.yonghuIdNumber" autocomplete="off" placeholder="用户身份证号" />
          </el-form-item>
          <el-form-item label="用户邮箱" class="input" v-if="tableName === 'yonghu'">
            <el-input v-model="ruleForm.yonghuEmail" autocomplete="off" placeholder="用户邮箱" />
          </el-form-item>
          <div style="display: flex;flex-wrap: wrap;width: 100%;justify-content: center;">
            <el-button class="loginInBt" type="primary" @click="login()">注册</el-button>
            <el-button class="loginInBt close" type="primary" @click="close()">取消</el-button>
          </div>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ruleForm: {},
      tableName: "",
      rules: {},
      sexTypesOptions: [],
      yonghuPhotoFlag: false, // 用于刷新文件上传
    };
  },
  mounted() {
    let table = this.$storage.get("loginTable");
    this.tableName = table;
    // 级联表的下拉框查询（若有具体逻辑可补充）
  },
  methods: {
    // 获取uuid
    getUUID() {
      return new Date().getTime();
    },
    yonghuPhotoUploadChange(fileUrls) {
      this.ruleForm.yonghuPhoto = fileUrls;
      if (this.yonghuPhotoFlag) {
        this.yonghuPhotoFlag = false;
      } else {
        this.yonghuPhotoFlag = true;
      }
    },
    close() {
      this.$router.push({ path: "/login" });
    },
    // 注册
    login() {
      if ((!this.ruleForm.username)) {
        this.$message.error('账号不能为空');
        return;
      }
      if ((!this.ruleForm.password)) {
        this.$message.error('密码不能为空');
        return;
      }
      if ((!this.ruleForm.repetitionPassword)) {
        this.$message.error('重复密码不能为空');
        return;
      }
      if (this.ruleForm.repetitionPassword !== this.ruleForm.password) {
        this.$message.error('密码和重复密码不一致');
        return;
      }
      if ((!this.ruleForm.yonghuName) && 'yonghu' === this.tableName) {
        this.$message.error('用户名称不能为空');
        return;
      }
      if ('yonghu' === this.tableName && this.ruleForm.yonghuPhone && (!this.$validate.isMobile(this.ruleForm.yonghuPhone))) {
        this.$message.error('手机应输入手机格式');
        return;
      }
      if ((!this.ruleForm.yonghuIdNumber) && 'yonghu' === this.tableName) {
        this.$message.error('用户身份证号不能为空');
        return;
      }
      if ('yonghu' === this.tableName && this.ruleForm.yonghuEmail && (!this.$validate.isEmail(this.ruleForm.yonghuEmail))) {
        this.$message.error("邮箱应输入邮件格式");
        return;
      }
      this.$http({
        url: `${this.tableName}/register`,
        method: "post",
        data: this.ruleForm
      }).then(({ data }) => {
        if (data && data.code === 0) {
          this.$message({
            message: "注册成功,请登录后在个人中心页面补充个人数据",
            type: "success",
            duration: 1500,
            onClose: () => {
              this.$router.replace({ path: "/login" });
            }
          });
        } else {
          this.$message.error(data.msg);
        }
      });
    }
  }
};
</script>

<style lang="scss" scoped>
// 引入和登录页一致的背景图等基础样式概念
.container {
  min-height: 100vh;
  position: relative;
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;
  // 和登录页背景图保持一致
  background-image: url(/xiaoyunhuiguanli/img/bcbg.png); 

  .login-form {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    box-sizing: border-box;
    width: 400px;
    height: auto;
    padding: 40px 16px;
    border-radius: 12px;
    border-width: 2px;
    border-style: solid;
    border-color: var(--publicSubColor);
    background-color: rgba(255, 255, 255, 0.1);
    box-shadow: 0 0 20px rgba(64, 158, 255, 0.3);
    backdrop-filter: blur(8px);

    .h1 {
      text-align: center;
      color: rgba(255, 255, 255, 1);
      font-size: 20px;
      margin: 0 auto 16px;
      padding: 10px;
      border-radius: 0;
      border-width: 0;
      border-style: solid;
      border-color: transparent;
      background-color: transparent;
      box-shadow: none;
    }

    .rgs-form {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      width: 100%;

      .el-form-item {
        width: 100%;
        display: flex;
        margin-bottom: 12px;

        ::v-deep .el-form-item__content {
          flex: 1;
          display: flex;
        }
      }

      .input {
        width: 100%;
        height: auto;
        padding: 0;
        margin: 0 0 12px 0;
        border-radius: 0;
        border-width: 0;
        border-style: solid;
        border-color: rgba(255, 0, 0, 0);
        background-color: rgba(236, 236, 236, 1);
        box-shadow: 0 0 6px rgba(255, 0, 0, 0);

        ::v-deep .el-form-item__label {
          width: 90px;
          line-height: 44px;
          color: rgba(0, 0, 0, 1);
          font-size: 14px;
          padding: 0 10px 0 0;
          margin: 0;
          border-radius: 0;
          border-width: 0;
          border-style: solid;
          border-color: rgba(255, 0, 0, 0);
          background-color: rgba(255, 0, 0, 0);
          box-shadow: 0 0 6px rgba(255, 0, 0, 0);
        }

        ::v-deep .el-input__inner {
          width: 100%;
          height: 44px;
          line-height: 44px;
          color: #606266;
          font-size: 14px;
          padding: 0 12px;
          margin: 0;
          border-radius: 0;
          border-width: 1px;
          border-style: solid;
          border-color: rgba(112, 112, 112, 1);
          background-color: rgba(255, 255, 255, 0);
          box-shadow: 0 0 6px rgba(255, 0, 0, 0);
          text-align: left;
        }
      }

      .loginInBt {
        width: auto;
        height: auto;
        line-height: auto;
        margin: 0 12px;
        padding: 10px 20px;
        color: rgba(255, 255, 255, 1);
        font-size: 16px;
        border-radius: 8px;
        border-width: 0;
        border-style: solid;
        border-color: #003366;
        background-color: #003366;
        box-shadow: 0 0 0px rgba(255, 0, 0, 0.1);
        transition: all 0.3s ease;
        &:hover {
          background-color: #002244;
          border-color: #002244;
        }
      }

      .close {
        background-color: rgba(255, 255, 255, 0.2);
        color: #333;
        &:hover {
          background-color: rgba(255, 255, 255, 0.3);
        }
      }
    }
  }
}
</style>