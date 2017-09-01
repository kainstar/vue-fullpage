<template>
  <div id="app" class="app">
    <page :currentPage="currentPage">
      <h1 class="text-center">项目介绍</h1>
      <section class="animate" ref="section1">
        <p class="demo-intro">Vue-FullPage是一个使用Vue制作的全屏页面模板，是为了学习Vue而制作的一个Vue项目</p>
        <p class="demo-intro">使用到的相关npm包有：
          <a href="https://github.com/vuejs/vue-cli" target="_blank">vue-cli</a>、
          <a href="https://github.com/vuejs-templates/webpack-simple" target="_blank">webpack-simple模板</a>
        </p>
        <p class="demo-intro">支持：导航条滚动，滚轮滚动，触摸滚动</p>
      </section>
    </page>
    <page :currentPage="currentPage">
      <h1 class="text-center">配置说明</h1>
      <section class="animate move-left">
        <p>在App.vue中修改data函数返回的内容，即为修改相应的配置。现在可以配置三个属性：currentPage、arrowAnimation和options</p>
        <dl>
          <dt>currentPage:</dt>
          <dd>表示当前显示的页面，通过设置currentPage可以改变初始显示的界面</dd>
        </dl>
        <dl>
          <dt>controllerOption:</dt>
          <dd>该属性表示控制器的配置属性，可以自由扩展
            <ul>
              <li>arrowsType表示页面控制器的上下箭头显示类型：no（不显示箭头）、normal（显示箭头）、animate（显示有动画效果的箭头）</li>
            </ul>
          </dd>
        </dl>
        <dl>
          <dt>options:</dt>
          <dd>该属性是一个数组，数组的每一项都是一个对象，通过设置对象内的值，可以改变对应的page组件的样式，可以自由扩展
            <ul>
              <li>
                <span class="text-bold">background:</span> 表示相应page的背景样式</li>
              <li>
                <span class="text-bold">color:</span> 表示相应page的文字颜色（可以手动设置css样式覆盖）</li>
              <li>
                <span class="text-bold">isCenter:</span> 表示相应page的内容是否居中（水平和垂直都包括）</li>
              <li>下一页介绍options内的方法属性</li>
            </ul>
          </dd>
        </dl>
      </section>
    </page>
    <page :currentPage="currentPage">
      <h1 class="text-center">方法说明</h1>
      <section class="animate move-left">
        <p>在每个options的对象中，可以设置两种方法：beforeLeave 和 afterEnter</p>
        <ul>
          <li>beforeLeave 方法表示在离开当前页面前所做的操作</li>
          <li>afterEnter 方法表示在进入当前页面后所做的操作</li>
        </ul>
        <p>这两个方法都有一个默认参数，该参数为当前Page的vue组件实例，方法的this为App.vue的组件实例</p>
      </section>
    </page>
    <page :currentPage="currentPage">
      <h1 class="text-center">作者信息</h1>
      <section class="animate move-left">
        <img class="avatar" src="./assets/avatar.jpg" alt="头像">
        <div class="author-info">
          <p>昵称：KainStar</p>
          <p>学校：南京理工大学</p>
          <p>项目地址：
            <a href="https://github.com/hzxszsk/vue-fullpage" target="_blank">https://github.com/hzxszsk/vue-fullpage</a>
          </p>
          <p>我的Github：
            <a href="https://github.com/hzxszsk" target="_blank">https://github.com/hzxszsk</a>
          </p>
        </div>
      </section>
    </page>
    <page-controller :pageNum="pageNum" :currentPage="currentPage" @changePage="changePage" :option="controllerOption"></page-controller>
  </div>
</template>

<script>
import Page from './components/Page.vue';
import PageController from './components/PageController.vue';

// 页面进出动画
function afterEnterAnimate($child) {
  $child.$el.querySelector('.animate').classList.remove('move-left', 'move-right');
}
function beforeLeaveAnimate($child) {
  let moveType = Math.random() > 0.5 ? 'move-left' : 'move-right';
  $child.$el.querySelector('.animate').classList.add(moveType);
}

export default {
  name: 'app',
  data() {
    return {
      currentPage: 1,
      options: [{
        // the color of background
        background: 'rgba(229, 199, 46, 1)',
        // the color of text
        color: '#fff',
        // is content center
        isCenter: true,
        // the function before page show
        afterEnter: afterEnterAnimate,
        // the function after page show
        beforeLeave: beforeLeaveAnimate
      }, {
        background: 'rgba(79, 204, 76, 1)',
        color: '#fff',
        isCenter: true,
        afterEnter: afterEnterAnimate,
        beforeLeave: beforeLeaveAnimate
      }, {
        background: 'rgba(233, 84, 84, 1)',
        color: '#fff',
        isCenter: true,
        afterEnter: afterEnterAnimate,
        beforeLeave: beforeLeaveAnimate
      }, {
        background: 'rgba(46, 153, 229, 1)',
        color: '#fff',
        isCenter: true,
        afterEnter: afterEnterAnimate,
        beforeLeave: beforeLeaveAnimate
      }],
      controllerOption: {
        arrowsType: false,
        navbar: true,
        highlight: true,
        loop: false
      }
    }
  },
  computed: {
    // 总page数
    pageNum() {
      return this.options.length;
    }
  },
  methods: {
    changePage(index) {
      // beforeLeave Hook
      let beforeIndex = this.currentPage - 1;
      let leaveFunction = this.options[beforeIndex].beforeLeave;
      typeof leaveFunction === 'function' && leaveFunction.call(this, this.$children[beforeIndex]);
      // change page
      this.currentPage = index;
      // afterEnter Hook
      let nextIndex = index - 1;
      let enterFunction = this.options[nextIndex].afterEnter;
      this.$nextTick(function() {
        typeof enterFunction === 'function' && enterFunction.call(this, this.$children[nextIndex]);
      })
    }
  },
  components: {
    Page, PageController
  },
  mounted() {
    this.$children.forEach((child, index) => {
      // 动态设置各个page内的options
      if (child.option === null) {
        let childOption = this.options[index];
        this.$set(childOption, 'index', index + 1);
        child.option = childOption;
      }
    });
  }
}
</script>

<style>
html,
body {
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

.animate {
  transition: all 1s ease-out 0s;
}

.move-left {
  transform: translateX(-1000%);
}

.move-right {
  transform: translateX(1000%);
}

@media screen and (max-width:768px) {
  html,
  body {
    font-size: 12px;
  }
}

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

.author-info {
  text-align: center;
}
</style>
