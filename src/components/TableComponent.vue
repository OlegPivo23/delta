<script setup>
import { ref } from 'vue'
import ChartComponent from '@/components/ChartComponent.vue'

const selectedIndex = ref(null)

const tableData = [
  { name: 'Выручка, руб', values: [500521, 480521, 4805121], percentages: [4, 5, 6] },
  { name: 'Наличные', values: [300000, 300000, 300000], percentages: [0, 0, 0] },
  { name: 'Безналичный расчет', values: [100521, 100521, 100521], percentages: [0, 0, 0] },
  { name: 'Кредитные карты', values: [100521, 100521, 100521], percentages: [0, 0, 0] },
  { name: 'Средний чек, руб', values: [1300, 900, 1300], percentages: [0, -3, 0] },
  { name: 'Средний гость, руб', values: [1200, 1300, 1000], percentages: [-5, 50, 0] },
  { name: 'Удаления из чека (после оплаты), руб', values: [100, 50, 150], percentages: [0, 0, 0] },
  { name: 'Удаления из чека (до оплаты), руб', values: [1300, 1400, 1300], percentages: [0, 0, 0] },
  { name: 'Количество чеков', values: [34, 36, 32], percentages: [0, 0, 0] },
  { name: 'Количество гостей', values: [34, 36, 32], percentages: [0, 0, 0] }
]

const selectRow = (index) => {
  selectedIndex.value = selectedIndex.value === index ? null : index
}
</script>

<template>
  <div>
    <div class="table">
      <div class="header">
        <p class="indicators">Показатели</p>
        <p class="current-day">Текущий день</p>
        <p class="yesterday">Вчера</p>
        <p class="week-day">Это день недели</p>
      </div>
      <div v-for="(row, index) in tableData" :key="index">
        <div class="row" @click="selectRow(index)">
          <p class="name">{{ row.name }}</p>
          <p class="current-day">
            {{ row.values[0] }}
          </p>
          <p
            class="yesterday"
            :style="{
              background:
                row.percentages[1] > 0
                  ? '#dff0d8'
                  : row.percentages[1] < 0
                    ? '#f2dede'
                    : '#f5f5f5'
            }"
          >
            {{ row.values[1] }}
            <span
              :style="{
                color: row.percentages[1] > 0 ? 'green' : row.percentages[1] < 0 ? 'red' : 'gray'
              }"
            >
              {{ row.percentages[1] }}%
            </span>
          </p>
          <p class="week-day">
            {{ row.values[2] }}
          </p>
        </div>
        <Transition name="fade">
          <div v-if="selectedIndex === index" class="chart-container">
            <ChartComponent :data="tableData[selectedIndex].values" />
          </div>
        </Transition>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.header,
.row {
  display: grid;
  grid-template-columns: 2fr 1fr 1fr 1fr;
  text-align: center;
  border: 1px solid #ddd;
}

.header {
  background-color: #f5f5f5;
  font-weight: bold;
}

.row {
  cursor: pointer;
  background-color: #f5f5f5;

  &:hover {
    background-color: #eef;
  }
}

.indicators {
  display: flex;
  justify-content: center;
  align-items: center;
}

.name {
  text-align: left;
  padding-left: 10px;
  display: flex;
  align-items: center;
}

.current-day,
.yesterday,
.week-day {
  padding: 5px;
  position: relative;
}

.current-day span,
.yesterday span,
.week-day span {
  position: absolute;
  right: 10px;
}

.current-day {
  background-color: #dff0d8;
}

.yesterday {
  background-color: #f5f5f5;
}

.week-day {
  background-color: #f2dede;
}

.chart-container {
  margin-top: 10px;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
