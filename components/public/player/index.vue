<template>
    <div class="player">
        <video-player  class="vjs-custom-skin"
                       ref="videoPlayer"
                       :options="playerOptions"
                       :playsinline="true"
                       @timeupdate="onPlayerTimeupdate($event)"
                       @ready="playerReadied($event)">
        </video-player>
    </div>
</template>

<script>
    import 'video.js/dist/video-js.css'
    import './index.css';

    import { videoPlayer } from 'vue-video-player'
    // Similarly, you can also introduce the plugin resource pack you want to use within the component
    // import 'some-videojs-plugin'
    export default {
        data() {
            return {
                // videojs options
                playerOptions: {
                    height: '360',
                    autoplay: true,
                    language: 'en',
                    playbackRates: [0.5,0.75, 1.0, 1.25,1.5, 2.0],
                    sources: [{
                        type: "video/mp4",
                        // mp4
                        src: "http://vjs.zencdn.net/v/oceans.mp4",
                        // webm
                        // src: "https://cdn.theguardian.tv/webM/2015/07/20/150716YesMen_synd_768k_vp8.webm"
                    }],
                    poster: "https://surmon-china.github.io/vue-quill-editor/static/images/surmon-1.jpg",
                },
                storage: window.sessionStorage,
                isPause: false,
                time: 0
            }
        },
        computed: {
            player() {
                return this.$refs.videoPlayer.player
            },
            currentTime(){
                return this.$store.state.currentTime;
            }
        },
        methods: {
            // listen event
            onPlayerTimeupdate(player) {
                //记录当前播放进度
                this.time = player.currentTime();
            },
            playerReadied(player) {
                // 读取断点，从断点处开始播放
                player.currentTime(this.currentTime);
            }
        },
        components: {
            videoPlayer
        },
        mounted(){
            //监听页面刷新，记录断点
            var _this = this;
            window.addEventListener("beforeunload", function() {
                _this.$store.state.currentTime = _this.player.currentTime();
                _this.storage.ct = _this.$store.state.currentTime;
            });
            //监听按键
            document.onkeydown = function(e){
                var keyCode = e.which | e.keyCode;    //按键码兼容
                if(keyCode == 32&&!_this.isPause){      //空格键暂停
                    _this.player.pause();
                    _this.isPause = true;
                    return;
                }
                if(keyCode == 32&&_this.isPause){       //空格键播放
                    _this.player.play();
                    _this.isPause = false;
                    return;
                }

                if(keyCode == 37){                       //左键快退5秒
                    _this.time -= 5;
                    _this.player.currentTime(_this.time);
                }
                if(keyCode == 39){                        //右键块退5秒
                    _this.time += 5;
                    _this.player.currentTime(_this.time);
                }
            }
        },
        //监听路由跳转，记录断点
        beforeRouteLeave (to, from, next) {
            this.$store.state.currentTime = this.player.currentTime();
            this.storage.ct = this.$store.state.currentTime;
            next();
        }
    }
</script>
