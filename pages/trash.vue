<template>
  <div>
    <Navbar :menus="menus"/>
    <div class="container">
      <br><br><br>
      <h4 class="w-full text-center pt-1 pb-1">Trashed Items</h4>
      <h5 class="w-full text-center pt-1 pb-1">Trashed Posts (temp content)</h5>
      <b-card-group deck class="mb-3">
        <b-card
          v-for="post in posts"
          :key="post._id"
          :title="post.title"
          tag="article"
          style="max-width: 15rem;min-width: 12rem;"
          class="mb-2"
        >
          <p class="card-text block-ellipsis">{{html2text(post.content)}}</p>
          <!-- <b-button href="#" variant="success">Edit</b-button> -->
          <!-- <b-button :href="editUrl(post)" variant="success">Edit</b-button> -->
          <!-- <nuxt-link class="btn btn-success" :to="{ name: 'post-edit-id', params: { id: post._id }}">Edit</nuxt-link> -->
          <b-button type="button" class="btn btn-success">Restore</b-button>
        </b-card>
        <!-- <div class="w-full flex flex-wrap overflow-hidden items-center">
          <br /><br />
          <div v-for="post in posts" :key="post._id" class="">
            <a :href="'/post/' + post._id" class="no-underline">
              {{ post.title }}
            </a>
          </div>
        </div>-->
      </b-card-group>
      <h5 class="w-full text-center pt-1 pb-1">Trashed Pages (temp content)</h5>
      <b-card-group deck class="mb-3">
        <b-card
          v-for="page in pages"
          :key="page._id"
          :title="page.title"
          tag="article"
          style="max-width: 15rem;min-width: 12rem;"
          class="mb-2"
        >
          <p class="card-text">
            {{ html2text(page.content).substring(0,80) + '...'}}
          </p>
          <b-button type="button" class="btn btn-success" @click="selectedPage=page">Restore</b-button>
        </b-card>
        <!-- <div class="w-full flex flex-wrap overflow-hidden items-center">
          <br /><br />
          <div v-for="post in posts" :key="post._id" class="">
            <a :href="'/post/' + post._id" class="no-underline">
              {{ post.title }}
            </a>
          </div>
        </div> -->
      </b-card-group>
      <no-ssr>
        <sidebar-menu :menu="sidebarMenu" @itemClick="onItemClick" />
      </no-ssr>
    </div>
    <Footer/>
  </div>
</template>

<style>
.v-sidebar-menu {
  z-index: 1031;
}
.navbar-brand {
  margin-left: 1.5rem;
}
</style>

<script>
import Navbar from '../components/NavbarBS.vue'
import Footer from '../components/Footer.vue'
import contentProcessing from '~/mixins/contentProcessing.js'
// import { SidebarMenu } from 'vue-sidebar-menu'
export default {
  mixins: [contentProcessing],
  components: {
    Navbar,
    Footer
  },
  computed: {
    menus() {
      // return this.$store.state.menus.menuItems
      return this.$store.state.menus.menuItems
    },
    posts() {
      // return this.$store.state.menus.menuItems
      return this.$store.state.posts.posts
    }
  },
  fetch ({ store, redirect }) {
    if (!store.state.authUser) {
      return redirect('/login')
    }
  },
  data() {
    return {
      show: true,
      sidebarMenu: [
        {
          header: true,
          title: 'Admin Panel'
          // component: componentName
          // visibleOnCollapse: true
        },
        {
          href: '/admin',
          title: 'Dashboard',
          icon: 'fas fa-tachometer-alt'
          /*
                        disabled: true
                        badge: {
                            text: 'new',
                            // class:''
                        }
                        */
        },
        {
          href: '/post/all',
          title: 'Posts',
          icon: 'fas fa-file'
          /*
                        disabled: true
                        badge: {
                            text: 'new',
                            // class:''
                        }
                        */
        },
        {
          href: '/pages',
          title: 'Pages',
          icon: 'fas fa-file-alt'
          /*
                        disabled: true
                        badge: {
                            text: 'new',
                            // class:''
                        }
                        */
        },
        {
          href: '/seo',
          title: 'SEO',
          icon: 'fab fa-google'
          /*
                        disabled: true
                        badge: {
                            text: 'new',
                            // class:''
                        }
                        */
        },
        {
          title: 'Theme Settings',
          icon: 'fas fa-pen-nib',
          child: [
            {
              href: '#',
              title: 'Global Settings'
            },
            {
              href: '#',
              title: 'Navbar Settings'
            },
            {
              href: '#',
              title: 'Footer Settings'
            }
          ]
        },
        {
          title: 'Plugins',
          icon: 'fas fa-plug',
          child: [
            {
              href: '#',
              title: 'Google Analytics'
            },
            {
              href: '#',
              title: 'Tawk.to Widget'
            },
            {
              href: '#',
              title: 'Mailchimp Settings'
            }
          ]
        },
        {
          href: '/users',
          title: 'Users',
          icon: 'fas fa-users'
          /*
                        disabled: true
                        badge: {
                            text: 'new',
                            // class:''
                        }
                        */
        },
        {
          href: '/trash',
          title: 'Trash',
          icon: 'fas fa-trash-alt'
          /*
                        disabled: true
                        badge: {
                            text: 'new',
                            // class:''
                        }
                        */
        },
        {
          href: '#',
          title: 'Logout',
          icon: 'fas fa-power-off'
          /*
                        disabled: true
                        badge: {
                            text: 'new',
                            // class:''
                        }
                        */
        }
      ]
    }
  },
  methods: {
    onItemClick(event, item) {
      console.log('item clicked: ' + JSON.stringify(item))
      if(item.title == 'Logout'){
        this.$nuxt.$store.dispatch('logout')
      }
    }
  }
}
</script>
