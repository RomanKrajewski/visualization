<template>
    <div class="hello">
        <h1>{{ msg }}</h1>
        <canvas ref="imagecanvas" v-bind:width="width" v-bind:height="height"
                id="canvas"></canvas>
        <button v-on:click="showCircles">show circles</button>
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
                height: 512,
                width: 512,
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
            showCircles: function () {
                this.ctx.beginPath();
                this.ctx.arc(75, 75, 50, 0, Math.PI * 2, true);
                this.ctx.fill();
            },
            handleResize: function () {
                const w = Math.min(window.innerWidth - 10, 900);
                const h = w / 2;
                this.width = w;
                this.height = h;
                this.$nextTick(() => {
                    this.showCircles();
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
