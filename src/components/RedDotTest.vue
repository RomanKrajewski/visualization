<template>
    <div class="redDotTest">
        <canvas v-show="showCanvas" ref="imagecanvas" v-bind:width="canvasWidth" v-bind:height="canvasHeight"
                id="canvas"></canvas>
        <h2 v-show="exampleStage">Beispiel</h2>
        <p v-show="exampleStage">Es werden mehrere solcher Bilder gezeigt. Bitte gib nach jedem Bild an, ob ein roter Punkt zu sehen war.
            Pass gut auf, die Bilder werden immer schneller verschwinden!
        </p>
        <button v-show="exampleStage" v-on:click="$emit('start-test', 0)" >Test starten</button>
        <h1 v-show="!exampleStage && !showCanvas" >War eben ein Roter Punkt zu sehen?</h1>
        <button v-show="!exampleStage && !showCanvas" v-on:click="yesClicked" >Ja</button>
        <button v-show="!exampleStage && !showCanvas" v-on:click="noClicked">Nein</button>
        <button v-show="!exampleStage && !showCanvas" v-on:click="dontKnowClicked">Keine Ahnung </button>
    </div>
</template>

<script>
    export default {
        name: 'RedDotTest',
        props: {
            exampleStage: Boolean,
            currentTestStage: Number
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
            showCircles: function (showRed,showMultipleColors, showSquares, radius) {
                const colorPalette = ["#cc1ea1", "#0feca4", "#ba9075", "#c68648", "#fdc0ea", "#5042b1", "#7fe828", "#e70af2", "#ebd846", "#724c9c", "#e3e7e4", "#d7f4bb", "#3fa730", "#494a7d", "#2596fa", "#6aa3be"]
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
                            if(showMultipleColors){
                                this.ctx.fillStyle = colorPalette[getRandomInt(colorPalette.length - 1)]
                            }
                            else {
                                this.ctx.fillStyle = "black"
                            }
                        }
                        if(showSquares && Math.random() < 0.5 && !(i===redX) && ! (j===redY)){
                            this.ctx.fillStyle = "red";
                            this.ctx.fillRect(this.scaleToCanvas(distanceBetweenCircles * (i+1) - Math.random() * randomFactor - radius),
                                this.scaleToCanvas(distanceBetweenCircles * (j+1) - Math.random() * randomFactor - radius),
                                this.scaleToCanvas(2*radius), this.scaleToCanvas(2*radius))
                        }
                        else{
                            this.ctx.beginPath();
                            this.ctx.arc(this.scaleToCanvas(distanceBetweenCircles * (i+1) - Math.random() * randomFactor - radius),
                                this.scaleToCanvas(distanceBetweenCircles * (j+1) - Math.random() * randomFactor - radius),
                                this.scaleToCanvas(radius), 0, Math.PI * 2, true);
                            this.ctx.fill();
                        }
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
                this.render(this.showRed)
                setTimeout(() => { this.showCanvas = false}, showForMs);
            },
            render: function(showRed){
                this.ctx.clearRect(0, 0, this.canvasWidth, this.canvasHeight);
                switch(this.currentTestStage){
                    case 0:
                        this.showCircles(showRed, false,false, 3)
                        break;
                    case 1:
                        this.showCircles(showRed,false,false, 1)
                        break;
                    case 2:
                        this.showCircles(showRed, true,false, 3)
                        break;
                    case 3:
                        this.showCircles(showRed, false,true, 3)
                }
                this.showCanvas = true;
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
            },
            dontKnowClicked: function(){
                this.$emit("test-result", "wrong");
            }
        },
        mounted: function () {
            this.handleResize()
            this.$nextTick(() => {
                this.render(true);
            })
        }
    }

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    h3 {
        margin: 40px 0 0;
    }


</style>
