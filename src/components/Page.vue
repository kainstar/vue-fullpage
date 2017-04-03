<template>
    <section class="page" v-if="options !== null"
    :style="{backgroundColor:options.bgColor,color:options.color||'#fff'}" 
    :class="{'page-before': options.index < currentPage,'page-after': options.index > currentPage}">
        <div :class="{'page-content': options.isCenter}">
            <slot></slot>
        </div>
    </section>
    <section class="page" v-else>页面正在渲染中。。。</section>
</template>

<script>
export default {
    name: 'page',
    props: {
        currentPage: Number
    },
    data () {
        return {
            options: null
        }
    },
    computed: {
    }
}
</script>

<style>
.page {
    position: absolute;
    width: 100%;
    height: 100%;
    transition: all 0.5s ease 0s;
}
/* 水平、垂直居中 */
.page-content {
    overflow: hidden;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
.page-before {
    transform: translate3d(0,-100%,0);
}
.page-after {
    transform: translate3d(0,100%,0);
}
</style>
