<template>
  <section class="section">
    <div v-for="(article, index) in articles" :key="index" class="article-content">
      <a :href="article.path">
        <h2>{{article.title}}</h2>
      </a>
      <nuxt-content :document="article" />
      <hr />
    </div>
  </section>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from '@vue/composition-api'
import Card from '@/components/Card.vue'
import { Context } from '@nuxt/types'

export default defineComponent({
  name: 'Articles',
  components: {
    Card,
  },
  setup(props, context) {
    const articles = ref<Array<any>>([]);
    // @ts-ignore
    const ctx = context.root.context
    onMounted(async () => {
      articles.value = await ctx.$content('articles').fetch()
    })
    return {
      articles,
    }
  },
})
</script>

<style lang="scss">
.article-content:not(:last-child) {
  margin-bottom: 2rem;
}
</style>