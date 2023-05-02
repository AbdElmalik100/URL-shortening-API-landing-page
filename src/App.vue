<template>
  <main class="min-vh-100 overflow-hidden">
    <div class="container">
      <HeaderNav></HeaderNav>
      <div class="landing py-5 d-flex align-items-center gap-5">
        <div class="info flex-fill w-50">
          <h1 class="fw-bold mb-0">More than just shorter links</h1>
          <p class="mt-1 mb-4">
            More than just shorter links Build your brand’s recognition and get detailed insights on how your links are
            performing.
          </p>
          <button class="butn">Get Started</button>
        </div>
        <div class="image-cont position-relative">
          <img src="/images/illustration-working.svg" alt="">
        </div>
      </div>
    </div>
    <div class="stats py-5">
      <div class="container">
        <div class="form-cont d-flex flex-column gap-3 position-relative">
          <form :class="`shoter-url rounded p-5 d-flex gap-3 ${validation.$error ? 'error' : ''}`"
            @submit.prevent="shortenLink">
            <div :class="`input-cont position-relative flex-fill ${validation.$error ? 'error' : ''}`">
              <input type="text" class="rounded p-2 px-3 w-100" placeholder="Shorten a link here ..."
                v-model="shortenInput">
              <span v-if="validation.$error" class="error d-block position-absolute w-100 fst-italic">
                {{ validation.$errors[0].$validator === 'required' ? 'Field can\'t be empty' : 'Please enter a link' }}
              </span>
            </div>
            <button class="butn rounded ">Shorten It!</button>
          </form>
          <div class="shorten-links d-flex align-items-center gap-3 flex-column" v-if="shortLinks.length > 0">
            <div class="box p-3 px-4 rounded bg-white d-flex align-items-center gap-4 justify-content-between w-100"
              v-for="(result, index) in shortLinks" :key="index">
              <span class="the-link w-75 text-break">{{ result.original_link }}</span>
              <div class="d-flex align-items-center gap-3">
                <a class="shortend-link text-decoration-none" :href="result.full_short_link2" target="_blank">
                  {{ result.full_short_link2 }}
                </a>
                <button
                  :class="`butn rounded overflow-hidden position-relative ${buttonIndex[index].copied ? 'copied' : ''}`"
                  @click="copyLink(result.full_short_link2, index)">
                  <transition name="slide-up" mode="out-in">
                    <span class="co d-block" v-if="!buttonIndex[index].copied">Copy</span>
                    <span class="co d-block" v-else>Copied!</span>
                  </transition>
                </button>
              </div>
            </div>
          </div>
        </div>
        <div class="heading text-center w-50 mx-auto mb-5 pb-5">
          <h1 class="fw-bold mb-4">Advanced Statistics</h1>
          <p class="mb-0">
            Track how your links are performing across the web with our advanced statistics dashboard.
          </p>
        </div>
        <div class="cards d-flex gap-4 py-5 position-relative">
          <div class="box p-4 pt-0 rounded bg-white position-relative">
            <div class="icon-cont rounded-circle d-flex align-items-center justify-content-center position-relative">
              <img src="/images/icon-brand-recognition.svg" alt="">
            </div>
            <h3 class="mb-3 fw-bold">Brand Recognition</h3>
            <p class="mb-0">
              Boost your brand recognition with each click. Generic links don’t mean a thing. Branded links help instil
              confidence in your content.
            </p>
          </div>
          <div class="box p-4 pt-0 rounded bg-white position-relative">
            <div class="icon-cont rounded-circle d-flex align-items-center justify-content-center position-relative">
              <img src="/images/icon-detailed-records.svg" alt="">
            </div>
            <h3 class="mb-3 fw-bold">Detailed Records</h3>
            <p class="mb-0">
              Gain insights into who is clicking your links. Knowing when and where people engage with your content helps
              inform better decisions.
            </p>
          </div>
          <div class="box p-4 pt-0 rounded bg-white position-relative">
            <div class="icon-cont rounded-circle d-flex align-items-center justify-content-center position-relative">
              <img src="/images/icon-fully-customizable.svg" alt="">
            </div>
            <h3 class="mb-3 fw-bold">Fully Customizable</h3>
            <p class="mb-0">
              Improve brand awareness and content discoverability through customizable links, supercharging audience
              engagement.
            </p>
          </div>
        </div>
      </div>
    </div>
    <div class="boost py-5">
      <div class="container text-center">
        <h1 class="fw-bold mb-4 text-light">Boost your links today</h1>
        <button class="butn">Get Started</button>
      </div>
    </div>
    <FooterComp></FooterComp>
  </main>
</template>
<script setup>
import FooterComp from './components/FooterComp.vue';
import HeaderNav from './components/HeaderNav.vue';
import { useVuelidate } from '@vuelidate/core'
import { required, url } from '@vuelidate/validators'
import { ref } from 'vue';




const shortenInput = ref('')
const shortLinks = ref([])
const buttonIndex = ref([])

const validation = useVuelidate({ required, url }, shortenInput)


const shortenLink = async () => {
  validation.value.$validate()
  if (!validation.value.$error) {
    await fetch(`https://api.shrtco.de/v2/shorten?url=${shortenInput.value}`)
      .then(response => response.json())
      .then(data => {
        if (data.ok) {
          shortLinks.value.push(data.result)
          buttonIndex.value.push({ index: shortLinks.value.indexOf(data.result), copied: false })
        }
      })
  }
}

const copyLink = async (link, index) => {
  // await navigator.clipboard.writeText(link)        (Not Work On Mobile Chrome)

  const dummy = document.createElement("input");      // (A Way To Copy To Clipboard But It's Deprecated, But It Works On Mobile Chrome)
  document.body.appendChild(dummy);
  dummy.setAttribute('value', link);
  dummy.select();
  document.execCommand("copy");
  document.body.removeChild(dummy);

  buttonIndex.value.forEach(el => {
    if (el.index === index) {
      el.copied = true
      setTimeout(() => {
        el.copied = false
      }, 1500)
    }
  })
}

</script>
<style lang="scss">
* {
  box-sizing: border-box;
}

// Animation For Transition
.slide-up-enter-active,
.slide-up-leave-active {
  transition: all 0.2s ease-out;
}

.slide-up-enter-from {
  opacity: 0;
  transform: translateY(30px);
}

.slide-up-leave-to {
  opacity: 0;
  transform: translateY(-30px);
}


:root {
  // ### Primary
  --Cyan: hsl(180, 66%, 49%);
  --Dark-Violet: hsl(257, 27%, 26%);

  // ### Secondary
  --Red: hsl(0, 87%, 67%);

  // ### Neutral
  --Gray: hsl(0, 0%, 75%);
  --Grayish-Violet: hsl(257, 7%, 63%);
  --Very-Dark-Blue: hsl(255, 11%, 22%);
  --Very-Dark-Violet: hsl(260, 8%, 14%);
}

body {
  font-size: 18px !important;
  font-family: 'Poppins', sans-serif;
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
}

button {
  background: none;
  outline: none;
  border: none;
  transition: 0.2s;
}

.butn {
  background-color: var(--Cyan);
  color: white;
  border-radius: 50px;
  padding: 10px 30px;

  &:hover {
    opacity: 0.6;
  }
}

main {
  .landing {
    .info {

      h1 {
        color: var(--Very-Dark-Blue);
        font-size: 50px;
      }

      p {
        color: var(--Grayish-Violet);
      }
    }

    .image-cont {

      right: -180px;

      img {
        width: 100%;
      }
    }
  }

  .stats {
    background-color: #EFF1F5;
    margin-top: 150px;

    .form-cont {
      top: -119px;

      .shoter-url {
        background-color: var(--Dark-Violet);
        background-image: url(/images/bg-shorten-desktop.svg);

        .input-cont {
          input {
            outline: none;
            transition: 0.2s;
            border: 2px solid transparent;
          }

          .error {
            color: var(--Red);
            bottom: -25px;
            font-size: 13px;
          }

          &.error {
            input {
              border: 2px solid var(--Red);
            }
          }
        }

        button {
          &:hover {
            opacity: 1;
            background-color: hsl(180, 57%, 67%);
          }
        }
      }

      .shorten-links {
        .the-link {
          color: var(--Dark-Violet);
        }

        .shortend-link {
          color: var(--Cyan);

          &:hover {
            text-decoration: underline !important;
          }
        }

        button {
          width: 115px;
          padding: 10px 0;
          z-index: 1;
          transition: 0.2s;

          &.copied {
            background-color: var(--Dark-Violet);

            &:hover {
              opacity: 1;
            }
          }

          span {
            pointer-events: none;
          }

        }
      }
    }

    .heading {
      h1 {
        color: var(--Very-Dark-Violet);
      }

      p {
        color: var(--Grayish-Violet);
      }
    }

    .cards {
      &::before {
        content: '';
        position: absolute;
        background-color: var(--Cyan);
        width: 100%;
        height: 7px;
        top: 50%;
        transform: translateY(-50%);
      }

      .box:first-child {
        transform: translateY(-50px);
      }

      .box:last-child {
        transform: translateY(50px);
      }

      .icon-cont {
        background-color: var(--Very-Dark-Blue);
        width: 90px;
        height: 90px;
        top: -45px;
      }

      h3 {
        color: var(--Very-Dark-Violet);
        font-size: 20px;
      }

      p {
        color: var(--Grayish-Violet);
        font-size: 15px;
      }
    }
  }

  .boost {
    background-color: var(--Dark-Violet);
    background-image: url(/images/bg-boost-desktop.svg);
    background-size: cover;

    button {
      &:hover {
        opacity: 1;
        filter: brightness(1.2);
      }
    }
  }

  @media (max-width: 767px) {
    .landing {
      flex-direction: column-reverse;

      .image-cont {
        right: -10px;

        img {
          width: 125%;
        }
      }

      .info {
        width: 100% !important;
        text-align: center;

        h1 {
          font-size: 40px;
          margin-bottom: 15px !important;
        }
      }
    }

    .stats {
      .form-cont {
        .shoter-url {
          flex-direction: column;
          padding: 25px !important;
          background-image: url(/src/assets/images/bg-shorten-mobile.svg);
          background-size: cover;
          background-repeat: no-repeat;
          background-position: 100px -40px;
        }

        span.error {
          bottom: initial;
          top: 55px;
        }

        form.error {
          height: 190px;
        }
      }

      .shorten-links {
        .box {
          flex-direction: column;

          .the-link {
            width: 100% !important;
            border-bottom: 1px solid var(--Gray);
            padding-bottom: 24px;
          }

          &>div {
            flex-direction: column;
            align-items: flex-start !important;
            width: 100%;

            button {
              width: 100% !important;

              &:hover {
                opacity: 1;
              }
            }
          }
        }
      }

      .heading {
        width: 100% !important;
      }

      .cards {
        flex-direction: column;
        text-align: center;

        &::before {
          left: 50%;
          transform: translateX(-50%);
          height: 100%;
          width: 7px;
          top: 0;

        }

        .icon-cont {
          left: 50%;
          transform: translateX(-50%);
        }
      }
    }

    .boost {
      background-image: url(/images/bg-boost-mobile.svg);
      padding: 75px 0 !important;
    }
  }
}
</style>