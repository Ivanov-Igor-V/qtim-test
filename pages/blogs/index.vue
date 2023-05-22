<template>
    <div>
        <div id="page">
            <div class="container">
                <header id="masthead" class="site-header">
                    <div class="site-branding">
                        <h1 class="site-title"><a href="index.html" rel="home">Moschino</a></h1>
                        <h2 class="site-description">Minimalist Portfolio HTML Template</h2>
                    </div>
                    <nav id="site-navigation" class="main-navigation">
                        <button class="menu-toggle">Menu</button>
                        <a class="skip-link screen-reader-text" href="#content">Skip to content</a>
                        <div class="menu-menu-1-container">
                            <ul id="menu-menu-1" class="menu">
                                <li><a href="index.html">Home</a></li>
                                <li><a href="about.html">About</a></li>
                                <li><a href="shop.html">Shop</a></li>
                                <li><a href="blog.html">Blog</a></li>
                                <li><a href="elements.html">Elements</a></li>
                                <li><a href="#">Pages</a>
                                    <ul class="sub-menu">
                                        <li><a href="portfolio-item.html">Portfolio Item</a></li>
                                        <li><a href="blog-single.html">Blog Article</a></li>
                                        <li><a href="shop-single.html">Shop Item</a></li>
                                        <li><a href="portfolio-category.html">Portfolio Category</a></li>
                                    </ul>
                                </li>
                                <li><a href="contact.html">Contact</a></li>
                            </ul>
                        </div>
                    </nav>
                </header>
                <div id="content" class="site-content">
                    <div id="primary" class="content-area column full">
                        <main id="main" class="site-main">
                            <div class="grid bloggrid my-grid " v-if="data">

                                <article class="hentry" v-for="post in dataOfCurrentPage" :key="post.id">
                                    <NuxtLink :to="`/blogs/${post.id}`">
                                        <header class="entry-header">
                                            <div class="entry-thumbnail">

                                            </div>
                                            <h2 class="entry-title">
                                                {{ post.title }}</h2>
                                            <!-- <a class='portfoliotype' href='portfolio-category.html'>summer</a>
                                            <a class='portfoliotype' href='portfolio-category.html'>woman</a>
                                            <a class='portfoliotype' href='portfolio-category.html'>yellow</a> -->
                                        </header>
                                        <img :src="post.image"
                                            class="attachment-post-thumbnail size-post-thumbnail wp-post-image" alt="p1" />
                                    </NuxtLink>
                                </article>
                                <!-- <div v-else>
                                    Loading...
                                </div> -->

                            </div>
                            <!-- .grid -->

                            <nav class="pagination" v-if="data">
                                <!-- <span class="page-numbers current">{{ currentPage }}</span> -->
                                <a v-for="(pages, index) in numOfPages" @click="changePage" :key="index"
                                    class="page-numbers" :class="{ 'current': index + 1 === currentPage }" href="#">{{ index
                                        + 1
                                    }}</a>
                            </nav>
                            <br />

                        </main>
                        <!-- #main -->
                    </div>
                    <!-- #primary -->
                </div>
                <!-- #content -->
            </div>
            <!-- .container -->
            <footer id="colophon" class="site-footer">
                <div class="container">
                    <div class="site-info">
                        <h1
                            style="font-family: 'Herr Von Muellerhoff';color: #ccc;font-weight:300;text-align: center;margin-bottom:0;margin-top:0;line-height:1.4;font-size: 46px;">
                            Moschino</h1>
                        <a target="blank" href="https://www.wowthemes.net/">&copy; Moschino - Free HTML Template by
                            WowThemes.net</a>
                    </div>
                </div>
            </footer>
            <a href="#top" class="smoothup" title="Back to top"><span class="genericon genericon-collapse"></span></a>
        </div>
    </div>
</template>
<script >
export default {

    async setup() {
        const { public: publicConfigField } = useRuntimeConfig();
        const { data, error } = await useFetch(publicConfigField.apiBase)

        const itemsOnPage = ref(6)
        const numOfPages = computed(() => data ? Math.ceil(data.value.length / itemsOnPage.value) : 0)
        const currentPage = ref(1)
        const splittedArray = ref([])
        const dataOfCurrentPage = computed(() => splittedArray.value[currentPage.value - 1])

        const splitDataArray = (arr, chunkSize) => {
            for (let i = 0; i < arr.length; i += chunkSize) {
                splittedArray.value.push(arr.slice(i, i + chunkSize));
            }
        }

        const changePage = (e) => {
            e.preventDefault()
            currentPage.value = +e.target.innerText
        }

        watchEffect(() => {
            splitDataArray(data.value, 6)
        })

        return { data, error, numOfPages, currentPage, itemsOnPage, dataOfCurrentPage, changePage, splittedArray }
    },
}
</script>
<style lang="scss">
// Не смог подлючить скрипты через useMeta, поэтому прихожится "Доверстывать"
.my-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 10px;

    article {
        width: 100%;
    }
}
</style>