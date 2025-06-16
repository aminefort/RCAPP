<template>
  <div class="race">
    <div class="race-container">
      <!-- Racer List (unchanged) -->
      <div class="positions-container">
        <RacerList :racers="globalStore.activeRace.positions" />
      </div>
      
      <!-- Main Race Info Panel -->
      <div class="race-info-panel">
        <!-- Race Name and Position Header -->
        <div class="race-header">
          <h2 class="race-name">{{ globalStore.activeRace.raceName }}</h2>
          <div 
            class="position-display"
            v-if="globalStore.activeRace.totalRacers && globalStore.activeRace.totalRacers !== 1"
          >
            <span class="position-text">{{ globalStore.activeRace.position }}/{{ globalStore.activeRace.totalRacers }}</span>
          </div>
        </div>

        <!-- Timing Information -->
        <div class="timing-section">
          <!-- Lap Information -->
          <div class="timing-row lap-row">
            <div class="timing-icon">
              <v-icon icon="mdi-flag-checkered" class="flag-icon"></v-icon>
            </div>
            <div class="timing-content">
              <span class="timing-label">LAP</span>
              <span class="timing-value">{{ lapText }}</span>
            </div>
          </div>

          <!-- Total Time -->
          <div class="timing-row">
            <div class="timing-icon">
              <v-icon icon="mdi-timer-outline" class="time-icon"></v-icon>
            </div>
            <div class="timing-content">
              <span class="timing-label">TOTAL</span>
              <span class="timing-value">{{ msToHMS(globalStore.activeRace.totalTime) }}</span>
            </div>
          </div>

          <!-- Current Lap Time -->
          <div class="timing-row">
            <div class="timing-icon">
              <v-icon icon="mdi-timer-sync-outline" class="time-icon"></v-icon>
            </div>
            <div class="timing-content">
              <span class="timing-label">CURRENT</span>
              <span class="timing-value">{{ msToHMS(globalStore.activeRace.time) }}</span>
            </div>
          </div>

          <!-- Best Lap -->
          <div class="timing-row">
            <div class="timing-icon">
              <v-icon icon="mdi-star" class="star-icon"></v-icon>
            </div>
            <div class="timing-content">
              <span class="timing-label">BEST</span>
              <span class="timing-value">{{ msToHMS(globalStore.activeRace.bestLap) }}</span>
            </div>
          </div>

          <!-- Checkpoint -->
          <div class="timing-row">
            <div class="timing-icon">
              <v-icon icon="mdi-map-marker" class="checkpoint-icon"></v-icon>
            </div>
            <div class="timing-content">
              <span class="timing-label">CHECKPOINT</span>
              <span class="timing-value">{{ globalStore.activeRace.currentCheckpoint }}/{{ globalStore.activeRace.totalCheckpoints }}</span>
            </div>
          </div>
        </div>

        <!-- Ghosted Indicator -->
        <div class="ghosted-indicator" v-if="globalStore.activeRace.ghosted">
          <v-icon icon="mdi-ghost-outline" class="ghost-icon"></v-icon>
          <span class="ghost-text">GHOSTED</span>
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
import { translate } from "@/helpers/translate";

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
  top: 0;
  right: 0;
  z-index: 100;
  padding: 20px;
  font-family: 'Roboto', 'Arial', sans-serif;
}

.race-container {
  display: flex;
  flex-direction: row;
  gap: 20px;
  align-items: flex-start;
}

.positions-container {
  // Keep existing RacerList positioning
}

.race-info-panel {
  background: linear-gradient(135deg, rgba(0, 0, 0, 0.85) 0%, rgba(20, 20, 20, 0.9) 100%);
  backdrop-filter: blur(10px);
  border-radius: 12px;
  border: 1px solid rgba(0, 209, 245, 0.2);
  padding: 20px;
  min-width: 280px;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.4);
}

.race-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  padding-bottom: 15px;
  border-bottom: 1px solid rgba(0, 209, 245, 0.2);
}

.race-name {
  color: #ffffff;
  font-size: 1.4rem;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin: 0;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
}

.position-display {
  background: linear-gradient(90deg, #00d1f5 0%, #0099cc 100%);
  color: #000;
  padding: 8px 16px;
  border-radius: 20px;
  font-weight: 700;
  font-size: 1.1rem;
  box-shadow: 0 4px 12px rgba(0, 209, 245, 0.3);
}

.position-text {
  text-shadow: none;
}

.timing-section {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.timing-row {
  display: flex;
  align-items: center;
  background: rgba(0, 0, 0, 0.3);
  border-radius: 8px;
  padding: 12px 16px;
  border-left: 3px solid #00d1f5;
  transition: all 0.3s ease;
}

.timing-row:hover {
  background: rgba(0, 209, 245, 0.1);
  transform: translateX(2px);
}

.lap-row {
  border-left-color: #4CAF50;
}

.timing-icon {
  margin-right: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 24px;
}

.timing-content {
  display: flex;
  flex-direction: column;
  flex: 1;
}

.timing-label {
  color: rgba(255, 255, 255, 0.7);
  font-size: 0.75rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 2px;
}

.timing-value {
  color: #ffffff;
  font-size: 1.1rem;
  font-weight: 700;
  font-family: 'Roboto Mono', monospace;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.5);
}

.time-icon {
  color: #00d1f5;
  font-size: 20px;
}

.flag-icon {
  color: #4CAF50;
  font-size: 20px;
}

.star-icon {
  color: #FFD700;
  font-size: 20px;
}

.checkpoint-icon {
  color: #FF6B35;
  font-size: 20px;
}

.ghosted-indicator {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 8px;
  margin-top: 15px;
  padding: 10px;
  background: rgba(255, 255, 255, 0.1);
  border-radius: 8px;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.ghost-icon {
  color: #ffffff;
  font-size: 18px;
}

.ghost-text {
  color: #ffffff;
  font-size: 0.9rem;
  font-weight: 600;
  text-transform: uppercase;
  letter-spacing: 1px;
}

// Responsive adjustments
@media (max-width: 1200px) {
  .race-info-panel {
    min-width: 240px;
  }
  
  .race-name {
    font-size: 1.2rem;
  }
}

@media (max-width: 768px) {
  .race {
    padding: 10px;
  }
  
  .race-container {
    flex-direction: column;
  }
  
  .race-info-panel {
    min-width: 200px;
  }
}
</style>