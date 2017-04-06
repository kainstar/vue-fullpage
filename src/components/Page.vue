<template>
    <section class="page" v-if="option"
    :style="{background:option.background,color:option.color||'#fff'}" 
    :class="{'page-before': option.index < currentPage,'page-after': option.index > currentPage}">
        <div :class="{'page-center': option.isCenter}">
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
            option: null
        }
    }
}
</script>

<style>
.page {
    overflow: hidden;
    position: absolute;
    width: 100%;
    height: 100%;
    transition: all 0.5s ease 0s;
}
/* 水平、垂直居中 */
.page-center {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
@media screen and (max-width:768px) {
    .page-center {
        width: 100%;
    }
}
.page-before {
    transform: translate3d(0,-100%,0);
}
.page-after {
    transform: translate3d(0,100%,0);
}
</style>
