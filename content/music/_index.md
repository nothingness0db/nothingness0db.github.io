---
title: "音乐"
date: 2025-03-11T02:38:00+08:00
draft: false
---

## 我的音乐收藏

欢迎来到我的音乐空间！这里收录了我喜欢的音乐作品。

<div class="music-player">
    <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=100% height=450 src="https://music.163.com/outchain/player?type=0&id=8152976921&auto=0&height=430"></iframe>
</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
    // 检查音乐播放器是否加载
    setTimeout(function() {
        var iframe = document.querySelector('.music-player iframe');
        var placeholder = document.querySelector('.music-player-placeholder');
        
        if (iframe && iframe.clientHeight < 50) {
            // 如果iframe高度异常，显示备用播放器
            if (placeholder) {
                placeholder.style.display = 'block';
            }
        }
    }, 3000);
});
</script>

<!-- 备用播放器 -->
<div class="music-player-placeholder" style="display:none;">
    <p>主播放器加载失败，请尝试使用以下链接访问：</p>
    <p><a href="https://music.163.com/#/playlist?id=8152976921" target="_blank">在网易云音乐中打开</a></p>
</div>

<style>
.music-player {
    margin: 20px 0;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 4px 8px rgba(0,0,0,0.1);
}

.music-player iframe {
    display: block;
    border-radius: 8px;
}

.music-player-placeholder {
    margin: 20px 0;
    padding: 20px;
    background-color: #f8f9fa;
    border-radius: 8px;
    text-align: center;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.music-player-placeholder a {
    display: inline-block;
    margin-top: 10px;
    padding: 8px 16px;
    background-color: #1ecc94;
    color: white;
    border-radius: 4px;
    text-decoration: none;
    font-weight: bold;
}

.music-player-placeholder a:hover {
    background-color: #19b383;
}
</style>
