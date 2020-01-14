<template>
  <div class="fab">
    <span class="fab-action-button" @click="showToolbar = !showToolbar">
      <human class="fab-action-button-icon" />
    </span>
    <transition name="fade">
      <div class='accessibility-toolbar' v-if="showToolbar">
        <ul class="accessibility-items">
          <li class="accessibility-item">
            <a class="accessibility-menu-item" :data-tooltip="invertColorsText" :class="{ active: accessibilityStates.inverted }" @click="toggleState('inverted')" :aria-label="invertColorsText">
              <invert-colors class="accessibility-menu-item-icon" />
            </a>
          </li>
          <li>
            <a class="accessibility-menu-item" :data-tooltip="highlightLinksText" :class="{ active: accessibilityStates.highlighted }" @click="toggleState('highlighted')" :aria-label="highlightLinksText">
              <flashlight class="accessibility-menu-item-icon" />
            </a>
          </li>
          <li>
            <a class="accessibility-menu-item" :data-tooltip="grayscaleText" :class="{ active: accessibilityStates.greyscaled }" @click="toggleState('greyscaled')" :aria-label="grayscaleText">
              <water-off class="accessibility-menu-item-icon" />
            </a>
          </li>
          <li>
            <a class="accessibility-menu-item" :data-tooltip="accessibileFontSizeText" :class="{ active: accessibilityStates.accessibileFontSize }" @click="toggleState('accessibileFontSize')" :aria-label="accessibileFontSizeText">
              <format-font-size-increase class="accessibility-menu-item-icon" />
            </a>
          </li>
          <li>
            <a class="accessibility-menu-item" :data-tooltip="dyslexicFontText" :class="{ active: accessibilityStates.dyslexicFont }" @click="toggleState('dyslexicFont')" :aria-label="dyslexicFontText">
              <eye-outline class="accessibility-menu-item-icon" />
            </a>
          </li><!--
          <li>
            <a class="accessibility-menu-item" :data-tooltip="textToSpeechText" :class="{ active: accessibilityStates.dyslexicFont }" @click="toggleState('dyslexicFont')" :aria-label="dyslexicFontText">
              <text-to-speech class="accessibility-menu-item-icon" />
            </a>
          </li>
          <li>
            <a class="accessibility-menu-item" :data-tooltip="pdfText" :class="{ active: accessibilityStates.dyslexicFont }" @click="toggleState('dyslexicFont')" :aria-label="dyslexicFontText">
              <pdf-box class="accessibility-menu-item-icon" />
            </a>
          </li>
          <li>
            <a class="accessibility-menu-item" :data-tooltip="cursorText" :class="{ active: accessibilityStates.dyslexicFont }" @click="toggleState('dyslexicFont')" :aria-label="dyslexicFontText">
              <cursor-default class="accessibility-menu-item-icon" />
            </a>
          </li> -->
        </ul>
      </div>
    </transition>
  </div>
</template>

<script>
//import TextToSpeech from 'mdi-vue/TextToSpeech'
import FormatFontSizeIncrease from 'mdi-vue/FormatFontSizeIncrease'
import WaterOff from 'mdi-vue/WaterOff'
import Flashlight from 'mdi-vue/Flashlight'
import InvertColors from 'mdi-vue/InvertColors'
import Human from 'mdi-vue/Human'
import EyeOutline from 'mdi-vue/EyeOutline'
//import PdfBox from 'mdi-vue/PdfBox'
//import CursorDefault from 'mdi-vue/CursorDefault'

export default {
  name: 'AccessibilityToolbar',
  components: {
    //TextToSpeech,
    FormatFontSizeIncrease,
    WaterOff,
    Flashlight,
    InvertColors,
    Human,
    EyeOutline,
    //PdfBox,
    //CursorDefault
  },
  props: {
    invertColorsText: {
      type: String,
      default: "Invert Colors"
    },
    highlightLinksText: {
      type: String,
      default: "Highlight Links"
    },
    grayscaleText: {
      type: String,
      default: "Desaturate"
    },
    accessibileFontSizeText: {
      type: String,
      default: "Increase Text Size"
    },
    dyslexicFontText: {
      type: String,
      default: "OpenDyslexic Font"
    },
    textToSpeechText: {
      type: String,
      default: "Read Selection Aloud"
    },
    pdfText: {
      type: String,
      default: "PDF Text Alternative"
    },
    cursorText: {
      type: String,
      default: "Enlarge the Mouse Cursor"    
    }
  },
  data() {
    return {
      accessibilityStates: {
        inverted: false,
        highlighted: false,
        accessibileFontSize: false,
        greyscaled: false,
        dyslexicFont: false
      },
      showToolbar: false,
    }
  },
  methods: {
    toggleState(state) {
      this.accessibilityStates[state] = !this.accessibilityStates[state]
      this.applyState(state)
    },
    applyState(state) {
      if (state === "inverted") {
        this.resetHighlightLinks()
        this.resetGrayscale()
        this.accessibilityStates[state] ?
          document.body.classList.add("accessibility-contrast") :
          document.body.classList.remove("accessibility-contrast")
      } else if (state === "greyscaled") {
        this.resetHighlightLinks()
        this.resetInvertContrast()
        this.accessibilityStates[state] ?
          document.body.classList.add("accessibility-greyscale") :
          document.body.classList.remove("accessibility-greyscale")
      } else if (state === "highlighted") {
        this.resetGrayscale()
        this.resetInvertContrast()
        this.hightlightLinks()
      } else if (state === "accessibileFontSize") {
        this.accessibilityStates[state] ?
          document.body.classList.add("accessibility-font") :
          document.body.classList.remove("accessibility-font")
      } else if (state === "dyslexicFont") {
        this.accessibilityStates[state] ?
          document.body.classList.add("dyslexic-font") :
          document.body.classList.remove("dyslexic-font")
      }
    },
    resetInvertContrast() {
      this.accessibilityStates.inverted = false
      document.body.classList.remove("accessibility-contrast")
    },
    resetGrayscale() {
      this.accessibilityStates.greyscaled = false
      document.body.classList.remove("accessibility-greyscale")
    },
    resetHighlightLinks() {
      this.accessibilityStates.highlighted = false
      this.hightlightLinks()
    },
    hightlightLinks() {
      for (let link of this.links) {
        if (!link.classList.contains("accessibility-menu-item")) {
          this.accessibilityStates.highlighted ? link.classList.add("accessibility-highlight-link") : link.classList.remove("accessibility-highlight-link")
        }
      }
    },
    invertContrast(percent) {
      document.body.style.setProperty('filter', `invert(${percent})`)
    }
  },
  computed: {
    links() {
      return [...document.querySelectorAll('a')]
    }
  },
  mounted() {
    if (localStorage.getItem('accessibility-settings')) {
      this.accessibilityStates = JSON.parse(localStorage.getItem('accessibility-settings'))
      for (var state in this.accessibilityStates) {
        if (this.accessibilityStates[state]) this.applyState(state)
      }
    }
  },
  watch: {
    accessibilityStates: {
      handler() {
        localStorage.setItem('accessibility-settings', JSON.stringify(this.accessibilityStates));
      },
      deep: true,
    },
  }
}
</script>

<style scoped>
  ul {
    margin: 0;
    padding: 0;
    font-weight: normal;
  }
  [data-tooltip] {
    position: relative;
    z-index: 2;
    cursor: pointer;
  }

  /* Hide the tooltip content by default */
  [data-tooltip]:before,
  [data-tooltip]:after {
    visibility: hidden;
    -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=0)";
    filter: progid: DXImageTransform.Microsoft.Alpha(Opacity=0);
    opacity: 0;
    pointer-events: none;
  }

  /* Position tooltip above the element */
  [data-tooltip]:before {
    position: absolute;
    bottom: 105%;
    left: 50%;
    margin-bottom: 5px;
    margin-left: -80px;
    padding: 7px;
    width: 160px;
    -webkit-border-radius: 3px;
    -moz-border-radius: 3px;
    border-radius: 3px;
    background-color: #000;
    background-color: hsla(0, 0%, 20%, 0.9);
    color: #fff;
    content: attr(data-tooltip);
    text-align: center;
    font-size: 14px;
    line-height: 1.2;
  }

  /* Triangle hack to make tooltip look like a speech bubble */
  [data-tooltip]:after {
    position: absolute;
    bottom: 105%;
    left: 50%;
    margin-left: -5px;
    width: 0;
    border-top: 5px solid #000;
    border-top: 5px solid hsla(0, 0%, 20%, 0.9);
    border-right: 5px solid transparent;
    border-left: 5px solid transparent;
    content: " ";
    font-size: 0;
    line-height: 0;
  }

  /* Show tooltip content on hover */
  [data-tooltip]:hover:before,
  [data-tooltip]:hover:after {
    visibility: visible;
    -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=100)";
    filter: progid: DXImageTransform.Microsoft.Alpha(Opacity=100);
    opacity: 1;
  }

  .accessibility-items {
    list-style-type: none;
  }
  .accessibility-toolbar {
    width: 70vw;
    position: absolute;
    left: 65px;
    bottom: 50%;
    margin-bottom: 2px;
  }
  .accessibility-menu-item-icon {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
  .accessibility-menu-item {
    border-radius: 100px;
    border: 2px solid #2196F3;
    text-align: center;
    float: left;
    width: 50px;
    height: 50px;
    transition: all 0.3s ease;
    background: white;
    color: black;
    font-size: 30px;
    cursor: pointer;
    margin-right: 10px;
  }

  .accessibility-menu-item:hover {
    background: #2196F3;
  }

  .active {
    background-color: #2196F3;
  }
</style>

<style>
@font-face {
  font-family: 'opendyslexic';
	src: url('./OpenDyslexic-Regular.woff2');
	font-style: normal;
	font-weight: normal;
} 

  .accessibility-font {
    font-size: 1.25em;
    font-size: 1.25rem;
  }
  .accessibility-greyscale {
    -webkit-filter: grayscale(100%);
    -moz-filter: grayscale(100%);
    filter: grayscale(100%);
    min-height: 100vh;
  }
  .accessibility-contrast {
    -webkit-filter: invert(100%);
    -moz-filter: invert(100%);
    filter: invert(100%);
    min-height: 100vh;
  }
  .accessibility-highlight-link {
    padding: 3px;
    background-color: black !important;
    color: yellow !important;
    text-decoration: underline !important;
  }

  .dyslexic-font {
    cursor: url(http://cursor.in/assets/copy.svg), auto !important;
    font-family: 'opendyslexic' !important;
  }
</style>


<style scoped>

  .fab {
    position: fixed;
    width: 56px;
    left: 3vw;
    bottom: 4vh;
    z-index: 999;
  }
  .fab-action-button {
    cursor: pointer;
    position: absolute;
    bottom: 0;
    display: block;
    width: 56px;
    height: 56px;
    background-color: #2196F3;
    border-radius: 50%;
    box-shadow: 0 2px 2px 0 rgba(0, 0, 0, 0.14), 0 1px 5px 0 rgba(0, 0, 0, 0.12), 0 3px 1px -2px rgba(0, 0, 0, 0.2);
  }
  .fab-action-button-icon {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -53%);
    font-size: 37px !important;
    color: white;
  }
  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s;
  }
  .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
    opacity: 0;
  }

</style>

<style scoped>
  /* fallback */
  @font-face {
    font-family: "Material Icons";
    font-style: normal;
    font-weight: 400;
    src: local("Material Icons"), local("MaterialIcons-Regular"),
      url(https://fonts.gstatic.com/s/materialicons/v17/2fcrYFNaTjcS6g4U3t-Y5ZjZjT5FdEJ140U2DJYC3mY.woff2)
        format("woff2");
  }
  .material-icons {
    font-family: "Material Icons";
    font-weight: normal;
    font-style: normal;
    font-size: 24px;
    line-height: 1;
    letter-spacing: normal;
    text-transform: none;
    display: inline-block;
    white-space: nowrap;
    word-wrap: normal;
    direction: ltr;
    -webkit-font-feature-settings: "liga";
    -webkit-font-smoothing: antialiased;
  }
</style>
