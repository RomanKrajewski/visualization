<template>
    <div class="hello">
        <h1>{{ msg }}</h1>
        <canvas ref="imagecanvas" v-bind:width="canvasWidth" v-bind:height="canvasHeight"
                id="canvas"></canvas>
    </div>
</template>

<script>
    export default {
        name: 'HelloWorld',
        props: {
            msg: String
        },
        data: function () {
            return {
                canvasHeight: 512,
                canvasWidth: 512,
            };
        },
        computed: {
            canvas: function () {
                return this.$refs.imagecanvas;
            },
            ctx: function () {
                return this.canvas.getContext('2d');
            }
        },
        methods: {
            scaleToCanvas: function(x){
                return x/100*this.canvasWidth
            },
            showCircles: function (showRed, radius) {
                function getRandomInt(max) {
                    return Math.floor(Math.random() * Math.floor(max));
                }
                let distanceBetweenCircles = radius*4
                let randomFactor = distanceBetweenCircles/2
                let nCirclesX = Math.floor(100/distanceBetweenCircles)
                let nCirclesY = Math.floor(50/distanceBetweenCircles)
                let redX = -1
                let redY = -1
                if(showRed){
                    redX = getRandomInt(nCirclesX)
                    redY = getRandomInt(nCirclesY)
                }
                for(let i = 0; i < nCirclesX; i++){
                    for(let j = 0; j < nCirclesY; j++){
                        if(i===redX && j===redY){
                          this.ctx.fillStyle = "red"
                        }
                        else{
                            this.ctx.fillStyle = "black"
                        }
                        this.ctx.beginPath();
                        this.ctx.arc(this.scaleToCanvas(distanceBetweenCircles * (i+1) - Math.random() * randomFactor - radius),
                            this.scaleToCanvas(distanceBetweenCircles * (j+1) - Math.random() * randomFactor - radius),
                            this.scaleToCanvas(radius), 0, Math.PI * 2, true);
                        this.ctx.fill();
                    }
                }
            },
            handleResize: function () {
                const w = Math.min(window.innerWidth - 10, 900);
                const h = w / 2;
                this.canvasWidth = w;
                this.canvasHeight = h;
                this.$nextTick(() => {
                    this.showCircles(true,4);
                })
            }
        },
        mounted: function () {
            this.handleResize()
            window.addEventListener('resize', this.handleResize)
        },
        beforeDestroy: function () {
            window.removeEventListener('resize', this.handleResize)
        }
    }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h3 {
        margin: 40px 0 0;
    }

</style>
