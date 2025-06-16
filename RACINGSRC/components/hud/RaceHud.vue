<template>
  <div class="race">
    <div class="race-container">
      <!-- Racer List (unchanged) -->
      <div class="positions-container">
        <RacerList :racers="globalStore.activeRace.positions" />
      </div>
      
      <!-- Main Race Info -->
      <div class="race-info-section">
        <!-- Race Name -->
        <div class="race-name-container">
          <h2 class="race-name">{{ globalStore.activeRace.raceName }}</h2>
        </div>

        <!-- Position Display -->
        <div 
          class="position-container"
          v-if="globalStore.activeRace.totalRacers && globalStore.activeRace.totalRacers !== 1"
        >
          <span class="position-text">{{ globalStore.activeRace.position }}/{{ globalStore.activeRace.totalRacers }}</span>
        </div>

        <!-- Timing Information Cards -->
        <div class="timing-cards">
          <!-- Lap Information -->
          <div class="timing-card lap-card">
            <div class="card-icon">
              <v-icon icon="mdi-flag-triangle" class="lap-icon"></v-icon>
            </div>
            <div class="card-content">
              <span class="card-label">LAP</span>
              <span class="card-value">{{ lapText }}</span>
            </div>
          </div>

          <!-- Total Time -->
          <div class="timing-card total-card">
            <div class="card-icon">
              <v-icon icon="mdi-clock-outline" class="total-icon"></v-icon>
            </div>
            <div class="card-content">
              <span class="card-label">TOTAL</span>
              <span class="card-value">{{ msToHMS(globalStore.activeRace.totalTime) }}</span>
            </div>
          </div>

          <!-- Current Lap Time -->
          <div class="timing-card current-card">
            <div class="card-icon">
              <v-icon icon="mdi-clock-fast" class="current-icon"></v-icon>
            </div>
            <div class="card-content">
              <span class="card-label">CURRENT</span>
              <span class="card-value">{{ msToHMS(globalStore.activeRace.time) }}</span>
            </div>
          </div>

          <!-- Best Lap -->
          <div class="timing-card best-card">
            <div class="card-icon">
              <v-icon icon="mdi-star" class="best-icon"></v-icon>
            </div>
            <div class="card-content">
              <span class="card-label">BEST</span>
              <span class="card-value">{{ msToHMS(globalStore.activeRace.bestLap) }}</span>
            </div>
          </div>

          <!-- Checkpoint -->
          <div class="timing-card checkpoint-card">
            <div class="card-icon">
              <v-icon icon="mdi-map-marker-check" class="checkpoint-icon"></v-icon>
            </div>
            <div class="card-content">
              <span class="card-label">CHECKPOINT</span>
              <span class="card-value">{{ globalStore.activeRace.currentCheckpoint }}/{{ globalStore.activeRace.totalCheckpoints }}</span>
            </div>
          </div>
        </div>

        <!-- Ghosted Indicator -->
        <div class="ghosted-card" v-if="globalStore.activeRace.ghosted">
          <div class="card-icon">
            <v-icon icon="mdi-ghost" class="ghost-icon"></v-icon>
          </div>
          <div class="card-content">
            <span class="card-label">GHOSTED</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import RacerList from "./RacerList.vue";
import { useGlobalStore } from "@/store/global";
import { msToHMS } from "@/helpers/msToHMS";
import { computed } from "vue";

const globalStore = useGlobalStore();

const lapText = computed(() => {
  if (globalStore.activeRace.totalLaps === 0) return "SPRINT";
  else if (globalStore.activeRace.totalLaps === -1)
    return `${globalStore.activeRace.currentLap}/-`;
  return `${globalStore.activeRace.currentLap}/${globalStore.activeRace.totalLaps}`;
});
</script>

<style scoped lang="scss">
.race {
  position: absolute;
  top: 20px;
  right: 20px;
  z-index: 100;
  font-family: 'Roboto', 'Arial', sans-serif;
}

.race-container {
  display: flex;
  flex-direction: row;
  gap: 20px;
  align-items: flex-start;
}

.positions-container {
  // RacerList positioning remains unchanged
}

.race-info-section {
  display: flex;
  flex-direction: column;
  gap: 8px;
  min-width: 280px;
}

.race-name-container {
  background: rgba(0, 0, 0, 0.85);
  backdrop-filter: blur(10px);
  border-radius: 8px;
  padding: 12px 16px;
  border-left: 4px solid #00d1f5;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
}

.race-name {
  color: #ffffff;
  font-size: 1.3rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin: 0;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.8);
}

.position-container {
  background: linear-gradient(90deg, #00d1f5 0%, #0099cc 100%);
  color: #000000;
  padding: 10px 16px;
  border-radius: 8px;
  text-align: center;
  box-shadow: 0 4px 12px rgba(0, 209, 245, 0.4);
  margin-bottom: 4px;
}

.position-text {
  font-weight: 700;
  font-size: 1.1rem;
  text-shadow: none;
}

.timing-cards {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.timing-card {
  display: flex;
  align-items: center;
  background: rgba(0, 0, 0, 0.85);
  backdrop-filter: blur(10px);
  border-radius: 8px;
  padding: 12px 16px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
  transition: all 0.3s ease;
  border-left: 4px solid;
}

.timing-card:hover {
  transform: translateX(2px);
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.4);
}

.lap-card {
  border-left-color: #4CAF50;
}

.total-card {
  border-left-color: #2196F3;
}

.current-card {
  border-left-color: #FF9800;
}

.best-card {
  border-left-color: #FFD700;
}

.checkpoint-card {
  border-left-color: #FF5722;
}

.ghosted-card {
  display: flex;
  align-items: center;
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(10px);
  border-radius: 8px;
  padding: 12px 16px;
  border-left: 4px solid #9C27B0;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
  margin-top: 4px;
}

.card-icon {
  margin-right: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 24px;
  height: 24px;
}

.card-content {
  display: flex;
  flex-direction: column;
  flex: 1;
}

.card-label {
  color: rgba(255, 255, 255, 0.8);
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 2px;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.8);
}

.card-value {
  color: #ffffff;
  font-size: 1.1rem;
  font-weight: 700;
  font-family: 'Roboto Mono', monospace;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.8);
}

// Icon Colors
.lap-icon {
  color: #4CAF50;
  font-size: 20px;
}

.total-icon {
  color: #2196F3;
  font-size: 20px;
}

.current-icon {
  color: #FF9800;
  font-size: 20px;
}

.best-icon {
  color: #FFD700;
  font-size: 20px;
}

.checkpoint-icon {
  color: #FF5722;
  font-size: 20px;
}

.ghost-icon {
  color: #9C27B0;
  font-size: 20px;
}

// Responsive Design
@media (max-width: 1200px) {
  .race-info-section {
    min-width: 240px;
  }
  
  .race-name {
    font-size: 1.1rem;
  }
  
  .card-value {
    font-size: 1rem;
  }
}

@media (max-width: 768px) {
  .race {
    top: 10px;
    right: 10px;
  }
  
  .race-container {
    flex-direction: column;
  }
  
  .race-info-section {
    min-width: 200px;
  }
  
  .timing-card {
    padding: 10px 12px;
  }
  
  .race-name {
    font-size: 1rem;
  }
  
  .card-value {
    font-size: 0.95rem;
  }
}

// Text shadows for better readability
* {
  text-shadow: 0 1px 3px rgba(0, 0, 0, 0.8);
}
</style>