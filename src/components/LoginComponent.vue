<template>
  <div>
    <img src="../assets/background_logo.png" class="logoImg" />
    <div class="maparea">
      <div id="map"></div>
    </div>

    <div class="loginForm">
      <input
        placeholder="ID"
        class="loginFormContent"
        v-model="User.loginID"
        @keyup.enter="Login()"
      />

      <input
        type="password"
        placeholder="PW"
        class="loginFormContent"
        @keyup.enter="Login()"
        v-model="User.loginPW"
      />

      <button class="loginFormContent" @click="Login()">로그인</button>
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
        map: null,
      },
    };
  },
  methods: {
    Login() {
      this.$router.push({
        path: "/mainpage",
      });
    },
    loadScript() {
      const script = document.createElement("script");
      script.src =
        "//dapi.kakao.com/v2/maps/sdk.js?appkey=cf334cb575788ef58634b31f39108c2e&libraries=services&autoload=false";
      script.onload = () => window.kakao.maps.load(this.loadMap); //script load가 끝난 뒤 지도 실행

      document.head.appendChild(script);
    },
    loadMap() {
      // 지도를 담을 DOM 영역
      const container = document.getElementById("map");
      // 지도 초기 정보
      const options = {
        center: new window.kakao.maps.LatLng(33.450701, 126.570667),
        level: 3,
      };
      this.map = new window.kakao.maps.Map(container, options);
    },
  },
  mounted() {
    if (window.kakao && window.kakao.maps) {
      // 카카오 객체가 있고, 카카오 맵 그릴 준비가 되어 있다면 맵 실행
      this.loadMap();
    } else {
      // 없다면 카카오 스크립트 추가 후 맵 실행
      this.loadScript();
    }
  },
};
</script>

<style scoped>
#map {
  flex: 1 1 auto;
  height: 600px;
  height: 655px;
}
.maparea {
  margin-right: 30px;
  margin-left: 20px;
}

body {
  padding: 0;
  margin: 0;
}

.logoImg {
  width: 100%;
  max-width: 300px;
  margin: auto;
  display: block;
}

.loginForm {
  margin-top: 5px;
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
  font-size: 27px;
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
    font-size: 22px;
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
