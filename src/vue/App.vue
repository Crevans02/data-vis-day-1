<template>
    <div class="w-full min-h-screen flex flex-col justify-center items-center bg-gray-100 text-sans px-4 sm:px-0">
        <h1 class="text-2xl sm:text-3xl mb-12 text-gray-700">Color Step Generator</h1>
        <div class="w-full max-w-2xl p-4 sm:p-8 shadow-2xl rounded-lg bg-white">
            <div class="flex border border-gray-200 border-solid rounded overflow-hidden">
                <el-popover
                  v-for="color in colorArray"
                  :style="{ backgroundColor: color }"
                  class="w-full h-16"
                  placement="top"
                  width="200"
                  trigger="hover">
                    <div class="flex flex-col text-xs">
                        <div class="flex mb-1"><div class="text-gray-500 w-1/4">RGB:</div><div class="text-gray-800 w-3/4">{{ color }}</div></div>
                        <div class="flex"><div class="text-gray-500 w-1/4">Hex:</div><div class="text-gray-800 w-3/4">{{ rgbToHex(color) }}</div></div>
                    </div>
                    <div class="flex h-full" slot="reference"></div>
                </el-popover>
            </div>
            <div class="flex justify-between mt-4 sm:mt-8">
                <div class="order-1">
                    <el-color-picker v-model="colorA" @change="createSteps" @active-change="activeChangeA"></el-color-picker>
                </div>
                <div class="order-2">
                    <el-input-number v-model="steps" @change="createSteps" :min="2" :max="100"></el-input-number>
                </div>
                <div class="order-3">
                    <el-color-picker v-model="colorB" @change="createSteps" @active-change="activeChangeB"></el-color-picker>
                </div>
            </div>
        </div>
        <p class="mt-12 text-gray-500 text-xs">
            <a href="https://datavis.co" target="_blank" class="underline hover:text-yellow-500">#100DaysOfDataVis</a>
            <span> by </span>
            <a href="https://twitter.com/madebycrevans" target="_blank" class="underline hover:text-blue-400">@MadeByCrevans</a>
        </p>
    </div>
</template>

<script lang="ts">
import Vue from "vue";
import * as d3 from "d3";

export default Vue.extend({
    data() {
        return {
            colorA: "#B6F7B6",
            colorB: "#077FFF",
            colorArray: [],
            steps: 8
        };
    },
    methods: {
        activeChangeA(color) {
            this.colorA = color;
            this.createSteps();
        },
        activeChangeB(color) {
            this.colorB = color;
            this.createSteps();
        },
        createSteps() {
            let clr = d3.scaleLinear().range([this.colorA, this.colorB]).domain([0, this.steps -1]);
            this.colorArray = d3.range(this.steps).map((d) => clr(d));
        },
        componentToHex(c) {
            const hex = Number(c).toString(16);
            return hex.length === 1 ? "0" + hex : hex;
        },
        rgbToHex(color) {
            let [r, g, b] = color.replace("rgb(", "").replace(")", "").split(", ");
            return "#" + this.componentToHex(r) + this.componentToHex(g) + this.componentToHex(b);
        }

    },
    created() {
       this.createSteps();
    }
});
</script>
