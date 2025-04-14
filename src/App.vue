<script setup lang="ts">

import { computed, ref } from 'vue';

type Click = {
    h: string
    m: string
    s: string
    ms: string
}

const clicks = ref<Click[]>([])

const hasClicks = computed<boolean>(() => {
    return clicks.value.length > 0
})

function onClick(): void {

    const now = new Date()

    clicks.value.push({
        h: String(now.getHours()),
        m: String(now.getMinutes()),
        s: String(now.getSeconds()),
        ms: String(now.getMilliseconds())
    })
}

</script>
<template>
<div style="max-width: 300px; margin: auto; padding: 15px 0;">

    <div class="mb-3">
        <button class="btn btn-primary w-100" @click="onClick">
            click
        </button>
    </div>

    <div class="list-group" v-if="hasClicks">
        <div class="list-group-item" v-for="click, i in clicks">

            <div class="row g-0">
                <div class="col-2">
                    {{ i + 1 }}
                </div>

                <div class="col-2 text-center">
                    {{ click.h }}
                </div>
                <div class="col-auto">:</div>
                <div class="col-2 text-center">
                    {{ click.m }}
                </div>
                <div class="col-auto">:</div>
                <div class="col-2 text-center">
                    {{ click.s }}
                </div>
                <div class="col-auto">:</div>
                <div class="col-3 text-center">
                    {{ click.ms }}
                </div>
            </div><!-- v-for -->

        </div><!-- .list-group-item -->
    </div><!-- .list-group -->

</div>
</template>