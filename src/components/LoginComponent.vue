<template>
  <div>
    <img src="../assets/국민체육진흥공단.jpg" class="logoImg" />

    <div class="loginForm">
      <button
        class="loginFormContent"
        disabled
        style="background-color: #b5b588; cursor: default"
      >
        카카오맵으로 보는 개방학교 체육시설
      </button>
      <button class="loginFormContent" @click="Login()">시작하기</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      User: {
        loginID: "",
        loginPW: "",
      },
    };
  },
  methods: {
    Login() {
      this.$router.push({
        path: "/DetailComponent1Update",
      });
    },
  },
  mounted() {},
};
</script>

<style scoped>
body {
  padding: 0;
  margin: 0;
}

.logoImg {
  width: 100%;
  max-width: 300px;
  margin: auto;
  margin-top: 40px;
  display: block;
}

.loginForm {
  margin-top: 90px;
  width: 100%;
  text-align: center;
}

.loginForm * {
  width: 100%;
  height: 100px;
  max-width: 500px;
  display: block;
  margin: auto;
  margin-bottom: 20px;
  border: none;
  border-radius: 25px;
  font-family: "Inter";
  font-style: normal;
  display: flex;
  align-items: center;
}

.loginForm input {
  font-weight: 500;
  font-size: 20px;
  padding-left: 15px;
  background: rgba(223, 209, 185, 0.2);
}

.loginForm button {
  font-weight: 700;
  font-size: 22px;
  text-align: center;
  justify-content: center;
  color: #ffffff;
  background: rgba(23, 61, 114, 0.85);
  cursor: pointer;
}

.loginForm button:hover {
  background-color: rgb(9 30 59 / 94%);
}

@media screen and (max-width: 1280px) {
  .loginForm * {
    max-width: 380px;
  }
}

@media screen and (max-width: 767px) {
  .loginForm * {
    max-width: 250px;
  }

  .loginForm input {
    font-size: 16px;
  }

  .loginForm button {
    font-size: 14px;
    font-weight: 700;
  }

  .logoImg {
    width: 100%;
    max-width: 270px;
    margin: auto;
    display: block;
  }
}

@media screen and (min-height: 1000px) {
  .logoImg {
    margin-top: 100px;
  }
}

@media screen and (max-height: 620px) {
  .logoImg {
    max-width: 200px;
  }

  .loginForm * {
    height: 60px;
  }
}
</style>
