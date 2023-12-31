<template>
  <q-page class="column gap-md">
    <h6 class="main-title">Ustawienia</h6>

    <section
      class="column q-px-md q-py-sm bg-surface rounded-borders gap-m shadow"
    >
      <router-link to="/settings/account" class="row items-center">
        <div class="row gap-sm">
          <div>
            <q-avatar size="15vw">
              <img :src="accountStore.getAvatar" />
            </q-avatar>
          </div>

          <div>
            <h6
              class="no-margin text-weight-600"
              style="margin-bottom: -5px !important"
            >
              {{ accountStore.nickName }}
            </h6>
            <span class="text-size-4" style="opacity: 0.75">Twoje konto</span>
          </div>
        </div>

        <q-space />

        <q-icon
          :name="iconsStore.icons.arrowRight"
          class="text-size-7 text-tertiary"
        />
      </router-link>
    </section>

    <section
      v-for="(section, index) in sections"
      :key="index"
      class="column q-px-md q-py-sm bg-surface rounded-borders gap-md shadow"
    >
      <div
        v-for="element in section"
        :key="element.label"
        @click="element.click"
        class="row items-center gap-sm"
      >
        <q-icon :name="element.icon" class="text-size-10" />

        <span>{{ element.label }}</span>

        <q-space />

        <q-icon
          :name="iconsStore.icons.arrowRight"
          class="text-size-7 text-tertiary"
        />
      </div>
    </section>
  </q-page>
</template>

<script setup lang="ts">
import { DialogOption } from 'components/models';

import { useIconsStore } from 'stores/iconsStore';
import { useAccountStore } from 'stores/accountStore';
import { useDialogStore } from 'stores/dialogStore';
import { useRouter } from 'vue-router';

const iconsStore = useIconsStore();
const accountStore = useAccountStore();
const dialogStore = useDialogStore();
const router = useRouter();

const sections = {
  links: [
    {
      icon: iconsStore.icons.bell,
      label: 'Powiadomienia',
      click: function () {
        router.push('/settings/notifications');
      },
    },
    {
      icon: iconsStore.icons.calendar,
      label: 'Twoja praca',
      click: function () {
        router.push('/settings/work');
      },
    },
    {
      icon: iconsStore.icons.colorPalette,
      label: 'Kolory aplikacji',
      click: function () {
        router.push('/settings/themes');
      },
    },
  ],
  misc: [
    {
      icon: iconsStore.icons.questionMark,
      label: 'Centrum Pomocy',
      click: function () {
        router.push('/help');
      },
    },
    {
      icon: iconsStore.icons.logout,
      label: 'Wyloguj się',
      click: function () {
        logout();
      },
    },
  ],
};

const dialogOption: DialogOption = {
  title: 'Czy na pewno chcesz się wylogować?',
  buttonsOptions: {
    baseButton: {
      label: 'Nie',
      transparent: true,
    },
    extendedButton: {
      label: 'Tak',
      color: 'error',
    },
  },
};

const logout = async () => {
  const response = await dialogStore.showDialog(dialogOption);

  console.log(response);
};
</script>
