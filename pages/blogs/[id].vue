<template >
    <main id="main" class="site-main" role="main">
        <NuxtLink to="/"> <button>Home</button> </NuxtLink>
        <article>
            <header class="entry-header">
                <h1 class="entry-title"> {{ data?.title }}</h1>
                <div class="entry-meta">
                    <span class="posted-on"><time class="entry-date published">{{ data?.createdAt }}</time></span>
                    <span class="comments-link"><a href="#">{{ data?.preview }}</a></span>
                </div>
                <div class="entry-thumbnail">
                    <img :src="data?.image" alt="picture">
                </div>
            </header>
            <!-- .entry-header -->
            <div class="entry-content">?
                {{ data?.description }}
            </div>
            <!-- .entry-content -->
            <footer class="entry-footer">
                <span class="cat-links">
                    Posted in
                    <a href="#" rel="category tag">audio</a>,
                    <a href="#" rel="category tag">embed</a>,
                    <a href="#" rel="category tag">media</a>
                </span>
            </footer>
            <!-- .entry-footer -->
        </article>
        <!-- #post-## -->
        <nav class="navigation post-navigation" role="navigation">
            <h1 class="screen-reader-text">Post navigation</h1>
            <div class="nav-links">
                <div class="nav-previous">
                    <a href="#" rel="prev"><span class="meta-nav">←</span> Thanks for watching!</a>
                </div>
            </div>
            <!-- .nav-links -->
        </nav>
        <!-- .navigation -->
        <div id="comments" class="comments-area">
            <div id="respond" class="comment-respond">
                <h3 id="reply-title" class="comment-reply-title">Leave a Reply <small><a rel="nofollow"
                            id="cancel-comment-reply-link" href="/demo-moschino/embed-audio/#respond"
                            style="display:none;">Cancel reply</a></small></h3>
                <form action="http://www.themepush.com/demo-moschino/wp-comments-post.php" method="post" id="commentform"
                    class="comment-form">
                    <p class="comment-notes">
                        <span id="email-notes">Your email address will not be published.</span> Required
                        fields are marked <span class="required">*</span>
                    </p>
                    <p class="comment-form-comment">
                        <label for="comment">Comment</label><textarea id="comment" name="comment" cols="45" rows="8"
                            aria-required="true" required=true></textarea>
                    </p>
                    <p class="comment-form-author">
                        <label for="author">Name <span class="required">*</span></label><input id="author" name="author"
                            type="text" value="" size="30" aria-required="true" required>
                    </p>
                    <p class="comment-form-email">
                        <label for="email">Email <span class="required">*</span></label><input id="email" name="email"
                            type="email" value="" size="30" aria-describedby="email-notes" aria-required="true" required>
                    </p>
                    <p class="comment-form-url">
                        <label for="url">Website</label><input id="url" name="url" type="url" value="" size="30">
                    </p>
                    <p class="form-submit">
                        <input name="submit" type="submit" id="submit" class="submit" value="Post Comment"><input
                            type="hidden" name="comment_post_ID" value="90" id="comment_post_ID">
                        <input type="hidden" name="comment_parent" id="comment_parent" value="0">
                    </p>
                    <noscript>
                    </noscript>
                </form>
            </div>
            <!-- #respond -->
        </div>
        <!-- #comments -->
    </main>
</template>
<script lang="ts">
import { _AsyncData } from 'nuxt/dist/app/composables/asyncData'
import { FetchError } from 'ofetch'


interface BlogDetails {
    id: number,
    createdAt: string,
    title: string,
    preview: string,
    image: string,
    description: string
}
export default {
    async setup() {
        const route = useRoute();
        const { public: publicConfigField } = useRuntimeConfig();
        const currentBlogDetailsURL = computed(() => `${publicConfigField.apiBase}/${route.params.id}`);
        const { data, error }: _AsyncData<BlogDetails, FetchError<any> | null> = await useFetch(currentBlogDetailsURL);
        return { data, error }
    }
}
</script>
<style lang="scss" scoped></style>