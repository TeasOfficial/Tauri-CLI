<script setup>
import { getCurrentWindow } from '@tauri-apps/api/window'
import { listen } from '@tauri-apps/api/event'

var isMaximized = ref(false)
const appWindow = getCurrentWindow()
async function setupWindowListeners() {
    await listen('tauri://resize', async() => {
        isMaximized.value = await appWindow.isMaximized()
    })
    await listen('tauri://restore', async() => {
        isMaximized.value = await appWindow.isMaximized()
    })
}
const reload = () => {
    location.reload()
}
setupWindowListeners()
</script>

<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated>
      <q-bar
        data-tauri-drag-region
        class="cursor-default non-selectable"
      >
        <span
          class="test-menu cursor-pointer"
          data-tauri-drag-region
        >
          <q-icon style="font-size: 24px;" name="mdi-bed-outline" />
          <q-menu>
            <q-list
              dense
              style="min-width: 100px"
            >
              <q-item
                v-close-popup
                clickable
                @click="reload()"
              >
                <q-item-section>刷新页面（devMode）</q-item-section>
              </q-item>

              <q-separator />

              <q-item
                v-close-popup
                clickable
                @click="appWindow.close()"
              >
                <q-item-section color="red">放我离开！</q-item-section>
              </q-item>
            </q-list>
          </q-menu>
        </span>
        <div data-tauri-drag-region>
          BedCore - 床核CLI - Quasar + Tauri
        </div>

        <q-space data-tauri-drag-region />

        <q-btn
          dense
          flat
          icon="minimize"
          @click="appWindow.minimize()"
        />
        <q-btn
          v-if="isMaximized"
          dense
          flat
          icon="mdi-circle-double"
          @click="appWindow.toggleMaximize()"
        />
        <q-btn
          v-else
          dense
          flat
          icon="mdi-circle-outline"
          @click="appWindow.toggleMaximize()"
        />
        <q-btn
          dense
          flat
          icon="mdi-window-close"
          color="red"
          @click="appWindow.close()"
        />
      </q-bar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>
<style scoped>
.q-bar {
  background-color: aliceblue;
  color: black;
}
.test-menu{
  margin: 0;
  padding: 0;
}
</style>
