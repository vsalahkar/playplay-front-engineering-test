<template>
    <main id="app" class="playplay">
        <base-add-video :video-url="videoUrl" :video-sprite="videoSprite"/>
    </main>
</template>

<script>
import BaseAddVideo from './components/BaseAddVideo'

export default {
    name: 'app',
    components: {
        BaseAddVideo,
    },
    data: () => {
        return {
            videoUrl: 'https://storage.googleapis.com/playplay-test/medias/big_buck_bunny_360p.mp4',
            videoSprite: 'https://storage.googleapis.com/playplay-test/medias/big_buck_bunny_sprite.png'
        }
    }
}
</script>

<style lang="scss" scoped>
    @import "./styles/variables";
    @import "./styles/base";

    .playplay {
        width: 100vw;
        height: 100vh;
        display: flex;
        align-items: center;
        justify-content: center;
        background-color: $base-color-dark;
    }
</style>
