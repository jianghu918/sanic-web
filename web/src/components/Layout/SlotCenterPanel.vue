<script lang="ts" setup>
interface Props {
  loading?: boolean
}
withDefaults(defineProps<Props>(), {
  loading: false,
})

const userStore = useUserStore()
const router = useRouter()
const logout = () => {
  userStore.logout()
  setTimeout(() => {
    router.push('/login')
  }, 500)
}
const handleClickEfficiency = (index) => {
  router.push('/testAssitant')
}

const handleClickChat = (index) => {
  router.push('/chat')
}

const handleClick = () => {
  router.push('/mcpChat')
}

// 使用ref定义响应式的selectedIndex
const selectedIndex = ref(0)
onMounted(() => {
  // 初始化selectedIndex为0
  router.currentRoute.value.path === '/testAssitant'
    ? (selectedIndex.value = 2)
    : (selectedIndex.value = 1)
})
</script>

<template>
  <LayoutSlotFrame :class="['bg-no-repeat bg-cover bg-center']">
    <template #center>
      <div
        flex="~"
        size-full
        overflow-hidden
        class="panel-shadow"
      >
        <n-spin
          w-full
          h-full
          content-class="w-full h-full flex"
          :show="false"
          :rotate="false"
          class="bg-#ffffff"
          :style="{
            '--n-opacity-spinning': '0',
          }"
        >
          <section
            flex="~ col"
            min-w-0
            w-70
            h-full
            overflow-hidden
            relative
          >
            <NavigationSideBar />
          </section>
          <section
            flex="1 ~"
            min-w-0
            h-full
            overflow-hidden
            py-12
            pr-12
            style="background: linear-gradient(to bottom, #8874f1, #588af9)"
          >
            <div
              size-full
              rounded-10
              overflow-hidden
            >
              <LayoutDefault />
            </div>
          </section>
        </n-spin>
      </div>
    </template>
    <template #bottom>
      <NavigationNavFooter />
    </template>
  </LayoutSlotFrame>
</template>

<style lang="scss" scoped>
.panel-shadow {
  --shadow: 50px 50px 100px 10px rgb(0 0 0 / 10%);
  --at-apply: 'shadow-[--shadow]';
}
</style>
