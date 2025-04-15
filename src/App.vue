<script setup lang="ts">

import { computed, onMounted, ref, type StyleValue } from 'vue';

type Click = {
    n: number
    h: number
    m: number
    s: number
    ms: number
}

const sms = ref({
    h: 0,
    m: 0,
    s: 0,
    ms: 0,
})

const rotate = ref(0)

const marker = ref(0)

const clicks = ref<Click[]>([])

const hasClicks = computed<boolean>(() => {
    return clicks.value.length > 0
})

function onClick(): void {
    const now = new Date()
    clicks.value.unshift({
        n: clicks.value.length + 1,
        h: now.getHours(),
        m: now.getMinutes(),
        s: now.getSeconds(),
        ms: now.getMilliseconds()
    })
}

function onClear(): void {
    clicks.value = []
}

function isMarker(dotMarker: number): boolean {
    return dotMarker === Number(marker.value)
}

function interval(): void {
    const now = new Date()
    const ms = now.getMilliseconds()
    sms.value.h = now.getHours()
    sms.value.m = now.getMinutes()
    sms.value.s = now.getSeconds()
    sms.value.ms = ms
    rotate.value = ms * .36
}

onMounted(() => {
    setInterval(interval, 0)
})

</script>
<template>
<div style="max-width: 300px; margin: auto; padding: 15px 0;">

    <div class="mb-4">
        <select class="form-select" v-model="marker">
            <option disabled>Метка</option>
            <option value="0">000</option>
            <option value="125">125</option>
            <option value="250">250</option>
            <option value="375">375</option>
            <option value="500">500</option>
            <option value="625">625</option>
            <option value="750">750</option>
            <option value="875">875</option>
        </select>
    </div>


    <div class="clock">
        <div class="clock-dot" :class="{ active: isMarker(0) }"></div>
        <div class="clock-dot" :class="{ active: isMarker(250) }"></div>
        <div class="clock-dot" :class="{ active: isMarker(500) }"></div>
        <div class="clock-dot" :class="{ active: isMarker(750) }"></div>
        <div class="clock-dot" :class="{ active: isMarker(125) }"></div>
        <div class="clock-dot" :class="{ active: isMarker(375) }"></div>
        <div class="clock-dot" :class="{ active: isMarker(625) }"></div>
        <div class="clock-dot" :class="{ active: isMarker(875) }"></div>
        <div class="clock-ms-arr" :style="{ transform: `rotate(${rotate}deg)` }"></div>
        <div class="clock-time">
            <div class="clock-time-s">{{ String(sms.h).padStart(2, '0') }}</div>
            <div class="clock-time-d">:</div>
            <div class="clock-time-s">{{ String(sms.m).padStart(2, '0') }}</div>
            <div class="clock-time-d">:</div>
            <div class="clock-time-s">{{ String(sms.s).padStart(2, '0') }}</div>
            <div class="clock-time-d">:</div>
            <div class="clock-time-ms">{{ String(sms.ms).padStart(3, '0') }}</div>
        </div>
    </div><!-- .clock -->

    <div class="mb-3">
        <div class="row g-3">
            <div class="col">
                <button class="btn btn-primary w-100" @click="onClick">
                    <svg xmlns="http://www.w3.org/2000/svg" height="30px" viewBox="0 -960 960 960" width="30px" fill="#e8eaed"><path d="M80-490v-60h120v60H80Zm129.08 224.31-42.16-43.39 84-84 43.39 42.16-85.23 85.23Zm41.84-381.23-84-84 42.16-43.39 85.23 85.23-43.39 42.16ZM699.23-190 515.38-373.85l-43.84 133.08-103.85-344.61 346.15 103.84L582-434.92l181.07 181.07L699.23-190ZM402.31-720v-120h60v120h-60Zm211.38 73.08-43.38-42.16 85.23-85.23 42.15 42.16-84 85.23Z"/></svg>
                </button>
            </div><!-- .col -->
            <div class="col">
                <button class="btn btn-danger w-100" @click="onClear">
                    <svg xmlns="http://www.w3.org/2000/svg" height="30px" viewBox="0 -960 960 960" width="30px" fill="#e8eaed"><path d="M292.31-140q-29.92 0-51.12-21.19Q220-182.39 220-212.31V-720h-40v-60h180v-35.38h240V-780h180v60h-40v507.69Q740-182 719-161q-21 21-51.31 21H292.31ZM680-720H280v507.69q0 5.39 3.46 8.85t8.85 3.46h375.38q4.62 0 8.46-3.85 3.85-3.84 3.85-8.46V-720ZM376.16-280h59.99v-360h-59.99v360Zm147.69 0h59.99v-360h-59.99v360ZM280-720v520-520Z"/></svg>
                </button>
            </div><!-- .col -->
        </div><!-- .row -->
    </div>

    <div class="list-group" v-if="hasClicks">
        <div class="list-group-item" v-for="click, i in clicks">

            <div class="row g-0">
                <div class="col">
                    {{ click.n }}
                </div>

                <div class="col-2 text-center">
                    {{ String(click.h).padStart(2, '0') }}
                </div>
                <div class="col-auto">:</div>
                <div class="col-2 text-center">
                    {{ String(click.m).padStart(2, '0') }}
                </div>
                <div class="col-auto">:</div>
                <div class="col-2 text-center">
                    {{ String(click.s).padStart(2, '0') }}
                </div>
                <div class="col-auto">:</div>
                <div class="col-3 text-center">
                    {{ String(click.ms).padStart(3, '0') }}
                </div>
            </div><!-- v-for -->

        </div><!-- .list-group-item -->
    </div><!-- .list-group -->

</div>
</template>