<template>
    <Head :title="title" />

    <div dusk="article-component" class="article-container manual-scope-XKpYE">
        <div id="wrapper">
            <div class="header">
                <Link :href="'tag/' + category" class="category">{{ category }}</Link>
                <h1>{{ title }}</h1>
                <div id="info" class="info">
                    <div v-if="author" class="author">
                        <!-- <div class="img force-aspect-ratio">
                            <img class="image" :src="authorimg" onerror="this.src='/img/authors/default-authorimg.svg';this.classList.add('svg');" alt="Author" />
                        </div> -->
                        <span class="author">
                            {{ author }}
                        </span>
                    </div>
                    <div v-if="author" class="seperator">-</div>
                    <div class="readtime">
                        {{ readTime }}
                    </div>
                    <!-- <div class="seperator" v-if="publishedAt">-</div>
                    <div v-if="publishedAt" class="date">
                        <span 
                            ref="publishedAt"
                            class="timeago" 
                            :datetime="publishedAtUnix"
                        ></span>
                    </div>  -->
                    
                    <!--
                    <div class="share">
                         <a href="">
                            <i class='material-icons'>facebook</i>
                        </a>
                        
                        <a onclick="navigator.clipboard.writeText(location.href).then(function() {
                                                    console.log('Async: Copying to clipboard was successful!');
                                                }, function(err) {
                                                    console.error('Async: Could not copy text: ', err);
                                                });">
                            <i class='material-icons'>share</i>
                        </a> -->
                        <!-- <a id="twitter"
                            href="https://twitter.com/intent/tweet?text=" 
                            target="_blank"
                            rel="noopener noreferrer">
                            <img :src="require('@img/twitter-icon.svg')">
                        </a>
                    </div> -->
                </div>
            </div>

            <div ref="articlecontent" id="articlecontent"></div>

            <div class="clear"></div>
            <read-more></read-more>
        </div>
    </div>

</template>

<script>
    import { createApp, defineComponent, onMounted, ref, compile, h } from 'vue'

    import MainAppLayout from '@/Layouts/MainAppLayout.vue'; 

    import { Head, Link } from '@inertiajs/inertia-vue3';

    // import { render, register } from 'timeago.js';
    // import de from 'timeago.js/lib/lang/de.js'
    // import moment from 'moment'

    import ReadMore from '@components/ReadMore'

    import Youtube from '@components/Youtube'
    import Image from '@components/Image'
    import VueForm from '@components/VueForm'
    import Dropdown from '@components/Dropdown'

    export default defineComponent({
        components: {
            Head,
            Link,
            ReadMore,
            VueForm,
            Dropdown
        },


        props: {
            canLogin: Boolean,
            canRegister: Boolean,
            content: String,
            title: String,
            author: String,
            category: String,
            publishedAt: String,
            readTime: String
        },

        layout: MainAppLayout,

        // beforeMount() {
        //     this.publishedAtUnix = moment(this.publishedAt, "DD.MM.YYYY HH:mm").unix() * 1000
        // },

        // mounted() {
        //     register('de', de);
        //     const publishedAt = this.$refs.publishedAt;
        //     // const nodes = this.$el.parentNode.querySelectorAll(".timeago");
        //     if (publishedAt != undefined) render([publishedAt], 'de');
        // },

        // data() {
        //     return {
        //         publishedAtUnix: null
        //     }
        // },

        setup(props) {
            

            const ArticleComponent = defineComponent({
                setup() {
                    const rawHTML = props.content
                    const renderer = compile(rawHTML)
                    const vnode = h(renderer)
                    return () => vnode
                }
            })
        
            const articlecontent = ref(null)

            onMounted(() => {
                createApp(ArticleComponent)
                    .component("youtube", Youtube)
                    .component("v-image", Image)
                    .component("inertia-link", Link)
                    .component("vue-form", VueForm)
                    .component("dropdown", Dropdown)
                    .mount(articlecontent.value)
            })

            return { articlecontent }
        }
    })
</script>



<style lang="scss">
.manual-scope-XKpYE {
    div.header {
        margin-bottom: 35px;
        div.info {
            display: flex;
            font-size: 18px;
            div {
                height: 40px;
                line-height: 40px;
                position: relative;
            }
            div.author {
                display: flex;
                div.img {
                    height: 100%;
                    width: 40px;
                    overflow: hidden;
                    border-radius: 20px;
                    margin-right: 10px;
                }
                @at-root {
                    @media (prefers-color-scheme: dark) {
                        #app.detect-colorscheme {
                            div.header div.info {
                                div.author div.img .svg, div.share a img {
                                    filter: invert(100%);
                                }
                            }
                        }
                    }
                    #app.dark {
                        div.header div.info {
                            div.author div.img .svg, div.share a img {
                                filter: invert(100%);
                            }
                        }
                    }
                }
                .force-aspect-ratio { 
                    // CLARIFICATION:
                    // This workaround replicates the behavior of the experimental feature 'aspect-ratio'
                    // SOURCE:
                    // https://pqina.nl/blog/presenting-images-in-an-aspect-ratio-with-css/
                    //
                    position: relative;
                    // width: calc(100% + 50px);
                    // transform: translate(-25px, -25px);
                    // margin-bottom: -15px;

                    &::after {
                        display: block;
                            content: '';
                        padding-bottom: 47%; // aspect ratio
                    }

                    .image {
                        position: absolute;
                        left: 0;
                        top: 0;
                        width: 100%;
                        height: 100%;
                        object-fit: cover;
                    }
                }
                span {
                    color: var(--clr-wgg-orange);
                    font-weight: bold;
                }
            }
            div.readtime, div.date, div.seperator {
                opacity: .6;
            }
            div.seperator {
                // background: var(--clr-font);
                // width: 2px;
                margin: 0 10px;
            }
            div.share {
                display: flex;
                margin-left: auto;
                a {
                    margin: 2.5px;
                    padding: 2.5px;
                    border-radius: 15px;
                    height: 30px;
                    width: 30px;
                    text-decoration: none;
                    transition-duration: .2s;
                    i {
                        display: block;
                        text-align: center;
                        font-size: 25px;
                        width: 100%;
                        height: 100%;
                    }
                    // background: var(--clr-font);
                    &:hover {
                        background: var(--clr-wgg-orange);
                        box-shadow: 0 0 10px rgba(0,0,0,0.1);
                    }
                }
            }
        }
    }

    // .content {
    //   max-width: var(--sz-content-width);
    //   width: 100%;
    //   padding: 15px;
    //   background: var(--clr-bg-primary);
    //   box-shadow: 0 0 15px rgba(0,0,0,.1);
    //   color: var(--clr-font);
    // }
    // #wrapper {
    //   position: relative;
    //   width: 100%;
    //   bottom: 0;
    //   margin-top: 25px;
    //   flex: 1 1 auto;
    //   display: flex;
    //   flex-direction: column;
    //   h2 {
    //     // border-bottom: 1px solid;
    //     // padding-bottom: 1em;
    //     // box-shadow: 0px 5px 6px -3px #00000062;
    //     font-weight: bold;
    //     box-shadow: 0 0 15px rgba(0,0,0,.1);
    //     max-width: var(--sz-content-width);
    //     background: var(--clr-bg-primary);
    //     border-radius: 15px;
    //     padding: 15px;
    //     margin-top: auto;
    //     position: relative;
    //     z-index: 10;
    //     font-size: 25px;
    //     line-height: 25px;
    //     font-family: var(--fnt-title);
    //     font-weight: 1200;
    //     color: var(--clr-font);
    //     width: 100%;
    //     margin: 0 auto;
    //     margin-bottom: 0;
    //   }
    //   .end {
    //     box-shadow: 0 0 15px rgba(0,0,0,.1);
    //     max-width: var(--sz-content-width);
    //     background: var(--clr-bg-primary);
    //     border-radius: 15px;
    //     padding: .5em 1em;
    //     position: relative;
    //     width: 100%;
    //     margin: 0 auto;
    //     // bottom: 0;
    //     // left: 0;
    //     font-size: .8em;
    //     z-index: 0;
    //     font-family: var(--fnt-title);
    //     color: var(--clr-font);
    //     display: flex;
    //     flex-direction: row;
    //     font-weight: 1200;
    //     div.author {
    //       padding-left: 10px;
    //       margin-left: auto;
    //     }
    //   }
        #wrapper {
            // min-height: calc(100vh - 2 * var(--sz-header-height) - 45px - var(--sz-footer-height) - 70px);
            max-width: var(--sz-content-width);
            word-wrap: break-word;
            flex: 1 1 auto;
            margin: 0 auto;
            width: 100%;
            padding: 35px;
            padding-top: 38px;
            background: var(--clr-bg-primary);
            box-shadow: 0 0 15px rgba(0,0,0,.05);
            color: var(--clr-font);
            a.category {
                color: var(--clr-wgg-orange);
                text-decoration: none;
                font-size: 20;
                font-weight: bold;
                font-family: var(--fnt-title);
                &:hover {
                    text-decoration: underline;
                }
            }
            h1 {
                margin: 0;
                margin-bottom: 17px;
                font-family: var(--fnt-title);
                font-size: 50px;
                font-weight: bold;
            }
            h2 {
                font-size: 26px;
            }
            h3,h4,h5,h6 {
                    font-weight: bold;
                    display: block;
            }
            p, ul, ol{
                font-size: 18px;
                display: block;
            }
            p, h3,h4,h5,h6, ul,ol {
                // font-size: 15px;
                    margin: 0;
                    margin-bottom: 1em;
                    &:last-child {
                        margin-bottom: 0;
                    }
            }
            code {
                font-family: Consolas, 'Courier New', Courier, monospace;
                background: #222222;
                color: white;
                padding: 5px;
                border-radius: 5px;
            }
            figure {
                max-width: 100%;
                margin: 0;
                margin-bottom: 5px;
                img {
                    border-radius: 5px;
                    width: 100%;
                }
                figcaption {
                    margin: .5em 0;
                    font-style: italic;
                    text-align: center;
                    opacity: .8;
                    font-size: 16px;
                    display: block;
                }
            }
            .float-left {
                float: left;
                margin-right: 15px;
                width: 50%;
            }
            .float-right {
                float: right;
                margin-left: 15px;
                width: 50%;
            }
            .afterfloat {
                clear: both;
            }
            .clear {
                width: 100%;
                clear: both;
                margin: 0;
                padding: 0;
            }
            ul, ol {
                padding-left: 3ch;
            }
            ul {
                list-style: disc;
                ul {
                    list-style: circle;
                    ul {
                        list-style: square;
                    }
                }
            }
            ol {
                list-style: decimal;
                ol {
                    list-style: upper-alpha;
                    ol {
                        list-style: lower-alpha;
                        ol {
                            list-style: lower-roman;
                        }
                    }
                }
            }
            li {
                margin-bottom: .5em;
                // clear: both;
                figure.float-left, figure.float-right {
                    height: 100px;
                    width: auto;
                    img {
                        height: 100%;
                        // width: auto;
                    }
                }
            }
        }
    // }
    // .article-container {
    //   position: relative;
    //   display: flex;
    //   flex-direction: column;
    //   // min-height: calc(100vh - 2 * var(--sz-header-height));
    //   height: 100%;
    //   flex: 1 1 auto;
    //   margin-bottom: 25px;
    // }

    @media only screen and (max-width: 700px) {
        .float-left, .float-right {
            width: 100% !important;
            float: none !important;
            margin-left: 0 !important; margin-right: 0 !important;
        }
        .content {
            padding-left: 10px !important;
            padding-right: 10px !important;
            font-size: .95em;
            line-height: 120%;
            max-width: calc(var(--sz-content-width) - 20px) !important;
            width: calc(100% - 20px) !important;
        }
        #wrapper {
            padding-left: 0px;
            padding-right: 0px;
            padding-top: 20px;
            div.header {
                margin-right: 15px; margin-left: 15px;
                div.info {
                    flex-wrap: wrap;
                    flex-direction: column-reverse;
                    position: relative;
                    div.readtime, div.date {
                        line-height: 20px;
                        height: 20px;
                    }
                    div.author {
                        width: 100%;
                        margin-top: 10px;
                    }
                    div.share {
                        margin: 0;
                        position: absolute;
                        top: 0;
                        right: 0;
                    }
                    div.seperator {
                        display: none;
                    }
                }
            }
            h1 {
                font-size: 45px;
            }
            p, ul, ol {
                font-size: 16px;
            }
            p, ul, ol, h2, h3, h4, h5, h6 {
                margin-left: 15px;
                margin-right: 15px;
            }
            figure {
                margin-left: 0; margin-right: 0;
                img {
                    border-radius: 0 !important;
                }
                span {
                    font-size: 14px;
                }
            }
        }
        .end {
            border-radius: 10px !important;
            padding: 5px 10px !important;
        }
    }
    @media print {
        #wrapper {
            padding: 0 !important; margin: 0 !important;
            box-shadow: none;
            h1 {
                font-size: 25px !important;
                margin-bottom: 10px !important;
            }
            div.header {
                font-size: 16px !important;
                margin-bottom: 10px !important;
            }
            p, ul, ol {
                font-size: 15px;
            }
            div.share {
                display: none;
            }
        }
    }
}
</style>