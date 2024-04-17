<template>
  <a-dropdown @select="handleSelect">
    <a-avatar :style="{ backgroundColor: '#3370ff' }">
      <IconUser />
      <!--        {{ store.state.user?.loginUser?.userName ?? "未登录" }}-->
    </a-avatar>
    <template #content>
      <template v-if="store.state.user?.loginUser.userName === 未登录">
        <a-doption>
          <a-button type="text" @click="handleLogin">登录</a-button>
        </a-doption>
        <a-doption>
          <a-button type="text" @click="handleRegister">注册</a-button>
        </a-doption>
      </template>
      <template v-else>
        <a-doption>
          <a-button type="text" @click="handleLogout">注销</a-button>
        </a-doption>
      </template>
    </template>
  </a-dropdown>
</template>

<style></style>
<script setup lang="ts">
import { IconUser } from "@arco-design/web-vue/es/icon";
import { useStore } from "vuex";
import { useRouter } from "vue-router";
import { UserControllerService } from "../../generated";
import message from "@arco-design/web-vue/es/message";

const store = useStore();
const router = useRouter();

const handleLogin = () => {
  router.push({ path: "/user/login", replace: true });
};
const handleRegister = () => {
  router.push({ path: "/user/register", replace: true });
};
const handleLogout = async () => {
  const res = await UserControllerService.userLogoutUsingPost();
  if (res.code === 0) {
    message.success("注销成功");
    router.push({ path: "/user/login", replace: true });
  } else {
    message.error("注销失败，" + res.message);
  }
};
</script>
