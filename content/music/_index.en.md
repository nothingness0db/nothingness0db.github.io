---
title: "Music"
date: 2025-03-11T02:38:00+08:00
draft: false
---

## My Music Collection

Welcome to my music space! Here you'll find my favorite music pieces.

<div class="music-player">
    <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=100% height=450 src="https://music.163.com/outchain/player?type=0&id=8152976921&auto=0&height=430"></iframe>
</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
    // Check if music player loaded properly
    setTimeout(function() {
        var iframe = document.querySelector('.music-player iframe');
        var placeholder = document.querySelector('.music-player-placeholder');
        
        if (iframe && iframe.clientHeight < 50) {
            // If iframe height is abnormal, show backup player
            if (placeholder) {
                placeholder.style.display = 'block';
            }
        }
    }, 3000);
});
</script>

<!-- Backup player -->
<div class="music-player-placeholder" style="display:none;">
    <p>Main player failed to load. Please try the following link:</p>
    <p><a href="https://music.163.com/#/playlist?id=8152976921" target="_blank">Open in NetEase Cloud Music</a></p>
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
