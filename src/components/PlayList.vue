<script setup lang="ts">

const props = defineProps({
    list: [Object]
})

</script>

<template>
<div class="play-out-box">
    <div class="play-top">
        播放列表
    </div>
    <div class="play-in-list">
        <div v-for="(item, index) in props.list" class="play-single">
            <div class="play-info">
                <div class="play-index">
                    {{ index + 1 }}
                </div>
                <div class="play-title">
                    {{ item.title }}
                </div>
                <div class="play-aid">
                    av{{ item.aid }}
                </div>
            </div>
            <div class="play-sender">
                点播: {{ item.sender }}
            </div>
        </div>
        <div v-if="props.list?.length == 0" class="play-notice">
            当前列表为空，可以发弹幕<br />「<b>点播</b> BV号 / av号」进行点播！
        </div>
    </div>
</div>
</template>

<style scoped>
.play-out-box {
    position: absolute;
    display: flex;
    flex-direction: column;
    top: 7rem;
    left: 1rem;
    width: 18rem;
    height: 31.5rem;
    background-color: rgba(255, 255, 255, 0.6);
    border: 2px solid white;
    border-radius: 0.75rem;
    padding: 0.5rem 1rem;
    box-sizing: border-box;
    box-shadow: 0.2rem 0.2rem 0.2rem rgba(0, 0, 0, 0.2);
}
.play-in-list {
    flex: 1;
    overflow-y: scroll;
}
.play-index {
    /* width: 1.5rem; */
    margin-right: 0.3rem;
    display: flex;
    align-items: center;
    font-size: 2rem;
    color: rgba(0, 0, 0, 0.225);
    font-weight: bold;
}
.play-notice {
    height: 100%;
    margin-top: 1rem;
    text-align: center;
    color: #0000007a;
}
.play-top {
    text-align: center;
    font-weight: bold;
    font-size: 1.2rem;
    margin-bottom: 0.2rem;
}

.play-sender {
    color: #0000007a;
    font-size: 0.8rem;
}

.play-single {
    border-bottom: 2px solid gray;
    display: flex;
    flex-direction: column;
    padding-bottom: 0.25rem;
}

.play-info {
    width: 100%;
    min-height: 2rem;
    display: flex;
    justify-content: space-between; 
}

.play-title {
    font-family: 'HarmonyOS Sans SC';
    display: flex;
    align-items: center;
    flex: 1;
}
.play-aid {
    display: flex;
    align-items: center;
    margin-left: 0.5rem;
    font-family: 'Montserrat';
    font-weight: bold;
    width: 5.5rem;
    word-break: break-all;
}

::-webkit-scrollbar {
  width: 0; /* Safari,Chrome 隐藏滚动条 */
  height: 0; /* Safari,Chrome 隐藏滚动条 */
  display: none; /* 移动端、pad 上Safari，Chrome，隐藏滚动条 */
}
</style>