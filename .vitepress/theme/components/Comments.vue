<template>
  <div id="gitalk-container"></div>
</template>
<script lang="ts" setup>
import "gitalk/dist/gitalk.css";
import Gitalk from "gitalk";
import { onContentUpdated, useRouter } from "vitepress";

// const { route, go } = useRouter();
function deleteChild(element: HTMLDivElement | null) {
  let child = element?.lastElementChild;
  while (child) {
    element?.removeChild(child);
    child = element?.lastElementChild;
  }
}
onContentUpdated(() => {
  // reset gittalk element for update
  const element = document.querySelector("#gitalk-container");
  if (!element) {
    return;
  }
  deleteChild(element);
  const gitalk = new Gitalk({
    clientID: "Ov23lirKwAao0z2dTKYh",
    clientSecret: "4c02b078834c14052fcfc64c32de4662d16bcf1b",
    repo: "blog-comments",
    owner: "maldron0309",
    admin: ["maldron0309"],
    id: location.pathname.substring(0, 50), // Ensure uniqueness and length less than 50
    language: "en-US",
    distractionFreeMode: true, // Facebook-like distraction free mode
  });
  gitalk.render("gitalk-container");
});
</script>
<style scoped></style>
