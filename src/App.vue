<script setup lang="ts">
import { message } from 'ant-design-vue';
import { gsap } from 'gsap'
import { ScrollToPlugin } from 'gsap/ScrollToPlugin'

gsap.registerPlugin(ScrollToPlugin);

import { onMounted, onUnmounted, ref } from 'vue';
import PlayList from './components/PlayList.vue';
import TopInfo from './components/TopInfo.vue';

let eventSource: EventSource | null = null;

let t1 = gsap.timeline();
let t2 = gsap.timeline();

const play_info = ref();
const play_list = ref([
    {aid: 123,title: "我不爱你"},{aid: 123,title: "我不爱你"},{aid: 123,title: "我不爱你"},
    {aid: 123,title: "我爱你"},{aid: 123,title: "我不爱你"},{aid: 123,title: "我不爱你"},
    {aid: 123,title: "我爱你"},
    {aid: 123,title: "我爱你"},
    {aid: 123,title: "我爱你"},
    {aid: 123,title: "我爱你"},
    {aid: 123,title: "我爱你"},
    {aid: 123,title: "我爱你"}]);

const initSSE = () => {
    eventSource = new EventSource('http://localhost:8080/sse');
 
    eventSource.onmessage = (event) => {
        const data = JSON.parse(event.data);
        if(data.type == "playinfo"){
            play_info.value = data.data.data;
            t2.restart();
        }
        if(data.type == "playlist"){
            play_list.value = data.data.playlist;
            resetTimeline1();
        }
        if(data.type == "notice"){
            if(data.data.state == "success"){
              message.success({
                content: data.data.message,
                class: "bigger-top"
            })
            }
            if(data.data.state == "error"){
              message.error({
                content: data.data.message,
                class: "bigger-top"
            })
            }
        }
    };
 
    eventSource.onerror = (error) => {
        console.error("SSE 连接出错：", error);
        eventSource?.close();
        initSSE();
    };
}

function resetTimeline2(){
    t2.clear()

    t2.from(
        '.work',
        {
            duration: 1,
            scaleY: 0,
            ease: 'expo.out'
        },
        0
    )

    t2.from(
        '.uploader',
        {
            duration: 1,
            scaleY: 0,
            ease: 'expo.out'
        },
        0
    )
}

function resetTimeline1(){
    t1.clear(true);

    document.getElementsByClassName("play-in-list")[0].scrollTop = 0;

    t1.to('.play-in-list', {
      duration: 15,
      scrollTo: { y: "max" },
      ease: 'sine.inOut',
      repeat: -1,
      yoyo: true
    });

    t1.from(
        '.play-single',
        {
            duration: 0.5,
            scaleY: 0,
            ease: 'back.out(1.8)'
        },
        0
    );

    t1.restart();
}

onMounted(() => {
    initSSE();
    resetTimeline1();
    resetTimeline2();
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
  body {
      margin: 0;
      padding: 0;
      background-color: gray;
      width: 100vw;
      height: 100vh;
      font-family: 'HarmonyOS Sans SC';
      overflow: hidden;
  }
  .bigger-top {
    font-size: 1.25rem;
    font-weight: bold;
    .anticon {
        font-size: 1.25rem;
        transform: translateY(-0.15rem);
    }
  }
</style>
