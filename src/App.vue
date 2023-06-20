<template>
  <!-- <side-bar/> -->
  <div class="container">
    <side-bar style="width: 24%" />
    <div class="content">
      <div class="search__area">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="16"
          fill="currentColor"
          class="bi bi-search"
          viewBox="0 0 16 16"
        >
          <path
            d="M11.742 10.344a6.5 6.5 0 1 0-1.397 1.398h-.001c.03.04.062.078.098.115l3.85 3.85a1 1 0 0 0 1.415-1.414l-3.85-3.85a1.007 1.007 0 0 0-.115-.1zM12 6.5a5.5 5.5 0 1 1-11 0 5.5 5.5 0 0 1 11 0z"
          />
        </svg>
        <input
          class="search"
          type="text"
          placeholder="Bạn muốn nghe gì?"
          v-model="search_value"
          @keyup="search_song"
        />
        <svg
          v-if="search_value != ''"
          @click="search_value = ''"
          aria-hidden="true"
          focusable="false"
          data-prefix="fas"
          data-icon="circle-xmark"
          class="svg-inline--fa fa-circle-xmark"
          role="img"
          xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 512 512"
        >
          <path
            fill="currentColor"
            d="M0 256C0 114.6 114.6 0 256 0C397.4 0 512 114.6 512 256C512 397.4 397.4 512 256 512C114.6 512 0 397.4 0 256zM175 208.1L222.1 255.1L175 303C165.7 312.4 165.7 327.6 175 336.1C184.4 346.3 199.6 346.3 208.1 336.1L255.1 289.9L303 336.1C312.4 346.3 327.6 346.3 336.1 336.1C346.3 327.6 346.3 312.4 336.1 303L289.9 255.1L336.1 208.1C346.3 199.6 346.3 184.4 336.1 175C327.6 165.7 312.4 165.7 303 175L255.1 222.1L208.1 175C199.6 165.7 184.4 165.7 175 175C165.7 184.4 165.7 199.6 175 208.1V208.1z"
          ></path>
        </svg>
        <div class="result__search" v-if="search_value != ''">
          <small>Gợi ý kết quả</small>
          <ul
            class="list_songs_search"
            v-for="song of list_songs_search"
            :key="song.encodeId"
          >
            <li class="song_search" @click="ChooseSong(song)">
              <img class="img_song_search" :src="song.thumbnailM" alt="" />
              <span class="infor_song_search">
                {{ song.title }}
                <small class="artistsNames">{{ song.artistsNames }}</small>
              </span>
            </li>
          </ul>
        </div>
      </div>
      <div class="content__title">
        <div class="title__small">
          <p>What's hot</p>
          <img src="../src/assets/images/fire.png" alt="" />
        </div>
        <div class="title__big">
          <h3>Trending</h3>
          <div class="more">
            More
            <svg
              xmlns="http://www.w3.org/2000/svg"
              height="1em"
              viewBox="0 0 320 512"
            >
              <path
                d="M310.6 233.4c12.5 12.5 12.5 32.8 0 45.3l-192 192c-12.5 12.5-32.8 12.5-45.3 0s-12.5-32.8 0-45.3L242.7 256 73.4 86.6c-12.5-12.5-12.5-32.8 0-45.3s32.8-12.5 45.3 0l192 192z"
              />
            </svg>
          </div>
        </div>
      </div>
      <div class="content__banner">
        <p class="title__small">Artist</p>
        <h3>
          On top
          <br />
          The world
        </h3>
        <div class="banner__sub">
          <div class="button">
            <button class="play">PLAY</button>
            <button class="follow">FOLLOW</button>
          </div>
          <div class="banner__sub--title">
            Monthly Listener
            <br />
            50.000
          </div>
        </div>
      </div>
      <div class="content__playlist">
        <h3>My Playlist</h3>
        <ul class="playlist playlist_title">
          <li class="col-1">#</li>
          <li class="col-2">TITLE</li>
          <li class="col-3">ARTIST</li>
        </ul>
        <div class="list__songs">
          <ul
            class="playlist playlist__content"
            v-for="(song, index) in listSongs"
            :key="index"
            @click="ChooseSong(song)"
          >
            <li class="col-1">{{ ++index }}</li>
            <li class="col-2">{{ song.title }}</li>
            <li class="col-3">{{ song.artistsNames }}</li>
            <!-- <li :class="song.encodeId" class="song" @click="ChooseSong(song)">
                <span class="index_song">{{ ++index }}</span>
                <img :src="song.thumbnailM" alt="">
                <span class="infor_song">
                    {{ song.title }}
                <small class="artistsNames">{{ song.artistsNames }}</small>
                </span>
            </li> -->
          </ul>
        </div>
      </div>
      <play-bar 
      :url_song="songUrl"
      :thumbnailM="thumbnailM"
      :name_song="songName"
      :artist_Name="artistName"  
      />
    </div>
    <div class="top__Trending">
      <div class="shortcuts">
        <p class="title__small">Shortcuts</p>
        <div class="shortcuts__items">
          <div class="item">
            Chill
            <svg
              xmlns="http://www.w3.org/2000/svg"
              height="1em"
              viewBox="0 0 448 512"
              style="fill: rgb(55, 149, 203)"
            >
              <path
                d="M224 0c17.7 0 32 14.3 32 32V62.1l15-15c9.4-9.4 24.6-9.4 33.9 0s9.4 24.6 0 33.9l-49 49v70.3l61.4-35.8 17.7-66.1c3.4-12.8 16.6-20.4 29.4-17s20.4 16.6 17 29.4l-5.2 19.3 23.6-13.8c15.3-8.9 34.9-3.7 43.8 11.5s3.8 34.9-11.5 43.8l-25.3 14.8 21.7 5.8c12.8 3.4 20.4 16.6 17 29.4s-16.6 20.4-29.4 17l-67.7-18.1L287.5 256l60.9 35.5 67.7-18.1c12.8-3.4 26 4.2 29.4 17s-4.2 26-17 29.4l-21.7 5.8 25.3 14.8c15.3 8.9 20.4 28.5 11.5 43.8s-28.5 20.4-43.8 11.5l-23.6-13.8 5.2 19.3c3.4 12.8-4.2 26-17 29.4s-26-4.2-29.4-17l-17.7-66.1L256 311.7v70.3l49 49c9.4 9.4 9.4 24.6 0 33.9s-24.6 9.4-33.9 0l-15-15V480c0 17.7-14.3 32-32 32s-32-14.3-32-32V449.9l-15 15c-9.4 9.4-24.6 9.4-33.9 0s-9.4-24.6 0-33.9l49-49V311.7l-61.4 35.8-17.7 66.1c-3.4 12.8-16.6 20.4-29.4 17s-20.4-16.6-17-29.4l5.2-19.3L48.1 395.6c-15.3 8.9-34.9 3.7-43.8-11.5s-3.7-34.9 11.5-43.8l25.3-14.8-21.7-5.8c-12.8-3.4-20.4-16.6-17-29.4s16.6-20.4 29.4-17l67.7 18.1L160.5 256 99.6 220.5 31.9 238.6c-12.8 3.4-26-4.2-29.4-17s4.2-26 17-29.4l21.7-5.8L15.9 171.6C.6 162.7-4.5 143.1 4.4 127.9s28.5-20.4 43.8-11.5l23.6 13.8-5.2-19.3c-3.4-12.8 4.2-26 17-29.4s26 4.2 29.4 17l17.7 66.1L192 200.3V129.9L143 81c-9.4-9.4-9.4-24.6 0-33.9s24.6-9.4 33.9 0l15 15V32c0-17.7 14.3-32 32-32z"
              />
            </svg>
          </div>
          <div class="item">
            Hop
            <svg
              xmlns="http://www.w3.org/2000/svg"
              height="1em"
              viewBox="0 0 576 512"
              style="fill: rgb(255, 240, 33)"
            >
              <path
                d="M316.9 18C311.6 7 300.4 0 288.1 0s-23.4 7-28.8 18L195 150.3 51.4 171.5c-12 1.8-22 10.2-25.7 21.7s-.7 24.2 7.9 32.7L137.8 329 113.2 474.7c-2 12 3 24.2 12.9 31.3s23 8 33.8 2.3l128.3-68.5 128.3 68.5c10.8 5.7 23.9 4.9 33.8-2.3s14.9-19.3 12.9-31.3L438.5 329 542.7 225.9c8.6-8.5 11.7-21.2 7.9-32.7s-13.7-19.9-25.7-21.7L381.2 150.3 316.9 18z"
              />
            </svg>
          </div>
          <div class="item">
            Accoustic
            <img src="./assets/images/guitar.svg" alt="" />
          </div>
          <div class="item">
            Pinao Blues
            <img src="./assets/images/piano.svg" alt="" />
          </div>
          <div class="item">
            Jazz
            <img src="./assets/images/saxophone.svg" alt="" />
          </div>
        </div>
      </div>
      <div class="trending__songs">
        <p class="title__small">Trending</p>
        <div class="list__songs--trending">
          <ul
            class="list__trending"
            v-for="(song, index) in listSongs"
            :key="index"
          >
            <li :class="song.encodeId" class="song" @click="ChooseSong(song)">
              <span class="index_song">{{ ++index }}</span>
              <img :src="song.thumbnailM" alt="" />
              <span class="infor_song">
                {{ song.title }}
                <br />
                <small class="artistsNames">{{ song.artistsNames }}</small>
              </span>
            </li>
          </ul>
        </div>
      </div>
      <div class="song__information">
        <div class="image__song">
          <img class="image" :src="thumbnailM" alt="" />
          <div class="image__overlay">
            <img class="wave_gif" src="./assets/images/giphy.gif" alt="" />
          </div>
        </div>
        <div class="song__title">
          <h3>{{ songName }}</h3>
          <p class="artistsNames">{{ artistName }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SideBar from "./components/SideBar.vue";
import PlayBar from "./components/PlayBar.vue";
import axios from "axios";
export default {
  components: { SideBar, PlayBar },
  name: "App",
  data() {
    return {
      listSongs: [],
      currentSong: {},
      songUrl: "",
      songName: "Chua co bai",
      artistName: "Thanh Nhan",
      releaseDate: "",
      thumbnailM:
        "https://scontent.fsgn2-4.fna.fbcdn.net/v/t39.30808-1/333460614_508333481268765_5148619952475777596_n.jpg?stp=cp6_dst-jpg_p480x480&_nc_cat=109&ccb=1-7&_nc_sid=7206a8&_nc_ohc=YA1Eg9BygPAAX8wKUYR&_nc_ht=scontent.fsgn2-4.fna&oh=00_AfAyKBC63SG6yoHfMBINjEG31EWxwpdl8wXjUHJZdS7SOA&oe=6494D7A4",
      search_value: "",
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    async fetchData() {
      console.log("asasas");
      try {
        await axios
          .get("https://zing-mp3-api.vercel.app/api/chart/home")
          .then((res) => {
            this.listSongs = res.data.data.weekChart.vn.items;
            this.current_song = this.listSongs[0];
          });
      } catch (error) {
        console.log(error);
      }
    },
    async ChooseSong(song) {
      try {
        console.log(song.encodeId);
        await axios
          .get(`https://zing-mp3-api.vercel.app/api/song/${song.encodeId}`)
          .then((res) => {
            console.log(res.data.data);
            this.songUrl = Object.values(res.data.data)[0];
            this.thumbnailM = song.thumbnailM;
            this.songName = song.title;
            this.artistName = song.artistsNames;
            this.releaseDate = song.releaseDate;
          });
        document.querySelectorAll(".song").forEach((item) => {
          item.classList.remove("active");
        });
        document.querySelector("." + song.encodeId).classList.add("active");
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>
<style scoped src="../src/assets/css/styles.css"></style>
<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  width: 80%;
  height: 87vh;
  margin: auto;
  margin-top: 50px;
  display: flex;
  border-radius: 15px;
  box-shadow: 3px 3px 10px rgb(84, 82, 82);
}
.content {
  padding: 20px 30px;
  width: 54%;
  background-color: #e7e8f3;
  position: relative;
}
.search {
  position: absolute;
  top: 25px;
  left: 24px;
  width: 90%;
  font-size: 15px;
  padding: 10px 50px 10px 42px;
  border-radius: 24px;
  border: none;
  background-color: #ffff;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 0px 5px 0px,
    rgba(0, 0, 0, 0.1) 0px 0px 1px 0px;
}
.search:focus {
  outline-width: 0;
}
.bi-search {
  position: relative;
  z-index: 3;
  top: 16px;
  right: -10px;
  color: rgb(0 0 0);
}
.result__search {
  position: absolute;
  z-index: 1;
  width: 80%;
  height: 50%;
  background-color: #ffff;
  box-shadow: rgba(99, 99, 99, 0.2) 0px 2px 8px 0px;
  margin-top: 30px;
  border-radius: 5px;
  padding: 12px;
  overflow: scroll;
  left: 30px;
  color: #233329;
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
  left: 35px;
}
.img_song_search {
  width: 40px;
  margin-right: 12px;
  border-radius: 5px;
}
.result__search::-webkit-scrollbar {
  display: none;
}
.list_songs_search {
  padding-left: 0;
}
.list_songs_search li {
  list-style-type: none;
  padding: 6px;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
}
.list_songs_search li:hover {
  background: #85ffbd;
  background: -webkit-linear-gradient(to right, #85ffbd, #fffca5);
  background: linear-gradient(to right, #85ffbd, #fffca5);
  border-radius: 5px;
}
.infor_song_search {
  display: flex;
  flex-direction: column;
}
.infor_song_search {
  display: flex;
  flex-direction: column;
}
.artistsNames {
  font-size: 11px;
  font-weight: 400;
}
.fa-circle-xmark {
  width: 20px;
  position: absolute;
  right: 57px;
  top: 34px;
  z-index: 9;
  color: rgb(192, 192, 192);
  cursor: pointer;
}
.content__title {
  margin-top: 40px;
}
.title__small {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  font-size: 12px;
}
.title__small img {
  width: 15px;
  height: 15px;
  margin-left: 10px;
}
.title__big {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.title__big h3 {
  margin: 5px 0;
  font-size: 18px;
  font-weight: 550;
}
.title__big div {
  font-size: 14px;
  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: 4px;
  color: #434344;
  cursor: pointer;
}
.content__banner {
  width: 100%;
  height: 30%;
  background: linear-gradient(
      to right,
      white,
      rgba(255, 255, 255, 0.85),
      rgba(255, 255, 255, 0)
    ),
    url(./assets/images/banner.jpg);
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
  border-radius: 16px;
  padding: 30px;
  margin-top: 10px;
}
.content__banner h3 {
  font-weight: 550;
  margin: 10px 0px 20px;
}
.banner__sub {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
button {
  padding: 4px 5px;
  border-radius: 14px;
  width: 80px;
  box-sizing: border-box;
  border: 1px solid black;
  margin-right: 10px;
  background-color: black;
  color: #ffff;
  font-size: 12px;
  font-weight: 600;
}
button:hover {
  opacity: 0.6;
}

.follow {
  background-color: #ffff;
  color: black;
}
.banner__sub--title {
  color: #ffff;
}
.content__playlist {
  height: 32%;
  box-sizing: border-box;
  margin-top: 20px;
}
.content__playlist h3 {
  padding: 0px 10px 20px;
}
.list__songs {
  height: 75%;
  overflow: auto;
}
.list__songs::-webkit-scrollbar {
  display: none;
}
.playlist {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  width: 100%;
  padding: 6px 10px;
  border-radius: 5px;
}
.playlist__content {
  padding: 3px 10px;
  transition: all 0.2s ease;
  border-radius: 5px;
  margin: 0 2px;
  height: 30px;
}
.playlist__content li {
  list-style-type: none;
  padding: 6px;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  align-items: center;
}
.playlist__content:hover {
  border-radius: 5px;
  box-shadow: 9px 9px 6px -10px rgb(125, 125, 125);
  /* background: linear-gradient(to right, #a6aae4, #e7e8f3); */
  background-color: #ffff;
  border-radius: 5px;
}
.playlist li {
  padding-left: 10px;
  font-size: 10px;
  color: #434344;
}
.playlist .col-1 {
  flex-basis: 15%;
}
.playlist .col-2 {
  flex-basis: 50%;
}
.playlist .col-3 {
  flex-basis: 30%;
}
.top__Trending {
  padding: 40px 0 0 30px;
  background-color: #e7e8f3;
  width: 22%;
}
.shortcuts__items {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: flex-start;
  gap: 10px;
  margin-top: 20px;
  padding-right: 10px;
}
.item {
  padding: 5px 15px;
  border-radius: 20px;
  font-size: 13px;
  background-color: #ffff;
  width: fit-content;
}

.item img {
  width: 15px;
  height: 15px;
}

.list__songs--trending::-webkit-scrollbar {
  display: none;
}
.trending__songs {
  padding-top: 30px;
  height: 34%;
}
.list__songs--trending {
  margin-top: 10px;
  height: 100%;
  overflow: auto;
}
.song {
  display: flex;
  display: flex;
  align-items: center;
  font-size: 12px;
  font-weight: 550;
  padding: 7px 16px;
  border-radius: 12px 0 0 12px;
  background-color: rgba(255, 255, 255, 0.39);
  height: 45px;
  cursor: pointer;
}
.song:hover {
  background-color: #ffff;
}
.song img {
  width: 32px;
  margin: 0 10px;
}
.list__trending {
  padding: 5.6px 0;
  
}
.infor_song {
  display: flex;
  flex-direction: column;
}

.active {
  background: linear-gradient(to right,#9ba0df, #ffff );
  border-radius: 5px;
}
.song__information {
  padding: 10px 20px;
    border-radius: 20px;
    background-color: #ffff;
    height: 33%;
    width: 80%;
    margin: auto;
    position: relative;
    margin-top: 30px;
  box-sizing: border-box;
}
.image__song{
  position: relative;
}
.image{
  width: 93%;
  height: 70%;
  border-radius: 15px;
  margin-left: 5px;
  transition: all 0.2s ease 0s;

}
.song__title {
  text-align: center;
}
.song__title h3{
  font-size: 16px;
}
.wave_gif {
  width: 90px;
  opacity: 0.7;
}
.image__overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 100%;
  border-radius: 15px;
  background-color: rgba(0, 0, 0, 0.2);
  display: flex;
  justify-content: center;
  align-items: center;
  transform: scale(0);
  transition: all 0.2s ease 0s;
}
.song__information:hover .image__overlay {
  transform: scale(1);
}
.song__information:hover .image{
  /* margin:0 ; */
  transform: scale(1.05);
}
</style>
