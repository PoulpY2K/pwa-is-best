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
        <div class="text-center text-neutral-content">
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
    </div>
</template>
