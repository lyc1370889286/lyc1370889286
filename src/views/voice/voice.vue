<template>
  <body>
  <div class="header-wrapper">
    <header>
      <div class="container">
        <div1 class="logo-container">
          <!-- Website Logo -->
          <p id="bigname"><strong>LIFTER</strong></p>
          <span class="tag-line">Premium WordPress Theme</span>
        </div1>

        <!-- Start of Main Navigation -->
        <img id="image2" src="../../assets/logo.jpg" alt="" />
        <el-link href="#"><img id="image1" :src="imgUrl" alt="" /></el-link>
        <nav class="main-nav">
          <div class="menu-top-menu-container">
            <ul id="menu-top-menu" class="clearfix">
              <template>
                <el-menu :default-active="activeIndex"   class="el-menu-demo" mode="horizontal" @select="handleSelect">
                  <el-menu-item index="2" ><a href="/menu">网站主页</a></el-menu-item>
                  <el-menu-item index="3" ><a href="/allArticle">所有文章</a></el-menu-item>
                  <el-submenu index="4" mode="vertical" >
                    <template  slot="title" >我的</template>
                    <el-menu-item index="4-1"><a href="/myArticle">我的文章</a></el-menu-item>
                    <el-menu-item index="4-3" ><a href="/myComment">我的评论</a></el-menu-item>
                  </el-submenu>
                  <el-menu-item index="1" @click="toUserPage" class="import">个人主页</el-menu-item>
                </el-menu>
                <div class="line"></div>

              </template>



            </ul>
          </div>
        </nav>
        <!-- End of Main Navigation -->

      </div>
    </header>
  </div>

  <div id="page-wraper">
    <!-- Sidebar Menu -->
    <div class="responsive-nav">
      <i class="fa fa-bars" id="menu-toggle"></i>
      <div id="menu" class="menu">
        <i class="fa fa-times" id="menu-close"></i>
        <div class="container">
          <el-radio-group v-model="isCollapse" style="margin-bottom: 20px;">

          </el-radio-group>
          <template>

            <el-menu default-active="1-4-1" class="el-menu-vertical-demo" @open="handleOpen" @close="handleClose" :collapse="isCollapse"  background-color="#545c64"  text-color="#fff"  active-text-color="#ffd04b">


              <el-submenu index="1">
                <template #title>
                  <i class="el-icon-location"></i>

                </template>
                <el-menu-item-group>
                  <template #title></template>
                  <el-link @click="toUserPage"><el-menu-item index="1-1">可视主页</el-menu-item></el-link>

                </el-menu-item-group>
              </el-submenu>
              <el-submenu index="2">
                <template #title>
                  <i class="el-icon-s-order"></i>
                  <span>文章</span>
                </template>
                <el-menu-item-group>
                  <template #title></template>
                  <el-link href="/myArticle"><el-menu-item index="1-1">历史文章</el-menu-item></el-link>
                  <el-link href="/writeArticle"><el-menu-item index="1-2">新增文章</el-menu-item></el-link>
                </el-menu-item-group>
              </el-submenu>




              <el-submenu index="3" >
                <template #title>
                  <i class="el-icon-setting"></i>
                  <span>设置</span>
                </template>
                <el-menu-item-group>
                  <template #title></template>
                  <el-link href="/changePassword"><el-menu-item index="3-1">重置密码</el-menu-item></el-link>
                  <el-link href="/personalNotKeep"><el-menu-item index="3-2">编辑信息</el-menu-item></el-link>
                  <el-link href="/changeHead"><el-menu-item index="3-3">更改头像背景</el-menu-item></el-link>
                  <el-link href="/voice"><el-menu-item index="3-4">语音设置</el-menu-item></el-link>
                </el-menu-item-group>
              </el-submenu>
              <el-submenu index="4">
                <template #title>
                  <i class="el-icon-switch-button"></i>

                </template>
                <el-menu-item-group>
                  <template #title></template>
                  <el-link href="/login"><el-menu-item index="1-1">退出登录</el-menu-item></el-link>
                </el-menu-item-group>
              </el-submenu>
            </el-menu>
          </template>
        </div>
      </div>
    </div>
  </div>


  <form action="#">
    <h3>语音设置</h3>
    <label>发声人</label>
    <select class="form-select" aria-label="Default select example" v-model="person">
      <option selected value="1">男1</option>
      <option value="3">男2</option>
      <option value="0">女1</option>
      <option value="4">女2</option>
    </select>
    <div class="block">
      <span class="demonstration">语速</span>
      <el-slider
          v-model="speed"
          :step="1"
          :max="15"
          show-stops>
      </el-slider>
    </div>

    <div class="block">
      <span class="demonstration">音调</span>
      <el-slider
          v-model="pitch"
          :step="1"
          :max="15"
          show-stops>
      </el-slider>
    </div>

    <div class="block">
      <span class="demonstration">音量</span>
      <el-slider
          v-model="volume"
          :step="1"
          :max="15"
          show-stops>
      </el-slider>
    </div>

    <br />
    <el-button type="primary" id="keep" @click="toEditAudioInfo">保存<i class="el-icon-upload el-icon--right"></i></el-button>
  </form>
  </body>
</template>

<script>
import {editAudioInfo, getAudioInfo} from "@/api/api";

export default {

  async created() {
    const code = await getAudioInfo();
    if (code.result === 0) {
      alert("请登录");
      await this.$router.push('/login');
    } else {
      this.person = code.person;
      this.speed = code.speed;
      this.pitch = code.pitch;
      this.volume = code.volume
    }
  },

  data() {
    return {
      imgUrl: 'http://127.0.0.1:8000/mainPage/getImage/' + window.sessionStorage.getItem("username"),
      page: window.sessionStorage.getItem("username"),
      isCollapse: true,
      activeIndex: '1',
      activeIndex2: '1',
      person: '',
      speed: 0,
      pitch: 0,
      volume: 0
    }
  },
  name:
      "voice",
  methods:{
    toUserPage() {
      this.$router.push("/personalPage/" + this.page);
    },
    handleSelect(key, keyPath) {
      console.log(key, keyPath);
    },
    handleOpen(key, keyPath) {
      console.log(key, keyPath);
    },
    handleClose(key, keyPath) {
      console.log(key, keyPath);
    },
    formatTooltip(val) {
      return val / 100;
    },

    async toEditAudioInfo() {
      const audioInfo = {
        speed: this.speed,
        pitch: this.pitch,
        volume: this.volume,
        person: this.person
      };
      const code = await editAudioInfo(audioInfo);
      if (code.result === 0) {
        alert("请登录");
        await this.$router.push('/login');
      } else if (code.result === "yes") {
        alert("修改成功！")
        location.reload();
      } else {
        alert("错误！")
      }
    },

  }
}
</script>

<style scoped>
.import{
  color:white;
}
.block{
  margin:auto;
  width:50%;
}

form {
  margin-right:100px !important;
  margin-top: -100px !important;
  padding: 0;
  width:70%;
  float:right;
  border: 1px solid #f2f2f2;
  background-color: #FAFAFA;
}
label {
  display:block;
  font-weight:bold;
  margin:5px 0;
  color:black;
}
.form-select{
  width:20%;
}
input,.form-select {
  padding: 2px;
  border:1px solid #eee;
  font: normal 1em Verdana, sans-serif;
  color:#777;
}
#keep {
  margin:0px;
  font: bold 1em Arial, Sans-serif;
  border: 1px solid #CCC;
  background: #FFF;
  padding: 2px 3px;
  color: #4284B0;
}
body{

  height:800px;
}
#image1{
  width: 100px;
  height: 100px;
  border-radius: 50%;
  float:left;
  margin-left:40px;
  margin-top:-160px !important;
}
#image2{
  width: 100px;
  height: 100px;
  border-radius: 50%;
  float:left;
  margin-left:70px;
  margin-top:-170px !important;
}
form {
  margin:10px; padding: 0;
  border: 1px solid #f2f2f2;
  background-color: #FAFAFA;
}
label {
  display:block;
  font-weight:bold;
  margin:5px 0;
  color:black;
}
input,.form-select {
  padding: 2px;
  border:1px solid #eee;
  font: normal 1em Verdana, sans-serif;
  color:#777;
}
.text {
  width:190px;
  padding:100px;
  margin-left:auto;
  margin-right: auto;
  font: normal 1em Verdana, sans-serif;
  border:1px solid #eee;
  height:20px;
  display:block;
  color:black;
}
#keep {
  margin: 0;
  font: bold 1em Arial, Sans-serif;
  border: 1px solid #CCC;
  background: #FFF;
  padding: 7px 15px;
  color: #00CACA;
}


#page-wraper {
  background: url("#") no-repeat center center fixed;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
  width: 100%;
}


/* Basic */


input{
  outline: none;
}

a,
a:hover {
  text-decoration: none;
}

p {
  font-size: 16px;
  line-height: 24px;
  color: #9D9D9D;
}

h4 {
  font-size: 26px;
  font-weight: 700;
  color: #fff;
  letter-spacing: 0.25px;
}

img {
  width: 100%;
  overflow: hidden;
}

ul {
  padding: 0;
  margin: 0;
  list-style: none;
}

ul li {
  display: inline-block;
}

html {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}

*,
*:before,
*:after {
  -webkit-box-sizing: inherit;
  -moz-box-sizing: inherit;
  box-sizing: inherit;
}

.container {
  margin: 0 auto;
}

/* Sidebar Menu */

.menu .image {
  margin-top: 60px;
}

.menu .image img {
  width: 140px;
  height: 140px;
  border-radius: 50%;
  text-align: center;
}

.menu .author-content {
  margin: 40px 0px 60px 0px;
}

.menu .author-content h4 {
  margin-bottom: 0px;
}

.menu .author-content span {
  font-size: 13px;
  font-style: italic;
  color: #fff;
}

.menu {
  margin-top: 60px;
}

.menu  {
  position: relative;
  margin-top: 60px;
}
.menu:before,
.menu:after {
  content: "";
  display: table;
}

.menu:after {
  clear: both;
}

.menu a {
  text-decoration: none;
  color: inherit;
}

.menu {
  text-align: center;
}

.header-wrapper {
  background-color: #3A5F7B;
  width: 100%;
  height: auto;
  margin-top:-61px;
}
.header-wrapper .container {
  position: relative;
  min-height: 60px;
  height: auto !important;
  top:5px;
}
#bigname{
  color:white;
  font-size:xxx-large;
  margin-left: -500px !important;
}
.logo-container {
  padding: 19px 0;
}
.logo-container img {
  margin-right: 10px;
}
.logo-container {
  width: 450px;
}
span.tag-line {
  color: #818a90;
  font-size: 12px;
  position: relative;
  top:-5px;
  margin-left:-1280px !important;
}
.main-nav {
  position: absolute;
  top: 5px;
  right: 0;
}
.main-nav div > ul {
  width: auto;
  margin: 0;
}
.main-nav div > ul > li {
  position: relative;
  float: left;
  list-style: none;
  padding: 16px 14px 18px;
}
.main-nav div > ul > li a {
  font-family: "HelveticaNeue", "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-size: 13px;
  color: #c1cad1;
}
.main-nav div > ul > li a:hover {
  color: #fff;
}
.main-nav div > ul > li:hover > a {
  color: #fff;
}
.main-nav div > ul > li:last-child {
  padding-right: 0;
}
.main-nav div > ul li.current-menu-item a {
  color: #fff;
}
.main-nav .sub-menu {
  position: absolute;
  top: 56px;
  left: 0px;
  list-style: none;
  margin: 0;
  display: none;
}
.main-nav .sub-menu li {
  position: relative;
  background-color: #3b4348;
  padding: 6px 10px;
  width: 150px;
  cursor: pointer;
}
.main-nav .sub-menu li:hover {
  background-color: #292e32;
}
.main-nav .sub-menu li:hover > a {
  color: #fff;
}
.main-nav .sub-menu .sub-menu {
  top: 0px;
  right: 170px;
  left: auto;
}
.main-nav .responsive-nav {
  display: none;
}

/* Header - Search ---------------------------------------------------------------------------------------------------*/
.search-area-wrapper {
  background: #353b65 url("#") center top no-repeat;
  background-size: 100%;
  min-height: 279px;
  height: 279px;
}
.search-area-wrapper .search-area {
  padding: 50px 0;
}

h3.search-header {
  font-family: "HelveticaNeue", "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-weight: bold;
  font-size: 36px;
  color: #fff;
  text-align: center;
}

p.search-tag-line {
  font-family: "Droid Serif", serif;
  font-style: italic;
  font-size: 18px;
  line-height: 28px;
  color: #fff;
  text-align: center;
}

form.search-form {
  margin: 40px 0 0;
  text-align: center;
}
form.search-form , form.search-form input.search-term, form.search-form input.search-btn {
  font-family: "HelveticaNeue", "Helvetica Neue", Helvetica, Arial, sans-serif;
  box-shadow: none;
  border-radius: 0;
  display: inline-block;
  border: none;
  vertical-align: bottom;
}
form.search-form input.search-term {
  font-size: 14px;
  height: 24px;
  line-height: 24px;
  padding: 12px;
  width: 72%;
  color: #858c91;
  margin-right: 10px;
  margin-bottom: 0;
}
form.search-form input.search-term.loading {
  background-image: url("#");
  background-repeat: no-repeat;
  background-position: 98% 50%;
}
form.search-form input.search-btn {
  background-color: #3f4b55;
  font-size: 14px;
  line-height: 19px;
  font-weight: 600;
  padding: 14px 30px 15px;
  color: #fff;
}
form.search-form input.search-btn:hover {
  background-color: #4b5760;
}

#search-error-container label {
  color: #fff;
  padding: 5px;
  font-size: 14px;
}

.lt-ie8 form.search-form input.search-btn {
  padding-top: 12px;
  padding-bottom: 13px;
}

/* Page - Container --------------------------------------------------------------------------------------------------*/
.page-container {
  width: 100%;
}
.page-container > .container {
  padding: 40px 35px;
  background-color: #fff;
}


.pages-nav a {
  margin-right: 10px;
}


div1 {
  display: block;
  margin: 5px auto 20px auto;
}

li.comment > article {
  background: #fff;
  float: left;
  margin: 0px 10px 10px 0;
  position: relative;
  z-index: 1;
  -webkit-border-radius: 60px;
  -moz-border-radius: 60px;
  border-radius: 60px;
}
.page-content {
  margin-bottom: 20px;
}

.row.separator {
  margin-bottom: 10px;
}
.articles-list {
  margin-bottom: 20px;
}
.articles-list > h3 {
  font-weight: 600;
  margin-bottom: 20px;
  font-size: 20px;
}
.articles-list > h3 span {
  color: #6f7579;
  font-size: 16px;
}
.articles-list > h3 a {
  color: #3b4348;
}
.articles-list > h3 a:hover {
  color: #395996;
}

ul.articles {
  list-style: none;
  margin: 0;
}
ul.articles .article-entry {
  position: relative;
  border-bottom: 1px solid #f2f2f2;
  padding: 0 0 0 24px;
  background: url("#") no-repeat 0 3px;
}
ul.articles .article-entry.standard {
  background: url("#") no-repeat 0 3px;
}
ul.articles .article-entry.video {
  background: url("#") no-repeat 0 3px;
}
ul.articles .article-entry.image {
  background: url("#") no-repeat 0 3px;
}
ul.articles .article-entry {
  background: url("#") no-repeat 0 3px;
}
ul.articles li.article-entry:last-child {
  border-bottom: 1px solid #f2f2f2;
}

.article-entry > h4 {
  font-size: 13px;
  margin-bottom: 5px;
  font-weight: 600;
}
.article-entry > h4 a {
  color: #3b4348;
}
.article-entry > h4 a:hover {
  color: #395996;
}
.article-entry .article-meta {
  display: block;
  margin-bottom: 10px;
}
.article-entry .like-count {
  position: absolute;
  bottom: 0px;
  right: 0px;
  line-height: 18px;
  font-weight: 600;
  padding: 3px 5px 3px 20px;
  border: 1px solid #f2f2f2;
  border-bottom: none;
  background: url("../../assets/liked.png") no-repeat 6px 8px;
}
.article-entry:hover .like-count {
  background: url("#") no-repeat 6px -22px;
  color: #395996;
}

#footer .article-entry .like-count {
  border-color: #535b61;
}
#footer .article-entry:hover .like-count {
  color: #fff;
}
#footer ul.articles li.article-entry:last-child {
  border-bottom: 1px solid #535b61;
}

.main-listing article {
  margin-bottom: 0;
}
.page-container > .container {
  padding: 40px 20px;
}
@media (max-width: 767px) {
  /* Navigation */
  .main-nav > div {
    display: none;
  }

  .main-nav {
    display: block;
    width: 85%;
    margin: 10px auto;
    padding: 10px;
    border-radius: 0;
    border: none;
  }

  /* Search Area */
  .search-area-wrapper {
    background-size: auto;
    min-height: 240px;
    height: auto !important;
  }

  .search-area-wrapper .search-area {
    padding: 20px 0;
  }

  .search-area-wrapper h3.search-header {
    font-size: 30px;
    padding: 0 15px;
  }

  .search-area-wrapper p.search-tag-line {
    padding: 0 15px;
    font-size: 14px;
    line-height: 22px;
  }

  .search-area-wrapper form.search-form input.search-term {
    width: 60%;
  }

  @media (max-width: 480px) {
    /* Search Area */
    .search-area-wrapper form.search-form input.search-term {
      width: 80%;
      display: block;
      margin: 0 auto 10px;
    }

    .search-area-wrapper form.search-form input.search-btn {
      display: block;
      margin: 0 auto;
    }

    ul.articles .article-entry {
      padding-bottom: 20px;
    }
  }
  /* Menu Fix for resizing to Bigger Screen sizes */
  @media (min-width: 980px) {
    .main-nav div > ul > li {
      float: none;
      display: inline-block;
    }
  }
}
</style>