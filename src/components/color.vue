<template>
    <div class="col color border border-dark animate-animated" :class="clase" :id="color" @click="seleccionar()"></div>
</template>

<script>
export default {
    name: 'color',
    props: { color: String },
    data() {
        return { clase: '' }
    },
    methods: {
        seleccionar() {
            this.$emit('elegir', this.color);
            this.clase = 'animate__bounceIn';
            var audio = new Audio(this.color + '.mp3');
            audio.play();
            setTimeout(() => this.clase = '', 500);
        }
    }
}
</script>
<style>
.color {
    height: 150px;
}
</style>
