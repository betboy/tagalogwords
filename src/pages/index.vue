<template>
  <div class="index">
    <label class="text-reader">
      choose a Tagalog Languague text file
      <input type="file" @change="loadTextFromFile">
    </label>
    <div>
      {{errorMsg}}
    </div>
    <div class="essay">
      <pre>{{showText}}</pre>
    </div>
    <div>
      <ul v-if="this.arrWords.length > 0">
        <li v-for="(word,index) in arrWords" :key="index">
          <div>
            <!--<button @click="remember(word)">remember</button>-->
            {{word}}
            <button class='play' @click="play(word)"></button>
            <!--<button @click="forget(word)">forget</button>-->
          </div>
          <div v-show="highLightWord === word">
            {{highLightComment}}
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<style>
</style>

<script>
import words from './words'
import info from './info'

export default {
  name: 'PageIndex',
  data: function () {
    return {
      text: '',
      showText: '',
      allWords: '',
      wordCount: 0,
      arrWords: [],
      highLightWord: '',
      highLightComment: '',
      errorMsg: ''
    }
  },
  methods: {
    loadTextFromFile (ev) {
      const file = ev.target.files[0]
      const reader = new FileReader()
      reader.onload = e => {
        this.text = e.target.result
        this.showText = this.text
        this.analysis(this.text)
      }
      reader.readAsText(file)
    },
    analysis (text) {
      let myWords = text.toLowerCase().split(/(?:,|\n| |'|")+/)
      let uniqueWords = []
      for (let i = 0; i < myWords.length; ++i) {
        let word = myWords[i]
        if (word.length > 0 && uniqueWords.indexOf(word) === -1) {
          uniqueWords.push(word)
        }
      }
      let tmpAllWords = ''
      uniqueWords.forEach(function (element) {
        tmpAllWords += element + '\n'
      })
      this.allWords = tmpAllWords
      this.wordCount = uniqueWords.length
      this.arrWords = uniqueWords
    },
    remember (word) {
      this.arrWords.splice(this.arrWords.indexOf(word), 1)
    },
    forget (word) {
      this.arrWords.splice(this.arrWords.indexOf(word), 1)
    },
    playAudio (src) {
      if (this.$q.platform.is.android && typeof Audio !== 'undefined') {
        new Audio('/android_asset/www' + src).play()
      } else if (typeof Audio !== 'undefined') {
        new Audio(src).play()
      } if (this.$q.platform.is.android) {
        // eslint-disable-next-line
        var mediaRes = new Media(src,
          function onSuccess () {
          // release the media resource once finished playing
            mediaRes.release()
          },
          function onError (e) {
            this.errorMsg += ' error ' + JSON.stringify(e)
            console.log('error playing sound: ' + JSON.stringify(e))
          })
        mediaRes.play()
      } else {
        this.errorMsg += ' no audio and not in android env ' + src
        console.log('no sound API to play: ' + src)
      }
    },
    play (word) {
      if (typeof words[word] !== 'undefined') {
      // your code here
        let id = words[word]
        this.playAudio('/statics/mp3/' + id + '.mp3')
        this.highLightComment = info[id]
        this.highLightWord = word
      } else {
        this.highLightComment = 'sorry, details not found in the app database'
        this.highLightWord = word
      }
    }
  }
}
</script>

<style lang="scss" scoped>
  .play {
    width: 12px;
    height: auto;
    border-top: 8px solid transparent;
    border-left: 10px solid #000;
    border-bottom: 8px solid transparent;
    border-right: 4px;
  }

  .essay {
    color: blue;
  }

  .index {
    color: green;
    overflow-y: scroll;
  }

  .text-reader {
    position: relative;
    overflow: hidden;
    display: inline-block;

    /* Fancy button style 😎 */
    border: 2px solid black;
    border-radius: 5px;
    padding: 8px 12px;
    cursor: pointer;
  }

  .text-reader input {
    position: absolute;
    top: 0;
    left: 0;
    z-index: -1;
    opacity: 0;
  }
</style>
