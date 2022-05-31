<template>
  <q-layout view="lHh Lpr lFf">
    <q-header>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />
      </q-toolbar>
      <div class="q-pl-lg q-pt-xl q-pb-md">
        <div class="text-h3">Todo</div>
        <div class="text-subtitle-1">{{ Mydte }}</div>
      </div>
      <q-img
        src="~/assets/img/marguerite.jpg"
        class="header-image absolute-top"
      />
    </q-header>

    <q-drawer v-model="drawer" show-if-above :width="250" :breakpoint="600">
      <q-scroll-area
        style="
          height: calc(100% - 185px);
          margin-top: 185px;
          border-right: 1px solid #ddd;
        "
      >
        <q-list padding>
          <q-item to="/" exact clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="list" />
            </q-item-section>

            <q-item-section> Todo </q-item-section>
          </q-item>

          <q-item to="/help" exact clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="help" />
            </q-item-section>

            <q-item-section> Help </q-item-section>
          </q-item>
        </q-list>
      </q-scroll-area>

      <q-img
        class="absolute-top header-image"
        src="~/assets/img/marguerite.jpg"
        style="height: 185px"
      >
        <div class="absolute-bottom bg-transparent">
          <q-avatar size="56px" class="q-mb-sm">
            <img src="https://cdn.quasar.dev/img/boy-avatar.png" />
          </q-avatar>
          <div class="text-weight-bold">Aziza za</div>
          <div>@ziza</div>
        </div>
      </q-img>
    </q-drawer>

    <q-page-container>
      <transition
        enter-active-class="animated fadeInLeft"
        appear
        :duration="300"
      >
        <router-view />
      </transition>
    </q-page-container>
  </q-layout>
</template>

<script>
import { defineComponent, ref } from "vue";
import { date } from "quasar";
import { computed } from "vue";

export default defineComponent({
  name: "MainLayout",

  setup() {
    const drawer = ref(false);
    const Mydte = computed(() => {
      return date.formatDate(Date.now(), "dddd DD MMMM YYYY");
    });
    return {
      drawer,
      miniState: ref(true),

      toggleLeftDrawer() {
        drawer.value = !drawer.value;
      },
      Mydte,
    };
  },
});
</script>
<style lang="scss">
.header-image {
  height: 100%;
  z-index: -1;
  opacity: 0.7;
  filter: grayscale(30%);
}
</style>
