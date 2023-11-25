<template>
  <div>
    <div class="black-bg" v-if="moreInfoModalView">
      <div class="white-bg">
        <p id="schoolName">{{ moreInfoModalContent.schoolName }}</p>
        <p>{{ moreInfoModalContent.addr }}</p>
        <p>코트 면적 : {{ moreInfoModalContent.cotvalue }}</p>
        <p>제공장비 : {{ moreInfoModalContent.item }}</p>
        <Table>
          <tbody>
            <tr>
              <th>라커룸</th>
              <th>샤워실</th>
              <th>화장실</th>
            </tr>
            <tr>
              <td>{{ moreInfoModalContent.locker }}개</td>
              <td>{{ moreInfoModalContent.shower }}개</td>
              <td>{{ moreInfoModalContent.toilet }}개</td>
            </tr>
          </tbody>
        </Table>
        <Table>
          <tbody>
            <tr>
              <th v-for="(rs, i) in moreInfoModalContent.day" :key="i">
                {{ rs }}
              </th>
            </tr>
            <tr>
              <td v-for="(rs, i) in moreInfoModalContent.dayTime" :key="i">
                {{ rs }}
              </td>
            </tr>
          </tbody>
        </Table>
        <button @click="moreInfoModalView = 0">닫기</button>
      </div>
    </div>
    <div class="black-bg" v-if="searchSchoolModalView">
      <div class="white-bg">
        <v-text-field
          label="Place"
          v-model="place"
          style="margin-top: 0px; padding-top: 0px"
          @click="showSelected = true"
          readonly
        ></v-text-field>
        <button @click="searchSchoolModalView = 0">닫기</button>
      </div>
    </div>
    <div
      style="
        width: 95%;
        display: flex;
        flex-direction: row;
        justify-content: flex-start;
        margin-top: 18px;
        margin-left: 35px;
      "
    >
      <!-- grid1 시작지점 -->
      <v-row>
        <v-col cols="12" sm="4">
          <div
            style="
              display: flex;
              flex-direction: row;
              justify-content: flex-start;
              margin-left: 1px;
            "
          >
            <v-card
              class="mx-auto"
              style="
                height: 655px;
                width: 100%;
                overflow-y: auto;
                margin-left: 5px;
              "
              tile
            >
              <div>
                <v-icon color="indigo darken-4" large> mdi-town-hall </v-icon>
                <v-subheader style="color: black; margin-left: 20px"
                  >개방학교 검색</v-subheader
                >
                <button
                  style="background-color: red"
                  @click="searchSchoolModalView = 1"
                >
                  학교조건검색
                </button>
              </div>
              <v-list dense style="padding: 0">
                <div v-for="(rs, i) in initSchoolData" :key="i">
                  <v-list-item>
                    <v-list-item-content>
                      <v-card elevation="5" outlined style="margin: 2px 0">
                        <div
                          style="
                            display: flex;
                            flex-direction: row;
                            justify-content: flex-start;
                            color: white;
                          "
                        >
                          <div class="planNumber">
                            <h4>{{ i + 1 }}</h4>
                          </div>
                          <v-list-item-title
                            style="
                              margin: 7px 0px 15px 9px;
                              width: 80%;
                              cursor: pointer;
                              color: black;
                            "
                            @click="setCenter2(rs)"
                            >{{ rs.schul_nm }}</v-list-item-title
                          >
                          <v-btn
                            style="
                              width: 15%;
                              min-width: none;
                              padding: 0%;
                              color: red;
                            "
                            @click="moreInfo(rs)"
                            ><v-icon color="red" large>
                              mdi-file-search-outline
                            </v-icon></v-btn
                          >
                        </div>
                        <div style="font-size: small; height: 23px">
                          {{ rs.alsfc_addr }}
                        </div>
                      </v-card>
                    </v-list-item-content>
                  </v-list-item>
                </div>
              </v-list>
            </v-card>
          </div>
        </v-col>
        <v-col cols="12" sm="8" style="max-width: 100%">
          <!-- grid1 종료지점 -->
          <!-- grid2 시작지점 -->
          <div class="maparea">
            <div class="categoryList">
              <div class="categoryElement" @click="changeCartegory('AT4')">
                <v-icon color="indigo darken-4" large> mdi-bank </v-icon>
                <span style="color: black">관광지</span>
              </div>
              <div class="categoryElement" @click="changeCartegory('PK6')">
                <v-icon color="red"> mdi-parking </v-icon>
                <span style="color: black">주차장</span>
              </div>
              <div class="categoryElement" @click="changeCartegory('SW8')">
                <v-icon color="red"> mdi-subway-variant </v-icon>
                <span style="color: black">지하철</span>
              </div>
              <div class="categoryElement" @click="changeCartegory('MT1')">
                <v-icon color="red"> mdi-cart </v-icon>
                <span style="color: black">마트</span>
              </div>
              <div class="categoryElement" @click="changeCartegory('FD6')">
                <v-icon color="red"> mdi-food </v-icon>
                <span style="color: black">식당</span>
              </div>
              <div class="categoryElement" @click="changeCartegory('AD5')">
                <v-icon color="red"> mdi-home </v-icon>
                <span style="color: black">숙소</span>
              </div>
              <div class="categoryElement" @click="changeCartegory('CE7')">
                <v-icon color="red"> mdi-coffee </v-icon>
                <span style="color: black">카페</span>
              </div>
            </div>
            <div
              style="
                margin-left: auto;
                margin-bottom: 50px;
                padding: 0px 12px;
                position: absolute;
                z-index: 3;
                right: 0;
                top: 11px;
                border-radius: 10px;
              "
            ></div>
            <div id="map">
              <div
                style="width: 330px; height: 0px; border-radius: 3px"
                ref="customOverlay"
              >
                <div
                  style="
                    display: flex;
                    flex-direction: row;
                    justify-content: flex-start;
                    background-color: white;
                    color: black;
                    box-shadow: 0 1px 1px rgba(0, 0, 0, 0.8);
                  "
                >
                  <v-list-item-title
                    style="
                      margin: 7px 0px 7px 9px;
                      width: 80%;
                      color: black;
                      font-weight: 700;
                    "
                    >{{ markerInfo.place_name }}</v-list-item-title
                  >
                  <v-btn
                    style="width: 10%; padding: 0%; color: black"
                    @click="addPlan(markerInfo)"
                  >
                    <v-icon color="indigo darken-4" large>
                      mdi-plus-circle
                    </v-icon>
                  </v-btn>
                </div>
                <div
                  class="addr"
                  style="
                    background-color: white;
                    padding: 2px 0px 0px 8px;
                    box-shadow: 0 1px 1px rgba(0, 0, 0, 0.8);
                  "
                >
                  <h5
                    style="
                      text-overflow: ellipsis;
                      overflow: hidden;
                      white-space: nowrap;
                      font-size: 10px;
                      font-weight: 300;
                      background-color: white;
                    "
                  >
                    {{ markerInfo.address }}
                  </h5>
                  <a
                    :href="markerInfo.url"
                    target="_blank"
                    style="
                      text-overflow: ellipsis;
                      overflow: hidden;
                      white-space: nowrap;
                      font-size: 10px;
                      font-weight: 700;
                      color: #1976d2a1;
                    "
                  >
                    상세보기
                  </a>
                </div>
              </div>
            </div>
          </div>
          <!-- grid2 종료지점 -->
        </v-col>
      </v-row>
    </div>
  </div>
</template>
  
  <script>
import dataSet from "../assets/data.js";
export default {
  data() {
    return {
      placeSelect: [
        "서울",
        "대전",
        "대구",
        "부산",
        "광주",
        "울산",
        "인천",
        "경기도",
        "강원도",
        "경상남도",
        "전라도",
        "제주도",
        "경상북도",
        "충청도",
      ],
      moreInfoModalView: 0,
      searchSchoolModalView: 0,
      moreInfoModalContent: {
        addr: "",
        city: "",
        signgu: "",
        cotvalue: "",
        baseYear: "",
        locker: 0,
        shower: 0,
        toilet: 0,
        item: "",
        time: [],
        day: [],
        dayTime: [],
        schoolName: "",
      },
      modal: 0,
      title: "",
      intro: "",
      end_date: "",
      curDate: "",
      dayCnt: 0,
      startDatePicker: 0,
      endDatePicker: 0,
      time: null,
      schoolData: dataSet,
      initSchoolData: [],
      timepicker: null,
      timepickerTime: "00:00",
      showDialog: false,
      showSelected: false,
      showSearch: true,
      width: 0,
      curCartegory: "AT4",
      selectedItem: 0,
      markers: [],
      dateResult: [],
      place: [],
      positions: [],
      mapOption: {
        center: {
          lat: 33.450701,
          lng: 126.570667,
        },
        level: 8,
      },
      search: {
        keyword: null,
        pgn: null,
        results: [],
      },
      markerInfo: {
        place_name: "",
        address: "",
        url: "",
        x: "",
        y: "",
      },
      customOverlay: {},
      planner: {
        title: "",
        intro: "",
        start_date: "",
        end_date: "",
        nickname: "",
        concept: "",
        placeList: [],
        planList: [],
        visibility: "VTP200Y",
      },
      map: null,
      plan: [],
      plan2: [],
    };
  },
  watch: {},
  mounted() {
    for (let i = 0; i < 30; i++) {
      this.initSchoolData.push(this.schoolData[i]);
    }
    console.log(this.initSchoolData);
    if (window.kakao && window.kakao.maps) {
      // 카카오 객체가 있고, 카카오 맵 그릴 준비가 되어 있다면 맵 실행
      this.loadMap();
    } else {
      // 없다면 카카오 스크립트 추가 후 맵 실행
      this.loadScript();
    }
    console.log(this.$route.params.plan);
    if (this.$route.params.plan != undefined) {
      this.title = this.$route.params.plan.title;
      this.intro = this.$route.params.plan.intro;
      this.start_date = this.$route.params.plan.start_date;
      this.end_date = this.$route.params.plan.end_date;
      this.planner.concept = this.$route.params.plan.concept;
      this.$route.params.plan.placeList.forEach((a) => {
        this.place.push(a.place);
      });
    }
    if (window.innerWidth < 600) {
      this.showSearch = false;
      let target = document.getElementsByClassName("categoryList");
      target.style.width = "300px";
    }
    window.addEventListener("resize", this.handleResize);
  },
  methods: {
    moreInfo(rs) {
      this.moreInfoModalContent.addr = rs.alsfc_addr;
      this.moreInfoModalContent.city = rs.alsfc_ctprvn_nm;
      this.moreInfoModalContent.signgu = rs.alsfc_signgu_nm;
      this.moreInfoModalContent.cotvalue = rs.arby_cot_co_value;
      this.moreInfoModalContent.baseYear = rs.base_year;
      this.moreInfoModalContent.locker = rs.lockerrm_co;
      this.moreInfoModalContent.shower = rs.shwerrm_co;
      this.moreInfoModalContent.toilet = rs.toilet_co;
      this.moreInfoModalContent.item = rs.oper_item_cn;
      this.moreInfoModalContent.schoolName = rs.schul_nm;
      this.moreInfoModalView = 1;

      let arr = rs.oper_time_cn.split(" ").filter((value) => {
        return value.length != 0;
      });
      this.moreInfoModalContent.time = [];
      this.moreInfoModalContent.day = [];
      this.moreInfoModalContent.dayTime = [];
      arr.map((e) => {
        this.moreInfoModalContent.time.push(e);
        let a = e.split("일:");
        this.moreInfoModalContent.day.push(`${a[0]}일`);
        this.moreInfoModalContent.dayTime.push(a[1]);
        console.log(this.moreInfoModalContent.day);
      });
    },
    setCenter2(rs) {
      var geocoder = new window.kakao.maps.services.Geocoder();
      geocoder.addressSearch(rs.alsfc_addr, (result, status) => {
        console.log(result);
        // 정상적으로 검색이 완료됐으면
        if (status === window.kakao.maps.services.Status.OK) {
          var coords = new window.kakao.maps.LatLng(result[0].y, result[0].x);
          this.map.setCenter(coords);

          // 결과값으로 받은 위치를 마커로 표시합니다
          var marker = new window.kakao.maps.Marker({
            map: this.map,
            position: coords,
          });

          // 인포윈도우로 장소에 대한 설명을 표시합니다
          var infowindow = new window.kakao.maps.InfoWindow({
            content: `<div style="width:150px;text-align:center;padding:6px 0;">${rs.schul_nm}</div>`,
          });
          infowindow.open(this.map, marker);
        }
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
      window.kakao.maps.event.addListener(
        this.map,
        "idle",
        this.searchCartegory
      );
      this.searchCartegoryForLoadMap();
    },
    searchPlaces(e) {
      const keyword = e.target.value.trim();
      if (keyword.length === 0) {
        return;
      }

      const ps = new window.kakao.maps.services.Places();
      ps.keywordSearch(keyword, (data, status, pgn) => {
        this.search.keyword = keyword;
        this.search.pgn = pgn;
        this.search.results = data;
      });
    },
    addPlan(rs) {
      let obj = {};
      obj.name = rs.place_name;
      obj.memo = "";
      obj.date = this.curDate;
      obj.x = rs.x;
      obj.y = rs.y;
      this.planner.planList.push(obj);
      this.sortDate1(this.planner.planList);
    },
    setCenter(rs) {
      // 이동할 위도 경도 위치를 생성합니다
      let moveLatLon = new window.kakao.maps.LatLng(rs.y, rs.x);

      // 지도 중심을 이동 시킵니다
      this.map.setCenter(moveLatLon);
    },
    showDate(rs) {
      rs.view = 1;
      this.curDate = rs.date;
    },
    checkDay(rs) {
      if (rs.date.substring(0, 10) == this.curDate.substring(0, 10)) {
        return true;
      } else {
        return false;
      }
    },
    doneTimePicker(rs) {
      let date = rs.date.substring(0, 11);
      rs.date = `${date}${this.timepickerTime}`;
      this.timepicker = null;
      this.timepickerTime = "00:00";
      this.planner.planList = this.sortDate1(this.planner.planList);
    },
    getOrderDate(idx) {
      return this.planner.planList[idx].date.substring(11, 16);
    },
    checkVisibility() {
      if (this.planner.visibility == "VTP200Y") {
        return true;
      } else {
        return false;
      }
    },
    searchCartegoryForLoadMap() {
      this.customOverlay = new window.kakao.maps.CustomOverlay({});
      let ps2 = new window.kakao.maps.services.Places(this.map);
      ps2.categorySearch(this.curCartegory, this.placesSearchCB, {
        useMapBounds: true,
      });
    },
    searchCartegory() {
      this.removeMarkers();
      this.customOverlay.setMap(null);
      this.positions.length = 0;
      this.markers.length = 0;
      let ps2 = new window.kakao.maps.services.Places(this.map);
      ps2.categorySearch(this.curCartegory, this.placesSearchCB, {
        useMapBounds: true,
      });
    },
    placesSearchCB(data) {
      console.log(data[0]);
      for (let i = 0; i < data.length; i++) {
        this.displayMarker(data[i]);
      }
      setTimeout(() => {
        for (let i = 0; i < this.positions.length; i++) {
          // 마커 이미지의 이미지 크기 입니다
          var imageSize = new window.kakao.maps.Size(24, 35);
          var imageSrc =
            "https://t1.daumcdn.net/localimg/localimages/07/mapapidoc/markerStar.png";

          // 마커 이미지를 생성합니다
          var markerImage = new window.kakao.maps.MarkerImage(
            imageSrc,
            imageSize
          );
          var iwContent = '<div style="padding:5px;">hello world</div>', // 인포윈도우에 표출될 내용으로 HTML 문자열이나 document element가 가능합니다
            iwRemoveable = true; // removeable 속성을 ture 로 설정하면 인포윈도우를 닫을 수 있는 x버튼이 표시됩니다

          // 인포윈도우를 생성합니다
          var infowindow = new window.kakao.maps.InfoWindow({
            content: iwContent,
            removable: iwRemoveable,
          });
          console.log(infowindow);
          // 마커를 생성합니다
          let marker = new window.kakao.maps.Marker({
            map: this.map, // 마커를 표시할 지도
            position: this.positions[i].latlng, // 마커를 표시할 위치
            title: this.positions[i].title, // 마커의 타이틀, 마커에 마우스를 올리면 타이틀이 표시됩니다
            image: markerImage, // 마커 이미지
          });

          /* 마커에 클릭이벤트 등록 */
          window.kakao.maps.event.addListener(marker, "click", () => {
            this.displayPlaceInfo(this.positions[i]);
          });
          this.markers.push(marker);
        }
      }, 100);
    },
    displayMarker(place) {
      let markerPosition = new window.kakao.maps.LatLng(place.y, place.x);
      let obj = {
        title: place.place_name,
        latlng: markerPosition,
        address: place.address_name,
        x: place.x,
        y: place.y,
        url: place.place_url,
      };
      this.positions.push(obj);
    },
    removeMarkers() {
      for (let i = 0; i < this.markers.length; i++) {
        this.markers[i].setMap(null);
      }
    },
    changeCartegory(car) {
      this.customOverlay.setMap(null);
      this.removeMarkers();
      this.markers.length = 0;
      this.curCartegory = car;
      this.searchCartegory();
    },
    displayPlaceInfo(place) {
      this.markerInfo.place_name = place.title;
      this.markerInfo.address = place.address;
      this.markerInfo.url = place.url;
      this.markerInfo.x = place.x;
      this.markerInfo.y = place.y;

      var content = this.$refs.customOverlay;
      this.customOverlay = new window.kakao.maps.CustomOverlay({
        position: place.latlng,
        content: content,
      });
      // 커스텀 오버레이를 지도에 표시합니다
      this.customOverlay.setMap(this.map);
    },
    sortDate1(list) {
      const sorted_list = list.sort((a, b) => {
        console.log(a);
        return new Date(a.date).getTime() - new Date(b.date).getTime();
      });
      return sorted_list;
    },
    goComponents(path) {
      this.$router.push({ path: `/${path}` });
    },
    handleResize() {
      if (window.innerWidth < 600) {
        this.showSearch = false;
      } else {
        this.showSearch = true;
      }
    },
  },
};
</script>
  
  <style scoped>
#schoolName {
  font-size: 26px;
  font-weight: 800;
}
.black-bg {
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  z-index: 60000;
  position: fixed;
  padding: 20px;
}
.white-bg {
  width: 70%;
  height: 70%;
  justify-content: center;
  margin: auto;
  margin-top: 40px;
  background: white;
  border-radius: 8px;
  padding: 20px;
  display: flex;
  flex-direction: column;
}

.white-bg table {
  width: 40%;
  border: 1px solid #444444;
  margin: auto;
}

.white-bg td {
  font-size: 0.7em;
  border: 1px solid #444444;
}

.white-bg p {
  margin: auto;
}
.moreInfoModal {
  width: 100%;
  height: 100%;
  background-color: lightgray;
}
#map {
  flex: 1 1 auto;
  height: 655px;
}
.maparea {
  display: flex;
  position: relative;
}
.maparea h4 {
  margin: 0;
}
.maparea h5 {
  margin: 0;
}
.searchbox {
  position: absolute;
  top: 0;
  left: 0;
  height: 600px;
  z-index: 10000;
  /*  background-color: #ffffffaa;
    color: rgb(21, 20, 20); */
  overflow-y: auto;
  width: 250px;
  display: flex;
  flex-direction: column;
}

.searchbox::-webkit-scrollbar {
  display: none; /* Chrome, Safari, Opera*/
}

.v-text-field::lable {
  color: aliceblue;
}

.place {
  padding: 8px;
}
.plan {
  top: 0;
  left: 0;
  background-color: #ffffffaa;
  overflow-y: auto;
  width: 250px;
  display: flex;
  flex-direction: column;
  margin: 5px;
}

.plan h4 {
  margin: 0;
}
.plan input {
  margin: 3px;
}

.planner button {
  margin: 3px;
}
.planNumber {
  border-radius: 50%;
  border: none;
  background: #7c30f5e6;
  color: white;
  width: 2vw;
  height: 1vw;
  -webkit-box-pack: center;
  justify-content: center;
  margin-top: 9px;
  display: flex;
  -webkit-box-align: center;
  margin-left: 12px;
  padding-bottom: 1px;
  align-items: center;
}
.planNumber h4 {
  margin: 0;
  font-size: 8px;
  line-height: 1vw;
  text-align: center;
  width: 1vw;
}
.black-bg {
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  z-index: 60000;
  position: fixed;
  padding: 20px;
}
.white-bg {
  width: 70%;
  background: white;
  border-radius: 8px;
  padding: 20px;
  display: flex;

  flex-direction: column;
}

.white-bg input {
  width: 80%;
  margin: 5px;
  vertical-align: middle;
}

.dateResult h4 {
  cursor: pointer;
}

.searchInput {
  color: black;
  font-size: small;
  width: 100%;
  padding-left: 10px;
  padding-right: 5px;
  height: 40px;
  /*   background-color: rgb(196 202 196 / 75%);
   */
  margin-left: 20px;
}

.searchInput::placeholder {
  color: rgba(0, 0, 0, 0.35);
  font-size: 12px;
}
.searchInput:focus {
  outline: none;
}

.mx-auto::-webkit-scrollbar {
  display: none; /* Chrome, Safari, Opera*/
}

.cartegoryBtn {
  position: absolute;
  z-index: 2;
  padding: 50px;
  margin: 0px;
  width: 50px;
}
.categoryList {
  position: absolute;
  top: 10px;
  left: 10px;
  border-radius: 5px;
  border: 1px solid #909090;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.4);
  background-color: white;
  color: white;
  overflow: hidden;
  z-index: 2;
  padding: 12px 15px;
  width: fit-content;
}
.categoryElement {
  float: left;
  list-style: none;
  width: 70px;
  text-align: center;
  cursor: pointer;
  font-size: 13px;
  font-weight: bold;
}
.categoryElement i {
  margin-right: 5px;
}
.cancelBtn {
  font-family: "Inter";
  border-radius: 8px;
  width: 63px;
  height: 36px;
  font-size: 14px;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.4);
}
.b1 {
  background-color: white;
  color: black;
  height: 43px;
  border: solid 1px;
}
.b2 {
  background-color: none;
  color: black;
  border: solid 1px black;
}
.searchIcon {
  position: absolute;
  /* background-color: rgb(196 202 196 / 72%); */
  height: 40px;
  width: 20px;
  /* color: white; */
  padding-top: 10px;
  padding-left: 9px;
  z-index: 3;
  font-size: 13px;
}
</style>