<template>
    <section class="box-sort" :style="`background: ${options.color}`">
        <div>
            <v-tooltip :text="textTitleSort">
                <template v-slot:activator="{ props }">
                    <v-btn
                            v-bind="props"
                            :icon="iconSort"
                            density="compact"
                            variant="text"
                            color="blue"
                            @click="sortList(options.id,null)"/>
                </template>
            </v-tooltip>
        </div>
        <div>
            <v-tooltip text="Без сортировки">
                <template v-slot:activator="{ props }">
                    <v-btn
                            v-bind="props"
                            icon="mdi-sort"
                            density="compact"
                            variant="text"
                            color="blue"
                            @click="sortList(options.id,0)"/>
                </template>
            </v-tooltip>
            <v-tooltip text="По убыванию">
                <template v-slot:activator="{ props }">
                    <v-btn
                            v-bind="props"
                            icon="mdi-sort-descending"
                            density="compact"
                            variant="text"
                            color="blue"
                            @click="sortList(options.id,1)"/>
                </template>
            </v-tooltip>
            <v-tooltip text="По возростанию">
                <template v-slot:activator="{ props }">
                    <v-btn
                            v-bind="props"
                            icon="mdi-sort-ascending"
                            density="compact"
                            variant="text"
                            color="blue"
                            @click="sortList(options.id,2)"/>
                </template>
            </v-tooltip>
        </div>
    </section>
</template>

<script setup>
    import {inject, ref, computed} from "vue";

    const props = defineProps({
        options: {
            type: Object,
            default: {}
        },
    });

    const firstList = inject('firstList');
    const secondList = inject('secondList');
    const lastList = inject('lastList');

    const currentStateIndex = ref(0);

    const iconSort = computed(() => {
        if (currentStateIndex.value === 0) {
            return 'mdi-sort'
        }
        if (currentStateIndex.value === 1) {
            return 'mdi-sort-descending'
        }
        if (currentStateIndex.value === 2) {
            return 'mdi-sort-ascending'
        }
    });
    const textTitleSort = computed(() => {
        if (currentStateIndex.value === 0) {
            return 'Без сортировки'
        }
        if (currentStateIndex.value === 1) {
            return 'По убыванию'
        }
        if (currentStateIndex.value === 2) {
            return 'По возростанию'
        }
    });

    const sortList = (id, toggleClick) => {
        let sortedArr = []
        if (id === 1) {
            sortedArr = firstList.value
        }
        if (id === 2) {
            sortedArr = secondList.value
        }
        if (id === 3) {
            sortedArr = lastList.value
        }

        if (toggleClick === null) {
            currentStateIndex.value = currentStateIndex.value < 2 ? currentStateIndex.value + 1 : 0
        } else {
            currentStateIndex.value = toggleClick
        }

        if (currentStateIndex.value === 0) {
            sortedArr = sortedArr.sort((a, b) => a.id - b.id)
        }

        if (currentStateIndex.value === 1) {
            sortedArr = sortedArr.sort((a, b) => b.rating.rate - a.rating.rate)
        }

        if (currentStateIndex.value === 2) {
            sortedArr = sortedArr.sort((a, b) => a.rating.rate - b.rating.rate)
        }

        if (id === 1) {
            firstList.value = sortedArr
        }
        if (id === 2) {
            secondList.value = sortedArr
        }
        if (id === 3) {
            lastList.value = sortedArr
        }
    }
</script>

<style lang="scss" scoped>
    .box-sort {
        height: 35px;
        display: flex;
        align-items: center;
        justify-content: space-around;
        margin-bottom: 10px;
        border-radius: 5px;
    }
</style>
