<script setup lang="js">
import { TresCanvas, useRenderLoop } from
    '@tresjs/core';
import { shallowRef } from 'vue';
import { GLTFModel, OrbitControls, } from '@tresjs/cientos'

import Room from '/Models/IsoBedroom.glb'


const { onLoop } = useRenderLoop()


const cubeRef = shallowRef()
onLoop(({ delta, elapsed }) => {
    if (cubeRef.value) {
        cubeRef.value.rotation.y += delta
        cubeRef.value.position.y = Math.sin(elapsed)
    }
})

const roomRef = shallowRef()
onLoop(({ delta, elapsed }) => {
    if (roomRef.value) {
        roomRef.value.rotation.y += delta / 2
        roomRef.value.position.y = Math.sin(elapsed) / 2
    }
})
</script>

<template>
    <TresCanvas alpha v-motion-slide-visible-right>
        <TresPerspectiveCamera :zoom="0.6" />
        <OrbitControls :enable-zoom="false" />
        <TresMesh ref="roomRef">
            <Suspense>
                <GLTFModel :path="Room" />
            </Suspense>
        </TresMesh>
        <TresAmbientLight :intensity="1" />
        <TresDirectionalLight :position="[-4, 8, 4]" :intensity="1.5" cast-shadow />
        <TresDirectionalLight :position="[-4, 30, 4]" :intensity="0.2" />
    </TresCanvas>
</template>
