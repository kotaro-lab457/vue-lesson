<template>
  <div>
    <h1 v-html="leads.message" :class="classObject"></h1>
    <p>{{ leads.description }}</p>
    <button @click="addDescription">add description</button>
    <!-- <h1 v-html="message" :class="classObject"></h1>
    <p>{{ description }}</p>
    <button @click="changeTextSize">large</button> -->
    <hr />
    <child-component v-show="isShow">
      <template v-slot:head>
        <p>head slot</p>
      </template>
      <template v-slot:default>
        <p>main slot</p>
        <p>main slot2</p>
      </template>
      <template v-slot:foot>
        <p>foot slot</p>
      </template>
    </child-component>
    <hr />
    <p v-if="id === 1">1</p>
    <template v-else-if="id === 2">
      <p>2-1</p>
      <p>2-2</p>
      <p>2-3</p>
    </template>
    <p v-else>Other</p>
    <hr />
    <template v-for="item in items">
      <child-component :key="item.id" :title="item.title">
        <span>slot content</span>
      </child-component>
    </template>
    <hr />
    <button @click="incrementCount">Add to count</button>
    <p>{{ count }}回クリックされました</p>
    <hr />
    <input type="text" v-model="inputText" />
    <p>computed: {{ getUpperCaseText }}</p>
    <p>methods: {{ showUpperCaseText() }}</p>
    <hr />
    <form>
      <div>
        <span>名前:</span>
        <input
          type="text"
          :value="form.name"
          @input="form.name = $event.target.value"
        />
        <!-- $event.target.valueで入力している値を取得。 -->
        <p>名前: {{ getInputName }}</p>
      </div>
      <div>
        <span>性別:</span>
        <label>
          男性
          <input type="radio" value="male" v-model="form.sex" />
        </label>
        <label>
          女性
          <input type="radio" value="female" v-model="form.sex" />
        </label>
        <p>性別: {{ getRadioValue }}</p>
      </div>
      <div>
        <!-- <select v-model="form.selected">
          <option disabled value="">--出身地を選択してください--</option>
          <option>東京都</option>
          <option>埼玉県</option>
          <option>神奈川県</option>
          <option>千葉県</option>
        </select> -->
        <select v-model="form.selected">
          <option disabled value="">--出身地を選択してください--</option>
          <option
            v-for="option in form.options"
            :value="option.value"
            :key="option.id"
          >
            {{ option.value }}
          </option>
        </select>
        <p>出身地:{{ getSelectValue }}</p>
      </div>
      <div>
        <label>
          <input type="checkbox" v-model="form.checked" />
          20際以上です
        </label>
        <p>チェックボックス: {{ getCheckBoxValue }}</p>
      </div>
    </form>
    <hr />
    <template v-for="category in categories">
      <p :key="category.id">
        {{ category }}
      </p>
    </template>
    <button @click="updateText">update text</button>
    <hr />
    <counter :count="count" @increment="incrementCount"></counter>
    <hr />
    <span>名前:</span>
    <input-text v-model="form.name"></input-text>
    <p>名前: {{ getInputName }}</p>
  </div>
</template>
<script>
import ChildComponent from "Components/ChildComponent";
import Counter from "Components/Counter";
import InputText from "Components/InputText";

export default {
  data() {
    return {
      // リアクティブプロパティ
      leads: {
        message: "<span>Hello Vue</span>",
        description: "",
      },
      message: "<span>Hello Vue</span>",
      // description: "",
      isShow: true,
      id: 2,
      count: 0,
      inputText: "",
      classObject: {
        "is-green": true,
      },
      items: [
        {
          id: this.$uuid.v4(),
          title: "1番目のリスト",
        },
        {
          id: this.$uuid.v4(),
          title: "２番目のリスト",
        },
        {
          id: this.$uuid.v4(),
          title: "3番目のリスト",
        },
      ],
      form: {
        name: "",
        sex: "",
        selected: "",
        options: [
          {
            id: this.$uuid.v4(),
            value: "東京都",
          },
          {
            id: this.$uuid.v4(),
            value: "埼玉県",
          },
          {
            id: this.$uuid.v4(),
            value: "神奈川県",
          },
          {
            id: this.$uuid.v4(),
            value: "千葉県",
          },
        ],
        checked: false,
      },
      categories: ["Javascript", "jQuery"],
    };
  },
  methods: {
    incrementCount() {
      this.count++;
    },
    showUpperCaseText() {
      const upperCaseText = this.inputText.toUpperCase();
      // console.log(`method: ${upperCaseText}`);
      return upperCaseText;
    },
    addDescription() {
      // this.description = "Vue-lesson";
      this.leads.description = "Vue-lesson";
      // console.log(this);
      // console.log(this.description);
    },
    updateText() {
      this.categories.splice(1, 1, "Vue.js");
      // splice(インデックス, 変更する要素数, 変更要素)
    },
    changeTextSize() {
      // Object.assign()でプロパティを追加。第一にからのオブジェクト、第二にthis.classObject、第三に新しいオブジェクトを代入。
      this.classObject = Object.assign({}, this.classObject, {
        "is-large": true,
      });
    },
  },
  computed: {
    // form
    getInputName() {
      return this.form.name;
    },
    // チェックボックス
    getCheckBoxValue() {
      return this.form.checked;
    },
    getRadioValue() {
      return this.form.sex;
    },
    getUpperCaseText() {
      const upperCaseText = this.inputText.toUpperCase();
      // console.log(`computed: ${upperCaseText}`);
      return upperCaseText;
    },
    getRadioValue() {
      return this.form.sex;
    },
    getSelectValue() {
      return this.form.selected;
    },
  },
  watch: {
    inputText(value, oldValue) {
      console.log(`value -> ${value}`); //現在の値
      console.log(`oldValue -> ${oldValue}`); //前回の値
    },
    leads: {
      handler() {
        console.log("add description");
      },
      deep: true,
    },
  },
  components: {
    ChildComponent,
    Counter,
    InputText,
  },
};
</script>

<style scoped>
.is-green {
  color: green;
}
.is-large {
  font-size: 48px;
}
hr {
  margin: 16px 0;
}
</style>