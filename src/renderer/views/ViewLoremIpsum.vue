<template>
  <div class="view">
    <div class="area-settings">
      <div class="header"><span><i class="mdi mdi-settings"></i> EINSTELLUNGEN</span></div>

      <div class="box">
        <div class="setting">
          <input type="number" v-model="settingsLength" />
          <div class="text">Länge</div>
        </div>
        <div class="setting">
          <label class="settingCheckbox">
            <input type="checkbox" v-model="settingsGerman" />
            <span><i class="mdi mdi-check"></i></span>
          </label>
          <div class="text">Deutscher Text</div>
        </div>

        <div class="generate">
          <button v-on:click="generateText()">Generieren</button>
          <button v-on:click="clearText()">LÖSCHEN</button>
        </div>
      </div>
    </div>
    <div class="area-text">
      <div class="header"><span><i class="mdi mdi-message-text"></i> GENERIERTER TEXT</span></div>

      <div class="box">
        <div class="noText" v-if="!generatedText">
          <div class="icon"><i class="mdi mdi-textbox"></i></div>
          <span>Klicke auf "Generieren" um<br />einen Text zu generieren</span>
        </div>
        <div class="text" v-if="generatedText">
          <textarea>{{ generatedText }}</textarea>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'view-loremipsum',
    components: {},
    data: function() {
      return {
        generatedText: "",
        settingsGerman: false,
        settingsLength: 150,
      }
    },
    methods: {
      ucwords: function(str) {
        return (str + '').replace(/^([a-z])|\s+([a-z])/g, function ($1) {
            return $1.toUpperCase();
        });
      },
      generateText: function() {
        var templateWords = [];
        var output = "";
        var sinceLastSentence = 0;

        if(this.settingsGerman) {
          templateWords = ["auch", "gibt", "es", "niemanden", "der", "den", "Schmerz", "an", "sich", "liebt", "sucht", "oder", "wünscht", "nur", "weil", "er", "Schmerz", "ist", "es", "sei", "denn", "es", "kommt", "zu", "zufälligen", "Umständen", "in", "denen", "Mühen", "und", "Schmerz", "ihm", "große", "Freude", "bereiten", "können", "Um", "ein", "triviales", "Beispiel", "zu", "nehmen", "wer", "von", "uns", "unterzieht", "sich", "je", "anstrengender", "körperlicher", "Betätigung", "außer", "um", "Vorteile", "daraus", "zu", "ziehen", "aber", "wer", "hat", "irgend", "ein", "Recht", "einen", "Menschen", "zu", "tadeln", "der", "die", "Entscheidung", "trifft", "eine", "Freude", "zu", "genießen", "die", "keine", "unangenehmen", "Folgen", "hat", "oder", "einen", "der", "Schmerz", "vermeidet", "welcher", "keine", "daraus", "resultierende", "Freude", "nach", "sich", "zieht"];
        } else {
          templateWords = ["Lorem", "ipsum", "dolor", "sit", "amet", "consetetur", "sadipscing", "elitr", "sed", "diam", "nonumy", "eirmod", "tempor", "invidunt", "ut", "labore", "et", "dolore", "magna", "aliquyam", "erat", "voluptua", "At", "vero", "eos", "accusam", "justo", "duo", "dolores", "ea", "rebum", "Stet", "clita", "kasd", "gubergren", "no", "sea", "takimata", "sanctus", "est", "Duis", "autem", "vel", "eum", "iriure", "in", "hendrerit", "vulputate", "velit", "esse", "molestie", "consequat", "illum", "eu", "feugiat", "nulla", "facilisis", "at", "eros", "accumsan", "iusto", "odio", "dignissim", "qui", "blandit", "praesent", "luptatum", "zzril", "delenit", "augue", "duis", "te", "feugait", "facilisi", "consectetuer", "adipiscing", "elit", "nonummy", "nibh", "euismod", "tincidunt", "laoreet", "aliquam", "volutpat", "Ut", "wisi", "enim", "ad", "minim", "veniam"];
        }

        for(var i = 0; i < this.settingsLength; i++) {
            var randWord = Math.floor(Math.random() * templateWords.length);
            var randSentence = Math.floor(Math.random() * 4);

            if(i==0 || this.settingsLength < 10 || sinceLastSentence < 4) { randSentence = 1; }

            var addedWord = templateWords[randWord];

            if(i==0) { addedWord = this.ucwords( addedWord ); }

            if(randSentence == 0) {
                output = output + ". " + this.ucwords( addedWord );
                sinceLastSentence = 0;
            } else {
              output = output + " " + addedWord;
              sinceLastSentence = sinceLastSentence + 1;
            }
        }

        this.generatedText = output + ".";
      },
      clearText: function() {
        this.generatedText = "";
      }
    }
  }
</script>

<style scoped>
.view {
  padding: 25px;
  display: grid;
  grid-template-columns: 300px 1fr;
  grid-gap: 25px;
}
.box .noText {
  padding: 25px 0px;
  text-align: center;
  line-height: 1.15;
}
.box .noText .icon i {
  font-size: 32px;
  transition: 0.2s ease all;
  color: var(--colorPrimary);
}
.box .noText .icon {
  margin-bottom: 15px;
}
.box .noText span {
  font-weight: bold;
  font-size: 12px;
  text-transform: uppercase;
}
.box input:focus, .box textarea:focus {
  transition: 0.2s ease all;
  border-color: var(--colorPrimary);
  outline: 0;
}
.box .text textarea {
  width: 100%;
  height: 350px;
  line-height: 1.5;
}
.box .setting {
  display: grid;
  grid-template-columns: auto 1fr;
  grid-gap: 10px;
  align-items: center;
}
.box .setting .text {
  font-weight: bold;
  font-size: 12px;
  text-transform: uppercase;
}
.box .setting .settingCheckbox span {
  width: 22px;
  height: 22px;
  display: block;
  border-radius: 6px;
  background: var(--colorShade);
  border: 2px solid var(--colorInputOutlineGrey);
  cursor: pointer;
  color: transparent;
  transition: 0.2s ease all;
}
.box .setting .settingCheckbox input[type="checkbox"] {
  display: none;
}
.box .setting .settingCheckbox input[type="checkbox"]:checked + span {
  transition: 0.2s ease all;
  border-color: var(--colorPrimary);
  color: var(--colorBoxFont);
}
.box .setting input[type="number"] {
  padding-top: 5px;
  padding-bottom: 5px;
  width: 90px;
}
.box .generate {
  margin-top: 15px;
  display: grid;
  grid-gap: 10px;
}
.box .generate button {
  width: 100%;
  text-transform: uppercase;
  background: var(--colorPrimary);
  border: 2px solid transparent;
  color: var(--colorPrimaryText);
  font-size: 12px;
  border-radius: 6px;
  padding: 10px 0px;
  text-align: center;
  font-weight: bold;
  transition: 0.2s ease all;
  margin-right: 10px;
}
.box .generate button:hover {
  background: var(--colorPrimaryText);
  color: var(--colorPrimary);
  border-color: var(--colorPrimary);
  cursor: pointer;
}
.box .generate button:active {
  background: var(--colorPrimary);
  color: var(--colorPrimaryText);
}
.box .generate button:focus {
  outline: 0;
}

@media screen and (max-width: 800px) {
  .view {
    grid-template-columns: 1fr;
  }
}
</style>
