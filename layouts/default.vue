<template>
  <div>
    <nav class="navbar header has-shadow is-primary" role="navigation" aria-label="main navigation">
      <div class="navbar-brand">
        <a class="navbar-item" href="/">
          <img src="~assets/buefy.png" alt="Buefy" height="28" />
        </a>

        <div class="navbar-burger">
          <span />
          <span />
          <span />
        </div>
      </div>
    </nav>

    <section class="main-content columns">
      <aside class="column is-2 section">
        <p class="menu-label is-hidden-touch">General</p>
        <b-menu v-for="(item, key) of state.items" :key="key">
          <b-menu-item :icon="item.menu.icon" expanded>
            <template slot="label" slot-scope="props">
              <nuxt-link :to="item.menu.to" exact-active-class="is-active">
                {{item.menu.title}}
              </nuxt-link>
                <b-icon
                  v-if="item.menuChild"
                  class="is-pulled-right"
                  :icon="props.expanded ? 'menu-down' : 'menu-up'"
                />
            </template>
            <div v-if="item.menuChild" class="menuChild">
              <div v-for="(childMenu, index) in item.menuChild" :key="index">
                <a :href="childMenu.to.name" exact-active-class="is-active">
                  <b-menu-item :icon="childMenu.icon" :label="childMenu.title" />
                </a>
              </div>
            </div>
          </b-menu-item>
        </b-menu>
      </aside>

      <div class="container column is-10">
        <nuxt />
      </div>
    </section>
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, onMounted, ref } from '@vue/composition-api'

interface Menu {
  menu: MenuItem
  menuChild?: MenuItem[]
}
interface MenuItem {
  title: string
  icon: string
  to: { name: string }
}
export default defineComponent({
  name: 'default',
  setup(props, { root }) {
    const articles = ref<Array<any>>([])
    // @ts-ignore
    const ctx = root.context

    let articleItems = new Array()

    onMounted(async () => {
      articles.value = await ctx.$content('articles').fetch()

      articles.value.forEach((a) => {
        let articleItem = {
          title: '',
          icon: 'pencil',
          to: { name: '' },
        }
        articleItem.title = a.title
        articleItem.to.name = a.path

        articleItems.push(articleItem)
      })
    })

    const menuContent: Menu[] = [
      {
        menu: {
          title: 'Home',
          icon: 'home',
          to: { name: 'index' },
        },
      },
      {
        menu: {
          title: 'Inspire',
          icon: 'lightbulb',
          to: { name: 'inspire' },
        },
      },
      {
        menu: {
          title: 'Articles',
          icon: 'book-open',
          to: { name: 'articles' },
        },
        menuChild: articleItems,
      },
    ]
    const state = reactive({
      items: menuContent,
    });

    return {
      state
    }
  },
})
</script>

<style lang="scss">
li {
  list-style: none;
}
.menuChild {
  margin-left: 1rem;
}
</style>
