<template>
  <section class="section">
    {{titleName}}
    <nuxt-content :document="article" />
  </section>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from '@vue/composition-api'
import Card from '@/components/Card.vue'
import { Context } from '@nuxt/types'

export default defineComponent({
  name: 'ArticleDetail',
  components: {
    Card,
  },
  setup(props, context) {
    const article = ref<Object>({})
    const titleName = ref<string>('')

    // @ts-ignore
    const ctx = context.root.context
    onMounted(async () => {
      article.value = await ctx.$content('articles', context.root.$route.params.slug).fetch();
    // @ts-ignore
      const { title } = await ctx.$content('articles', context.root.$route.params.slug).only(['title']).fetch();
        titleName.value = title
    })
    return {
      article,
      titleName,
    }
  },
})
</script>
