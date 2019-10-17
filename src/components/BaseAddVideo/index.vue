<template>
    <article class="baseAddVideo">
        <header class="baseAddVideo__header">
            <h1>Add a video</h1>
            <button title="Close and cancel add a video"><i class="material-icons">close</i></button>
        </header>
        <main class="baseAddVideo__video">
            <video :src="videoUrl"></video>
            <div class="baseAddVideo__trimmer">
                <button class="trimmer trimmer--start">
                    <i class="material-icons">chevron_left</i>
                    <span>00:12:53</span>
                </button>
                <img :src="videoSprite" alt="">
                <button class="trimmer trimmer--end">
                    <i class="material-icons">chevron_right</i>
                    <span>00:12:53</span>
                </button>
            </div>
        </main>
        <footer class="baseAddVideo__actions">
            <div class="actions__parameters">
                <button class="button button--primary">
                    <i class="material-icons">play_arrow</i>
                </button>
                <form class="actions__framing">
                    <label class="framing framing__start">
                        Start:
                        <input type="number">
                    </label>
                    <label class="framing framing__end">
                        End:
                        <input type="number">
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
    props: {
        videoUrl: {
            type: String,
            default: ''
        },
        videoSprite: {
            type: String,
            default: ''
        },
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
                font-size: 13px;
                color: $secondary-color;
            }

            &:first-child span {
                left: 0;
            }

            &:last-child span {
                right: 0;
            }

            i {
                font-size: 16px;
                color: $base-color-light;
            }

            &--start {
                left: -2px;
                border-top-left-radius: 4px;
                border-bottom-left-radius: 4px;
            }

            &--end {
                right: -2px;
                border-top-right-radius: 4px;
                border-bottom-right-radius: 4px;
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
