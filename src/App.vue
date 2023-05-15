<script setup lang="ts">
import {onBeforeUnmount, onMounted, ref} from 'vue'

const showInstallButton = ref(false);
let deferredPrompt: any = null;

const beforeInstallPromptHandler = (event: Event) => {
    event.preventDefault();
    deferredPrompt = event;
    showInstallButton.value = true;
};

const installPWA = () => {
    if (deferredPrompt) {
        deferredPrompt.prompt();
        deferredPrompt.userChoice.then((choiceResult: { outcome: string }) => {
            if (choiceResult.outcome === 'accepted') {
                console.log('User accepted the install prompt');
            } else {
                console.log('User dismissed the install prompt');
            }
            deferredPrompt = null;
            showInstallButton.value = false;
        });
    }
};

onMounted(() => {
    window.addEventListener('beforeinstallprompt', beforeInstallPromptHandler);
});

onBeforeUnmount(() => {
    window.removeEventListener('beforeinstallprompt', beforeInstallPromptHandler);
});
</script>

<template>
    <div class="flex flex-col justify-center items-center min-h-screen bg-hero-pattern">
        <div class="bg-opacity-60"></div>

        <!--    Le bouton pourrait déclencher une animation pour montrer à l'utilisateur comment installer la PWA une fois le bouton cliqué    -->
        <div class="text-center text-neutral-content mb-20">
            <div class="max-w-2xl">
                <h1 class="mb-2 text-5xl font-semibold">Bienvenue sur une</h1>
                <h1 class="mb-5 text-6xl font-bold text-purple-500">Progressive Web App</h1>
                <p class="mb-5">J'étends les capacités de votre navigateur en vous offrant des fonctionnalités qu'il ne
                    pouvait qu'espérer atteindre. Pour commencer, il vous suffit de m'installer sur votre appareil.</p>
                <button class="btn btn-primary text-white bg-purple-500 rounded-full px-7 py-3" @click="installPWA">
                    Installez moi !
                </button>
            </div>
        </div>

        <!--    La jauge devrait évoluer si l'utilisateur lance l'application installée et non pas la version web,
        avec un petit son joué afin de lui faire comprendre que tout fonctionne bien -->
        <div>
            <ol class="items-center sm:flex">
                <li class="relative mb-6 sm:mb-0">
                    <div class="flex items-center">
                        <div class="z-10 flex items-center justify-center w-6 h-6 bg-red-500 rounded-full ring-0 ring-white dark:bg-blue-900 sm:ring-8 dark:ring-gray-900 shrink-0"></div>
                        <div class="hidden sm:flex w-full bg-gray-200 h-0.5 dark:bg-gray-700"></div>
                    </div>
                    <div class="mt-3 sm:pr-8">
                        <h3 class="text-lg font-semibold text-gray-900 dark:text-white">Vous n'avez pas installé
                            l'application...</h3>
                        <p class="text-base font-normal text-gray-500 dark:text-gray-400">Prenez le temps d'installer
                            l'application pour pouvoir l'utiliser pleinement.</p>
                    </div>
                </li>
                <li class="relative mb-6 sm:mb-0">
                    <div class="flex items-center">
                        <div class="z-10 flex items-center justify-center w-6 h-6 bg-green-100 rounded-full ring-0 ring-white dark:bg-blue-900 sm:ring-8 dark:ring-gray-900 shrink-0"></div>
                    </div>
                    <div class="mt-3 sm:pr-8">
                        <h3 class="text-lg font-semibold text-gray-900 dark:text-white">Vous avez bien installé
                            l'application !</h3>
                        <p class="text-base font-normal text-gray-500 dark:text-gray-400">Bravo ! Vous pouvez désormais
                            découvrir les joies de la PWA !</p>
                    </div>
                </li>
            </ol>
        </div>
    </div>
</template>
