<template>
  <div class="com-marquee-box"
       ref="MarqueeBox">
    <p class="com-marquee-info"
       ref="MarqueeInfo"
       :style="'left:'+left+'px'">{{content}}<span class="copy-info">{{copycontent}}</span></p>
  </div>
</template>
<script>
export default {
    name: 'ComMarquee',
    components: {
    },
    props: {
        content: {
            type: String,
            default: ''
        }
    },
    data () {
        return {
            boxWidth: 0,
            infoWidth: 0,
            left: 0,
            timer: null,
            copycontent: ''
        };
    },
    watch: {

    },
    computed: {

    },
    methods: {
        init () {
            this.$nextTick(() => {
                let boxWidth = this.$refs.MarqueeBox.clientWidth;
                let infoWidth = this.$refs.MarqueeInfo.clientWidth;
                let left = 0;
                if (infoWidth > boxWidth) {
                    this.copycontent = this.content;
                    left = infoWidth * -1;
                    this.left = 40;
                    setInterval(() => {
                        if (this.left === left) {
                            this.left = 0;
                        } else {
                            this.left--;
                        }
                    }, 20);
                }
            });
        }
    },
    beforeCreate () {
    },
    beforeDestroy () {
        clearInterval(this.timer);
    },
    created () {
    },
    mounted () {
        this.init();
    }
};
</script>
<style lang="scss" scoped>
.com-marquee-box {
  width: 100%;
  height: 100%;
  overflow: hidden;
  position: relative;
  .com-marquee-info {
    position: absolute;
    white-space: nowrap;
  }
  .copy-info {
    padding-left: 20px;
  }
}
</style>