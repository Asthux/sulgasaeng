<template>
    <div class="card border-left-success shadow h-100 py-2">
        <div class="card-body">
            <div class="row no-gutters align-items-center">
                <div class="col mr-2">
                    <div class="font-weight-bold text-success text-uppercase mb-1">{{ month }}월 지출
                        </div>
                        <!-- 당월 지출 카드에 바인딩-->
                        <div v-if="isLoading" class="h5 mb-0 font-weight-bold text-gray-800">Loading...</div>
                        <div v-else class="h5 mb-0 font-weight-bold text-gray-800">{{ totalOutput }}</div>
                </div>
                <div class="col-auto">
                    <i class="fas fa-wallet fa-2x text-gray-300"></i>
                    
                </div>
            </div>
        </div>
    </div>
</template>

<script setup>
import { ref, watch, watchEffect, computed } from 'vue';
import { monthlyOutput} from '../../service/monthlySummary';
import { useBudgetStore } from '@/stores/budgetStore.js';

const budgetStore = useBudgetStore();
const totalOutput = ref(0);
const isLoading = ref(true);

const month = Number((new Date()).getMonth()) + 1;

// 이용자의 당월 소득 바인딩
const updateTotalOutput = () => {
  totalOutput.value = monthlyOutput();
  isLoading.value = false;
};

// 빠른 등록으로 추가되는 경우, 카드 값 업데이트
watch(() => budgetStore.transactions, () => {
  updateTotalOutput();
}, {deep:true});

updateTotalOutput();
</script>

