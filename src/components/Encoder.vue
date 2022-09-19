<script setup>
  import { ref } from 'vue'
  import { useClipboard } from '@vueuse/core'

  const inputText = ref('')
  const outputText = ref('')

  const replacements = {
    ه: 'ہ',
    ء: 'ء',
    ا: 'ا',
    ﺎ: 'ا',
    آ: 'آ',
    ﺑ: 'ب',
    ب: 'ب',
    ﭘ: 'پ',
    پ: 'پ',
    ت: 'ت',
    ﺗ: 'ت',
    ﭨ: 'ٹ',
    ٹ: 'ٹ',
    ﺛ: 'ث',
    ث: 'ث',
    ﺟ: 'ج',
    ج: 'ج',
    ﺞ: 'ج',
    ﭼ: 'چ',
    ﭻ: 'چ',
    ﺣ: 'ح',
    ﺢ: 'ح',
    ح: 'ح',
    ﺧ: 'خ',
    ﺦ: 'خ',
    خ: 'خ',
    د: 'د',
    ڈ: 'ڈ',
    ذ: 'ذ',
    ر: 'ر',
    ڑ: 'ڑ',
    ز: 'ز',
    ﺳ: 'س',
    س: 'س',
    ﺷ: 'ش',
    ش: 'ش',
    ﺻ: 'ص',
    ص: 'ص',
    ﺿ: 'ض',
    ض: 'ض',
    ط: 'ط',
    ظ: 'ظ',
    ﻌ: 'ع',
    ع: 'ع',
    ﻊ: 'ع',
    ﻋ: 'ع',
    ﻏ: 'غ',
    ﻐ: 'غ',
    ﻎ: 'غ',
    ﻓ: 'ف',
    ﻔ: 'ف',
    ف: 'ف',
    ﻗ: 'ق',
    ق: 'ق',
    ﻘ: 'ق',
    ﮏ: 'ک',
    ﮐ: 'ک',
    ﮑ: 'ک',
    ک: 'ک',
    ﮓ: 'گ',
    ﮔ: 'گ',
    ﮕ: 'گ',
    گ: 'گ',
    ل: 'ل',
    ﻠ: 'ل',
    ﻟ: 'ل',
    ﻻ: 'لا',
    ﻼ: 'لا',
    م: 'م',
    ﻣ: 'م',
    ں: 'ں',
    ن: 'ن',
    ﻧ: 'ن',
    ه: 'ھ',
    ﮨ: 'ہ',
    ﮩ: 'ہ',
    ﮭ: 'ھ',
    ھ: 'ھ',
    و: 'و',
    ے: 'ے',
    ﮯ: 'ے',
    ی: 'ی',
    ﯾ: 'ی',
    ﯽ: 'ی',
    ﺋ: 'ئ',
    ه: 'ہ'
  }

  const { copy, copied, isSupported } = useClipboard({ outputText })

  const exampleStory = 'اس ﺳﺎﺋٹ ﮐﮯ ﺑﺎرے ﻣﯾں'

  function encodeText(text) {
    let storyText = Array.from(text)
    const revisedText = storyText.map((character) => {
      if (replacements[character] != null) {
        return replacements[character]
      }

      return character
    })

    return revisedText.join('')
  }

  function handleSubmitClick(text) {
    outputText.value = ''
    outputText.value = encodeText(text)
  }

  function handleExampleClick() {
    inputText.value = exampleStory
  }

  function handleClear() {
    inputText.value = ''
    outputText.value = ''
  }
</script>

<template>
  <form>
    <label class="input-label">Text Input:</label>
    <textarea
      id="inputText"
      v-model="inputText"
      class="text-input"
      autocomplete="off"
      dir="rtl"
    ></textarea>
    <button class="form-button" @click.prevent="handleSubmitClick(inputText)">
      Convert Text
    </button>
    <button class="form-button" @click.prevent="handleExampleClick()">
      Load Example Text
    </button>
    <button class="form-button" @click.prevent="handleClear()">Clear</button>
  </form>
  <h2 class="input-label">Encoded Text:</h2>
  <p class="text-output" readonly dir="rtl">{{ outputText }}</p>
  <button v-if="isSupported" class="form-button" @click="copy(outputText)">
    <span v-if="!copied">Copy Text</span>
    <span v-else>Copied!</span>
  </button>
</template>

<style scoped>
  .input-label {
    font-size: 1.2rem;
    font-weight: 700;
    margin-bottom: 1rem;
    display: block;
  }

  .text-input {
    width: 100%;
    height: auto;
    min-height: 33vh;
    font-family: 'Cairo', 'Arial', 'Segoe UI', Tahoma, Geneva, Verdana,
      sans-serif;
    margin-bottom: 1rem;
    border: #24292e;
    border-style: ridge;
    border-width: 1px;
    border-radius: 0.3rem;
  }

  .text-input:focus {
    border-color: rgba(137, 32, 236, 0.15);
    border-style: outset;
    border-width: 1px;
  }

  .text-output {
    height: auto;
    min-height: 33vh;
    border-width: 1px;
    font-family: 'Noto Nastaliq Urdu', 'Jameel Noori Nastaleeq',
      'Alvi Nastaleeq', 'Pak Nastaleeq', 'Nafees Web Naskh',
      'Urdu Naskh Asiatype', 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    padding: 1rem;
    background-color: #fefeff;
    border: #24292e;
    border-style: ridge;
    border-width: 1px;
    border-radius: 0.3rem;
  }

  .form-button {
    appearance: none;
    background-color: #fafbfc;
    border: 1px solid rgba(27, 31, 35, 0.15);
    border-radius: 6px;
    box-shadow: rgba(27, 31, 35, 0.04) 0 1px 0,
      rgba(255, 255, 255, 0.25) 0 1px 0 inset;
    box-sizing: border-box;
    color: #24292e;
    cursor: pointer;
    display: inline-block;
    font-family: -apple-system, system-ui, 'Segoe UI', Helvetica, Arial,
      sans-serif, 'Apple Color Emoji', 'Segoe UI Emoji';
    font-size: 14px;
    font-weight: 500;
    line-height: 20px;
    list-style: none;
    margin-right: 16px;
    padding: 6px 16px;
    position: relative;
    transition: background-color 0.2s cubic-bezier(0.3, 0, 0.5, 1);
    user-select: none;
    -webkit-user-select: none;
    touch-action: manipulation;
    vertical-align: middle;
    white-space: nowrap;
    word-wrap: break-word;
  }

  .form-button:hover {
    background-color: #f3f4f6;
    text-decoration: none;
    transition-duration: 0.1s;
  }

  .form-button:disabled {
    background-color: #fafbfc;
    border-color: rgba(27, 31, 35, 0.15);
    color: #959da5;
    cursor: default;
  }

  .form-button:active {
    background-color: #edeff2;
    box-shadow: rgba(225, 228, 232, 0.2) 0 1px 0 inset;
    transition: none 0s;
  }

  .form-button:focus {
    outline: 1px transparent;
  }

  .form-button:before {
    display: none;
  }

  .form-button:-webkit-details-marker {
    display: none;
  }
</style>
