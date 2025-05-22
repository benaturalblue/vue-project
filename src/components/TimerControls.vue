<template>
  <div>
    <button @click="$emit('start')">スタート</button>
    <button @click="$emit('pause')">一時停止</button>
    <button @click="$emit('reset')">リセット</button>
  </div>
</template>
