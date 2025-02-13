<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue';
import PlayList from './components/PlayList.vue';
import TopInfo from './components/TopInfo.vue';


let eventSource: EventSource | null = null;

const play_info = ref();
const play_list = ref([{12345: "我爱你"},{67890: "你爱我"},{123456789100: "什么都不爱，我只我爱你。我只我爱你。我只我爱你。"},{123456789100: "什么都不爱，我只我爱你。我只我爱你。我只我爱你。"},{123456789100: "什么都不爱，我只我爱你。我只我爱你。我只我爱你。"}]);

const initSSE = () => {
    eventSource = new EventSource('http://localhost:8080/sse');
 
    eventSource.onmessage = (event) => {
        const data = JSON.parse(event.data);
        if(data.type == "playinfo"){
            play_info.value = data.data.data;
        }
        if(data.type == "playlist"){
            play_list.value = data.data.playlist;
        }
    };
 
    eventSource.onerror = (error) => {
        console.error("SSE 连接出错：", error);
        eventSource?.close();
        initSSE();
    };
}
 
onMounted(() => {
    initSSE();
});
 
onUnmounted(() => {
    if (eventSource) {
        eventSource.close();
    }
});

</script>

<template>
<TopInfo :data="play_info"/>
<PlayList :list="play_list"/>
</template>


<style>
  :root {
      --c-accent: #f06e8e;
  }
  html {
      font-size: 25px;
  }
  @keyframes slidein {
      from {transform: translateY(-5rem);}
      to {transform: translateY(0);}
  }
  body {
      margin: 0;
      padding: 0;
      background-color: gray;
      width: 100vw;
      height: 100vh;
      font-family: 'HarmonyOS Sans SC';
      overflow: hidden;
  }
</style>
