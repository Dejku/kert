<template>
  <div class="relative-position column gap-sm text-weight-300 text-size-7">
    <div class="row justify-between">
      <div class="row items-center gap-xs">
        <q-icon :name="iconsStore.icons.calendar" class="text-size-10" />
        Dzień
      </div>
      <span>{{ formattedDate }}</span>
    </div>

    <div class="row justify-between" style="height: 40px">
      <div class="row items-center gap-xs">
        <q-icon :name="iconsStore.icons.text" class="text-size-10" />
        Typ
      </div>
      <q-select
        v-model="modalStore.component.vacationDays"
        :options="[Normal, OnDemand, Force]"
        option-label="name"
        color="primary"
        bg-color="surfaceVariant"
        dense
        standout
        options-dark
        behavior="menu"
        class="rounded-borders shadow"
        style="width: max(55%, 190px)"
      />
    </div>

    <div class="row justify-between">
      <div class="row items-center gap-xs">
        <q-icon :name="iconsStore.icons.clock" class="text-size-10" />
        Czas trwania
      </div>
      <div>
        <Transition name="fade" mode="out-in">
          <span v-if="modalStore.component.vacationDays.time.type == 'day'">
            Cały dzień
          </span>

          <div v-else class="row gap-sm">
            <q-icon
              :name="iconsStore.icons.arrowLeft"
              class="text-size-10"
              v-touch-repeat:0:100.mouse="decrement"
            />
            <span>{{ modalStore.component.vacationDays.time.hours }}</span>
            <q-icon
              :name="iconsStore.icons.arrowRight"
              class="text-size-10"
              v-touch-repeat:0:100.mouse="increment"
            />
          </div>
        </Transition>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { Normal, OnDemand, Force } from 'components/models';
import { Months } from '../utils';

import { useIconsStore } from 'stores/iconsStore';
import { useModalStore } from 'stores/modalStore';

import { ref, watch } from 'vue';

const iconsStore = useIconsStore();
const modalStore = useModalStore();

const formattedDate = ref<string>('Brak tytułu');

if (modalStore.modal.component.options.date) {
  formattedDate.value = `${modalStore.modal.component.options.date.day} ${
    Months[modalStore.modal.component.options.date.month - 1].formattedName
  } ${modalStore.modal.component.options.date.year}`;
}

const decrement = () => (modalStore.component.vacationDays.time.hours -= 1);
const increment = () => (modalStore.component.vacationDays.time.hours += 1);

watch(
  () => modalStore.component.vacationDays.time.hours,
  () => {
    modalStore.component.vacationDays.time.hours =
      modalStore.component.vacationDays.time.hours < 1
        ? 1
        : modalStore.component.vacationDays.time.hours;
    modalStore.component.vacationDays.time.hours =
      modalStore.component.vacationDays.time.hours > 8
        ? 8
        : modalStore.component.vacationDays.time.hours;
  }
);
</script>
