<template>
  <n-config-provider
    class="app-wrapper"
    :theme="store.darkMode ? darkTheme : lightTheme"
  >
    <n-message-provider :duration="6000" :closable="true">
      <rup-header> </rup-header>
      <rup-content> </rup-content>
    </n-message-provider>
  </n-config-provider>
</template>

<script setup lang="ts">
import { defineComponent } from "vue";
import { useStore } from "./store/store";
import { supabase } from "./supabase";
import {
  darkTheme,
  lightTheme,
  NConfigProvider,
  NMessageProvider,
} from "naive-ui";
import { RupHeader, RupContent } from "./components";

const store = useStore();

onresize = store.setWindowWidth;

supabase.auth.onAuthStateChange(async (_, session: any) => {
  const user: any = supabase.auth.user();
  let { data } = await supabase.from("profiles").select("*").single();
  store.setUser({
    ...user,
    ...data,
  });
});

defineComponent({
  darkTheme,
  lightTheme,
});
</script>

<style scoped>
.app-wrapper {
  height: 100vh;
  display: flex;
  margin: 0px;
  flex-direction: column;
}
</style>
