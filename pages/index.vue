<template>
    <div class="home-page">
        <cl-line title="CODING"></cl-line>
        <ul class="posts-list">
            <li v-for="post in codings" :key="post.id">
                <nuxt-link class="title" :to="`/post/${post.id}/${post.slug}`">{{post.title}}</nuxt-link>
                <span class="createTime">{{post.create_time | timeFormat}}</span>
            </li>
        </ul>

        <cl-line title="SHARING"></cl-line>
        <ul class="posts-list">
            <li v-for="post in sharings" :key="post.id">
                <nuxt-link class="title" :to="`/post/${post.id}/${post.slug}`">{{post.title}}</nuxt-link>
                <span class="createTime">{{post.create_time | timeFormat}}</span>
            </li>
        </ul>

        <!-- 联系我 -->
        <cl-line title="LINKS"></cl-line>
        <ul class="links">
            <li>
                <span style="color: #333333;">Github：</span><a href="https://github.com/Lee-Cloud" target="_blank">Lee-cloud</a>
            </li>
            <li>
                <span style="color: #E80025;">Weibo：</span><a href="https://weibo.com/u/2808915224" target="_blank">@不想睡觉又困的不行的cloud</a>
            </li>
            <li>
                <span style="color: #0C9DF2;">twitter：</span><a href="https://twitter.com/Lewiiiissss" target="_blank">@Lewiiiissss</a>
            </li>
            <li>
                <span>Mail：</span><a href="mailto:lewis1990@hotmail.com">lewis1990@hotmail.com</a>
            </li>
        </ul>
    </div>
</template>

<script>
import Vue from "vue";
import moment from "moment";
import clLine from "~/components/line.vue";
import pinyin from "pinyin";
export default {
    layout: "blog",
    async asyncData (ctx) {
        let codings = [];
        let sharings = [];
        try {
            const res = await Vue.http.get(Vue.api.HOME_PAGE);
            if (res.success) {
                codings = res.data.codings;
                sharings = res.data.sharings;
            }
        } catch (e) {
            ctx.error({ statusCode: 500, message: "出错啦" });
        }
        codings.forEach(item => {
            item.slug = pinyin(item.title, { style: pinyin.STYLE_NORMAL }).join("-");
        });
        sharings.forEach(item => {
            item.slug = pinyin(item.title, { style: pinyin.STYLE_NORMAL }).join("-");
        });
        return {
            codings: codings,
            sharings: sharings
        };
    },
    data () {
        return {};
    },
    head () {
        return {
            meta: [
                {
                    name: "description",
                    content: "Lewis1990@Amoys是本人全栈开发的个人博客，采用KOA2 + NUXT.js的服务端渲染方案"
                }
            ]
        };
    },
    mounted () {},
    filters: {
        timeFormat: function (time) {
            if (!time) return "";
            return `（${moment(time).format("MMMM YYYY")}）`;
        }
    },
    methods: {},
    components: {
        clLine
    }
};
</script>

<style lang="less" scoped>
    .home-page {
        padding: 0 10px;
        ul.posts-list {
            margin-bottom: 30px;
            padding: 0 10px;
            list-style: none;
            > li {
                margin-bottom: 10px;
                overflow: hidden;
                text-overflow: ellipsis;
                white-space: nowrap;
                .title {
                    color: #555;
                    cursor: pointer;
                    text-decoration: underline;
                }
                .createTime {
                    font-size: 12px;
                }
            }
        }

        ul.links {
            list-style: none;
            li {
                > span {
                    font-weight: bold;
                }
                > a {
                    color: #555;
                    text-decoration: underline;
                }
            }
        }
    }
</style>
