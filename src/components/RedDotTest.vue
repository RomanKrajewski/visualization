<template>
    <div class="redDotTest">
        <h1>Gibt es auf dem Bild einen roten Punkt?</h1>
        <h2 v-show="exampleStage">Beispiel</h2>
        <canvas v-show="showCanvas" ref="imagecanvas" v-bind:width="canvasWidth" v-bind:height="canvasHeight"
                id="canvas"></canvas>
        <button v-show="exampleStage" v-on:click="$emit('start-test', 0)" >Test starten</button>
        <button v-show="!exampleStage && !showCanvas" v-on:click="yesClicked" >Ja</button>
        <button v-show="!exampleStage && !showCanvas" v-on:click="noClicked">Nein</button>
    </div>
</template>

<script>
    export default {
        name: 'RedDotTest',
        props: {
            exampleStage: Boolean
        },
        data: function () {
            return {
                showRed: true,
                showCanvas: false,
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
            },
            startTest: function(showForMs){
                this.showRed = Math.random() < 0.5;
                this.ctx.clearRect(0, 0, this.canvasWidth, this.canvasHeight);
                this.showCircles(this.showRed, 3)
                this.showCanvas = true;
                setTimeout(() => { this.showCanvas = false}, showForMs);
            },
            yesClicked: function(){
                if(this.showRed){
                    this.$emit("test-result", "correct")
                }
                else{
                    this.$emit("test-result","wrong")
                }
            },
            noClicked: function(){
                if(this.showRed){
                    this.$emit("test-result","wrong")
                }
                else{
                    this.$emit("test-result","correct");
                }
            }
        },
        mounted: function () {
            this.handleResize()
            this.$nextTick(() => {
                this.showCircles(this.showRed,4);
            })
            this.showCanvas = true;
        }
    }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h3 {
        margin: 40px 0 0;
    }
    button {
        background-color: #555555; /* Green */
        border: none;
        color: white;
        padding: 15px 32px;
        text-align: center;
        text-decoration: none;
        display: inline-block;
        font-size: 16px;
        margin: 5px;
    }

</style>
