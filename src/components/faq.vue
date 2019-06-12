<template>
  <div class="faq">
    <ul class="faqList">
      <li class="faqList__element" v-for="(faqListElement, index) of faqList" :key="index">
        <div class="faqList__elementContent">
          <div class="faqList__elementContent--question">
            {{ faqListElement.question }}
            <div class="faqList__elementContent--tick" @click="active_el = index">[+]</div>
          </div>
          <div
            class="faqList__elementContent--answer"
            :class="{'show': active_el == index}"
          >{{ faqListElement.answer }}</div>
        </div>
      </li>
    </ul>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    let faqList;
    let active_el = 0;
    return {
      faqList,
      active_el
    };
  },
  mounted() {
    axios
      .get("https://api.myjson.com/bins/jw3rg")
      .then(response => (this.faqList = response.data.faqs))
      .catch(error => "nothing returned");
  }
};
</script>