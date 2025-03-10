---
title: "Music"
date: 2025-03-11T02:38:00+08:00
draft: false
---

## My Music Collection

Welcome to my music space! Here you'll find my favorite music pieces.

<!-- Include APlayer CSS and JS -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/aplayer@1.10.1/dist/APlayer.min.css">
<script src="https://cdn.jsdelivr.net/npm/aplayer@1.10.1/dist/APlayer.min.js"></script>

<!-- Player container -->
<div id="aplayer"></div>

<script>
const ap = new APlayer({
    container: document.getElementById('aplayer'),
    fixed: false,
    autoplay: false,
    theme: '#b7daff',
    loop: 'all',
    order: 'list',
    preload: 'auto',
    volume: 0.7,
    mutex: true,
    listFolded: false,
    listMaxHeight: 90,
    audio: [
        {
            name: 'Samsa',
            artist: 'Yume no Kesshō ROSE',
            url: '/music/【夢ノ結唱 ROSE】 ザムザ「萨姆沙」【Synthesizer V Cover】-Dr_Black.wav',
            cover: 'https://img.icons8.com/fluency/96/music.png'
        },
        {
            name: 'Emotions',
            artist: 'Mariah Carey',
            url: '/music/Emotions - Mariah Carey.flac',
            cover: 'https://img.icons8.com/fluency/96/music.png'
        },
        {
            name: 'Netsu Ijō',
            artist: 'Yume no Kesshō ROSE',
            url: '/music/【夢ノ結唱ROSE SV】熱異常【Synth V Cover】-AsteN613.m4s',
            cover: 'https://img.icons8.com/fluency/96/music.png'
        }
    ]
});
</script>

<style>
.aplayer {
    margin: 20px 0;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    border-radius: 4px;
}
.aplayer .aplayer-info .aplayer-music .aplayer-title {
    font-size: 16px;
}
.aplayer .aplayer-info .aplayer-music .aplayer-author {
    font-size: 14px;
}
.aplayer .aplayer-list ol li {
    padding: 10px 15px;
}
</style>
