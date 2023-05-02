<template >
    <header class="py-5 d-flex justify-content-between align-items-center gap-5">
        <img src="/images/logo.svg" alt="">
        <div
            :class="`links-cont flex-fill d-flex align-items-center justify-content-between ${openMenu ? 'active-menu' : ''}`">
            <ul class="links d-flex align-items-center gap-4">
                <li class="fw-bold">Features</li>
                <li class="fw-bold">Pricing</li>
                <li class="fw-bold">Resources</li>
            </ul>
            <div class="start-butns d-flex align-items-center gap-4">
                <button class="bg-none border-0 fw-bold">Login</button>
                <button class="butn fw-bold">Signup</button>
            </div>
        </div>
        <i @click="openMenu = !openMenu" class="menu fa-solid fa-bars fa-xl"></i>
    </header>
</template>
<script setup>
import { ref } from 'vue';

const openMenu = ref(false)

window.addEventListener('click', (e) => {
    if (e.target.classList[0] !== 'links-cont' && e.target.classList[0] !== 'menu') {
        openMenu.value = false
    }
})

</script>
<style lang="scss" scoped>
header {
    .menu {
        display: none;
    }

    .links-cont {
        .links {
            li {
                cursor: pointer;
                transition: 0.2s;
                color: var(--Gray);

                &:hover {
                    color: var(--Very-Dark-Violet);
                }
            }
        }

        .start-butns {
            button:first-child {
                color: var(--Gray);

                &:hover {
                    color: var(--Very-Dark-Violet);
                }
            }
        }
    }

    @media (max-width: 767px) {
        .menu {
            display: block;
            color: var(--Gray);
        }

        .links-cont {
            flex-direction: column;
            position: absolute;
            gap: 50px;
            z-index: 5555;
            background-color: var(--Dark-Violet);
            width: calc(100% - 24px);
            padding: 40px 25px;
            left: 50%;
            top: -100%;
            transform: translateX(-50%);
            border-radius: 10px;
            transition: 0.2s ease-in-out;

            &.active-menu {
                top: 20%;
            }

            .links {
                flex-direction: column;
                width: 100%;
                padding-bottom: 50px;
                border-bottom: 1px solid var(--Gray);

                li {
                    color: white;
                }
            }

            .start-butns {
                flex-direction: column;
                width: 100%;

                button {
                    width: 100%;

                    &:first-child {
                        color: white;
                    }
                }
            }
        }

    }
}
</style>