<template>
  <div id="app" class="app">
    <page :currentPage="currentPage">
      <h1 class="text-center">Vue-FullPage</h1>
      <section>
        <p class="demo-intro">Vue-FullPage是一个使用Vue制作的全屏页面模板，是为了学习Vue（顺便做简历）而制作的一个Vue项目</p>
        <p class="demo-intro">使用到的相关npm包有：
          <a href="https://github.com/vuejs/vue-cli" target="_blank">vue-cli</a>、
          <a href="https://github.com/vuejs-templates/webpack-simple" target="_blank">webpack-simple模板</a>
        </p>
        <p class="demo-intro">支持：导航条滚动，滚轮滚动（触屏滚动正在开发中）</p>
      </section>
    </page>
    <page :currentPage="currentPage">
      <h1 class="text-center">配置说明</h1>
      <section>
        <p>在App.vue中修改data函数返回的内容，即为修改相应的配置。现在可以配置两个属性：currentPage和options</p>
        <dl>
          <dt>currentPage:</dt>
          <dd>表示当前显示的页面，通过设置currentPage可以改变初始显示的界面</dd>
        </dl>
        <dl>
          <dt>options:</dt>
          <dd>该属性是一个数组，数组的每一项都是一个对象，通过设置对象内的值，可以改变对应的page组件的样式
            <ul>
              <li><span class="text-bold">bgColor:</span> 表示相应page的背景颜色</li>
              <li><span class="text-bold">color:</span> 表示相应page的文字颜色（可以手动设置css样式覆盖）</li>
              <li><span class="text-bold">isCenter:</span> 表示相应page的内容是否居中（水平和垂直都包括）</li>
            </ul>
          </dd>
        </dl>
      </section>
    </page>
    <page :currentPage="currentPage">
      <h1 class="text-center">作者信息</h1>
      <section>
        <img class="avatar" src="./assets/avatar.jpg" alt="头像">
        <p>昵称：KainStar</p>
        <p>学校：南京理工大学</p>
        <p>我的Github：<a href="https://github.com/hzxszsk" target="_blank">https://github.com/hzxszsk</a></p>
      </section>
    </page>
    <page-controller :pageNum="pageNum" :currentPage="currentPage" @changePage="changePage"></page-controller>
  </div>
</template>

<script>
import Page from './components/Page.vue';
import PageController from './components/PageController.vue';

export default {
  name: 'app',
  data () {
    return {
      currentPage: 1,
      options: [{
        bgColor: 'rgba(229, 199, 46, 1)',
        color: '#fff',
        isCenter: true
      },{
        bgColor: 'rgba(94, 233, 90, 1)',
        color: '#fff',
        isCenter: true
      },{
        bgColor: 'rgba(46, 153, 229, 1)',
        color: '#fff',
        isCenter: true
      }]
    }
  },
  computed: {
    // 总page数
    pageNum () {
      return this.options.length;
    }
  },
  methods: {
    changePage (index) {
      this.currentPage = index;
    }
  },
  components: {
    Page, PageController
  },
  mounted () {
    let _this = this;
    this.$children.forEach((child, index) => {
      // 动态设置各个page内的options
      if (child.options === null) {
        let childOptions = this.options[index];
        childOptions.index = index + 1;
        child.options = childOptions;
      }
    });
  }
}
</script>

<style>
html,body {
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
  overflow: hidden;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #fff;
}
.app {
  height: 100%;
  width: 100%;
}

/* 下面的是与fullPage无关的样式 */
a {
  text-decoration: none;
  color: inherit;
}
a:hover {
  text-decoration: underline;
}
.person-img {
  width: 223px;
  height: 185px;
  float: left;
  background-color: #fff;
  box-shadow: 3px 3px 10px #999;
}
.person-img img {
  height: 100%;
  width: 100%;
}
.person-basic-info {
  overflow: hidden;
  float: right;
  margin-left: 40px;
}
.info-line {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: flex-start;
}
.text-center {
  text-align: center;
}
.text-bold {
  font-weight: bold;
}
.demo-intro {
  text-indent: 2em;
}
dt {
  font-weight: bold;
  font-size: 16px;
}
ul {
  padding-left: 1em;
}
.avatar {
  margin: 10px auto;
  display: block;
  box-shadow: 1px 1px 5px #666;
}
</style>
