<template>
  <div class="maparea">
    <div class="searchDiv"></div>
    <div id="map"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      map: null,
    };
  },
  methods: {
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
    setCenter() {
      // 이동할 위도 경도 위치를 생성합니다
      let moveLatLon = new window.kakao.maps.LatLng(33.450701, 126.570667);

      // 지도 중심을 이동 시킵니다
      this.map.setCenter(moveLatLon);

      var imageSize = new window.kakao.maps.Size(24, 35);
      var imageSrc =
        "https://t1.daumcdn.net/localimg/localimages/07/mapapidoc/markerStar.png";

      // 마커 이미지를 생성합니다
      var markerImage = new window.kakao.maps.MarkerImage(imageSrc, imageSize);
      // 마커가 표시될 위치입니다
      let markerPosition = new window.kakao.maps.LatLng(33.450701, 126.570667);

      // 마커를 생성합니다
      let marker = new window.kakao.maps.Marker({
        position: markerPosition,
        image: markerImage, // 마커 이미지
      });

      // 마커가 지도 위에 표시되도록 설정합니다
      marker.setMap(this.map);
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
@media (hover: hover) and (pointer: fine) {
  .registration button:hover {
    background: #605b5bbd;
    color: white;
  }

  .search button:hover {
    background: #605b5bbd;
    color: white;
  }
}

.searchDiv {
  width: 100%;
  height: 30%;
  border: 1px solid red;
}

#map {
  width: 100%;
  height: 100%;
}
.maparea {
  flex: 1 1 auto;
  margin-right: 20px;
  margin-left: 20px;
  margin-top: 20px;
  margin-bottom: 40px;
}

.search button {
  background: #d9d9d9bd;
  border-radius: 25px;
  width: 70px;
  height: 70%;
  border: none;
  font-weight: 600;
  font-size: 16px;
}

.search input {
  padding-left: 10px;
  height: 100%;
  border: none;
  background-color: transparent;
  outline: none;
  font-family: "Inter";
  font-size: 14px;
  max-width: 167px;
}

.search input:focus {
  border: none;
}

.registration {
  display: flex;
  align-items: center;
  height: 80px;
  flex-wrap: wrap;
}

.registration button {
  background: rgba(217, 217, 217, 0.5);
  border-radius: 25px;
  border: none;
  width: 180px;
  height: 60px;
  font-family: "Inter";
  font-size: 17px;
  font-weight: 600;
  line-height: 18px;
}

.teacherReg {
  margin-right: 5px;
}

.mainContent {
  display: flex;
  justify-content: center;
  align-items: center;
}

@media screen and (max-width: 767px) {
  .userInfo {
    padding-left: 10px;
    padding-right: 15px;
  }

  .search {
    margin-left: 0;
  }

  .search button {
    width: 40px;
    height: 70%;
    font-weight: 500;
    font-size: 13px;
  }

  .search input {
    padding-left: 5px;
    height: 80%;
    font-family: "Inter";
    font-size: 12px;
    max-width: 167px;
  }

  .registration button {
    width: 110px;
    height: 60px;
    font-weight: 500;
    font-size: 11px;
  }
}

@media screen and (max-width: 500px) {
  .teacherReg {
    margin-right: 0px;
    margin-bottom: 2px;
  }

  .registration {
    display: flex;
    align-items: center;
    height: 80px;
    flex-direction: column;
    flex-wrap: nowrap;
  }
}
</style>
