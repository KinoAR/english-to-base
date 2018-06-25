<template>
  <div class="container">
    <h2 class="title-heading">English To Base</h2>
    <form action="">
      <div class="form-group"><input v-model="base" type="text" class="form-control">
      <small class="form-text text-muted">
        Enter your base to convert the characters to that base.
      </small>
      </div>
      <div class="form-group">
        <textarea v-model="english" name="englishInput" id="englishInput" cols="30" rows="10" class="form-control">
        </textarea>
      </div>
      <div class="form-group">
        <textarea v-model="binary" name="binaryOutput" id="binaryOutput" cols="30" rows="10" class="form-control">
        </textarea>
      </div>
    </form>
  </div>
</template>


<script>
import * as R from 'ramda';
export default {
  data() {
    return {
      name: 'Main',
      english: '',
      base: 2,
      binary: '',
    }
  },
  watch: {
    english(value) {
      this.englishToBase(value, this.base);
    }, base(value) {
      this.englishToBase(this.english,value);
    }
  },
  methods: {
    englishToBase(text, base) {
      const finalBase = R.clamp(2, 36, base);
      const binary = this.splitText(text)
        .map(character => character.codePointAt(0))
        .map(R.curry(this.toBase)(finalBase));
      
      this.binary = binary.join("").replace(/\s+(\D)/g, (match, p1) => {
        return p1;
      });
    },
    splitText(text) {
      return R.split("", text);
    },
    toBase(base, num) {
      return this.getPunctuation(num) === undefined ? 
      Number(num).toString(base) + " " : this.getPunctuation(num);
    },
    getPunctuation(num) {
      return R.find(punctuation => punctuation.codePointAt(0) === num ,[
        '.',
        ' ',
        '|',
        '!',
        '?',
        ';',
        ':',
        '"',
        "'",
        '/',
        '\\',
        '=',
        '+',
        '_',
        ")",
        "(",
        '*',
        '$',
        '%',
        '^',
        "&",
        '#',
        '@',
        ',',
        '[',
        ']',
        '{',
        '}',
        '>',
        '<'
      ])
    }
  }  
}
</script>


<style>
.title-heading {
  margin-bottom: 30px;
}
</style>
