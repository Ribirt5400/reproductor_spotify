<template>
    <div class="player-container">
    
        <div class="content-container">
            <div class="lateral-container">
                <nav>

                </nav>
            </div>
            <div class="data-container">
                <RouterView />
            </div>
        </div>

        <div class="reproductor-container">
            <audio ref="reproductor" id="Reproductor" controls>
                    <source src="https://cdn.freesound.org/previews/467/467497_3082984-hq.mp3" type="audio/mpeg">
                    Your browser does not support the audio element.
            </audio>

            <div class="control-box">
                <div class="buttons-container">
                    <i class="bi bi-skip-backward"></i>
                    <i :class="currentButtonIcon" @click="toggleReproduction"></i>
                    <i class="bi bi-skip-forward"></i>

                </div>
                <div class="range-container">
                    <input @input="changeAudioTime"  type="range" :value="songCurrentTime" min="0" :max="songFullTime" />
                </div>
                <div class="range-time-container">
                    <label class="start">{{ currentTime }}</label>
                    <label class="end">{{ endTime }}</label>
                </div>
            </div>
        </div>
    </div>
    
</template>

<script setup>
    import { ref, useTemplateRef, onMounted } from 'vue'

    let currentButtonIcon = ref("bi bi-play-circle");
    const reproductor = useTemplateRef('reproductor');

    let currentTime = ref("0:00");
    let endTime = ref("0:00");
    let songCurrentTime = ref(0);
    let songFullTime = ref(0);
    
    const toggleReproduction = () => {
        if (reproductor.value.paused) {
            currentButtonIcon.value = "bi bi-pause-circle";
            reproductor.value.play();
        } else {
            currentButtonIcon.value = "bi bi-play-circle";
            reproductor.value.pause();
        }
    };

    const formatTime = (time) => {
        let minutes = Math.floor(time / 60);
        let seconds = Math.floor(time % 60);
        return `${minutes}:${seconds < 10 ? "0" + seconds : seconds}`;
    };
    onMounted(() => {

        reproductor.value.addEventListener("timeupdate", () => {
            songCurrentTime.value = reproductor.value.currentTime;
            songFullTime.value = reproductor.value.duration;
            currentTime.value = formatTime(reproductor.value.currentTime);
            endTime.value = formatTime(reproductor.value.duration);
        });
        
    });
        const changeAudioTime = (e) => {
            reproductor.value.currentTime = e.target.value;
            currentTime.value = formatTime(reproductor.value.currentTime);
        };
        
        


</script>

<style>

</style>