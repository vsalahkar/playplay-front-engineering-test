<template>
    <article class="baseAddVideo">
        <header class="baseAddVideo__header">
            <h1>Add a video</h1>
            <button title="Close and cancel add a video"><i class="material-icons">close</i></button>
        </header>
        <main class="baseAddVideo__video">
            <video :src="videoUrl" ref="videoElement"></video>
            <div class="baseAddVideo__trimmer">
                <button class="trimmer trimmer--start" :style="`left: calc(${trimStartXPosition}px - 2px)`">
                    <i class="material-icons">chevron_left</i>
                    <span>{{trimStartTime}}</span>
                </button>
                <img :src="videoSprite">
                <button class="trimmer trimmer--end" :style="`left: calc(${trimEndXPosition}px - 18px)`">
                    <i class="material-icons">chevron_right</i>
                    <span>{{trimEndTime}}</span>
                </button>
                <div class="trimmer__currentTimeCursor" :style="`left: ${this.cursorXPosition}px`"></div>
            </div>
        </main>
        <footer class="baseAddVideo__actions">
            <div class="actions__parameters">
                <button class="button button--primary" @click="toggleVideoPlayback">
                    <i class="material-icons">{{ isVideoPlaying ? 'pause' : 'play_arrow'}}</i>
                </button>
                <form class="actions__framing">
                    <label class="framing framing__start">
                        Start:
                        <input type="text" v-model="trimStartTime">
                    </label>
                    <label class="framing framing__end">
                        End:
                        <input type="text" v-model="trimEndTime">
                    </label>
                </form>
            </div>
            <div class="actions__save">
                <button class="button button--secondary">cancel</button>
                <button class="button button--primary">save</button>
            </div>
        </footer>
    </article>
</template>

<script>
export default {
    mounted() {
        const videoDomElement = this.$refs.videoElement

        videoDomElement.onloadeddata = () => {
            const videoDuration = videoDomElement.duration

            this.trimEndTime = this.formatVideoDuration(videoDuration)
        }
    },
    data: () => {
        return {
            isVideoPlaying: false,
            cursorXPosition: 16,
            cursorXPositionPolling: null,
            trimStartTime: 0,
            trimEndTime: 0,
            trimStartXPosition: 0,
            trimEndXPosition: 0
        }
    },
    props: {
        videoUrl: {
            type: String,
            default: ''
        },
        videoSprite: {
            type: String,
            default: ''
        },
    },
    methods: {
        toggleVideoPlayback() {
            const videoDomElement = this.$refs.videoElement

            if (!videoDomElement.paused) {
                videoDomElement.pause()
            } else {
                videoDomElement.play()
            }

            this.isVideoPlaying = !videoDomElement.paused

            this.cursorXPositionPolling = setInterval(() => {
                this.setCursorXPosition(videoDomElement)
            }, 500)
        },
        setCursorXPosition(domElement) {
            const videoCurrentTime = domElement.currentTime
            const videoDuration = domElement.duration

            const videoWidth = domElement.offsetWidth - 32 // video frame has the same width as the frame bar

            this.cursorXPosition = 16 + (videoCurrentTime / videoDuration * videoWidth)
        },
        formatVideoDuration(duration) {
            const minutes = Math.floor(duration / 60)
            const secondsRemainder = duration % 60
            const seconds = Math.floor(secondsRemainder)
            const mSecondsRemainder = Math.round((secondsRemainder - seconds) * 100)

            return `${minutes} : ${seconds} : ${mSecondsRemainder}`
        },
        parseVideoDuration(duration) {
            const minutes = parseInt(duration[0], 10) * 60 || 0
            const seconds = parseInt(duration[1], 10) || 0
            const mSeconds = parseInt(duration[2], 10) / 100 || 0

            console.info(minutes + seconds + mSeconds)

            return minutes + seconds + mSeconds
        },
        setTrimmerXPosition(timeToParse) {
            const videoDomElement = this.$refs.videoElement

            const timeFrameWidth = videoDomElement.offsetWidth
            const videoDuration = videoDomElement.duration

            const durationArray = timeToParse.split(' : ')
            const trimmedDuration = this.parseVideoDuration(durationArray)

            console.info(trimmedDuration / videoDuration * timeFrameWidth)

            return trimmedDuration / videoDuration * timeFrameWidth
        }
    },
    watch: {
        trimStartTime(newValue, oldValue) {
            if (newValue !== oldValue) {
                this.trimStartXPosition = this.setTrimmerXPosition(this.trimStartTime)
            }
        },
        trimEndTime(newValue, oldValue) {
            if (newValue !== oldValue) {
                this.trimEndXPosition = this.setTrimmerXPosition(this.trimEndTime)
            }
        }
    },
    beforeDestroy() {
        clearInterval(this.cursorXPositionPolling)
    }
}
</script>

<style lang="scss" scoped>
    @import "../../styles/variables";
    @import "../../styles/base";
    @import "../../styles/objects/button";

    .baseAddVideo {
        border-radius: 8px;
        overflow: hidden;
        width: 100%;
        max-width: 660px;

        &__header {
            background-color: $base-color-light;
            padding: 16px;
            display: flex;
            align-items: center;
            justify-content: space-between;

            h1,
            i {
                color: $tertiary-color;
            }

            h1 {
                font-size: 18px;
                font-weight: 700;
            }

            button:hover i {
                color: $base-color-dark;
            }
        }

        &__video {
            background-color: $base-color-light-1;
            padding: 16px;

            video {
                width: 100%;
            }
        }

        &__trimmer {
            position: relative;
            margin-top: 24px;
            border: 2px solid $secondary-color;
            border-radius: 4px;
            display: flex;
            align-items: center;

            img {
                width: 100%;
            }
        }

        .trimmer {
            position: absolute;
            background-color: $secondary-color;
            height: 100%;

            span {
                position: absolute;
                top: -24px;
                width: max-content;
                font-size: 13px;
                color: $secondary-color;
            }

            i {
                font-size: 16px;
                color: $base-color-light;
            }

            &--start {
                left: -2px;
                border-top-left-radius: 4px;
                border-bottom-left-radius: 4px;

                span {
                    left: 0;
                }
            }

            &--end {
                right: -2px;
                border-top-right-radius: 4px;
                border-bottom-right-radius: 4px;

                span {
                    right: 0;
                }
            }

            &__currentTimeCursor {
                position: absolute;
                top: 50%;
                transform: translateY(-50%);
                background-color: $accent-color-1;
                height: 100%;
                width: 2px;
                cursor: ew-resize;
            }
        }

        &__actions {
            background-color: $base-color-light;
            padding: 12px 16px;
            display: flex;
            align-items: center;
            justify-content: space-between;
        }

        .actions {
            &__parameters {
                display: flex;
                align-items: center;

                & > button {
                    margin-right: 8px;
                    border-radius: 8px;
                    padding-top: 10px;
                    padding-bottom: 10px;
                }
            }

            &__framing {
                display: flex;
                align-items: center;
            }

            &__save {
                display: flex;
                align-items: center;

                button:last-child {
                    margin-left: 16px;
                }
            }
        }

        .framing {
            padding: 8px;
            display: flex;
            align-items: center;
            font-size: 14px;
            color: $base-color-dark;

            input {
                margin-left: 8px;
                border: 2px solid $base-color-light-2;
                border-radius: 4px;
                padding: 6px;
                max-width: 92px;
                color: $base-color-dark;
                font-size: 13px;
            }

            &__start {

            }

            &__end {

            }
        }
    }
</style>
