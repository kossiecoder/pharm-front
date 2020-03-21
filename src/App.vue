<template>
  <div id="app">
    <AppNavbar />

    <MemberStat />

    <MemberList />
  </div>
</template>

<script>
import AppNavbar from "@/components/AppNavbar";
import MemberList from "@/components/MemberList";
import MemberStat from "@/components/MemberStat";

export default {
  components: {
    AppNavbar,
    MemberList,
    MemberStat,
  },
}
</script>
