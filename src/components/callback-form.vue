<template>
  <div class="mainWrapper">
    <div class="flexible formHeader">
      <img class="bulbImage" src="./assets/business-energy-lightbulb.png">
      <h2>Let us know if you want us to get back to you with our latest info.</h2>
    </div>
    <div class="form">
      <div class="flexible" v-if="content === 'notSent'">
        <div class="fields">
          <div>Account name:</div>
          <input
            v-model="formFields.accountName"
            placeholder="Enter account name"
            required
            :class="{'errorField': errors.account }"
            @change="checkForms()"
          >
          <div class="error" v-if="errors.account">Missing account name</div>
          <div>Partner name:</div>
          <input
            v-model="formFields.partnerName"
            placeholder="Enter partner name"
            required
            :class="{'errorField': errors.account }"
            @change="checkForms()"
          >
          <div class="error" v-if="errors.partner">Missing partner name</div>
          <div>Full Name:</div>
          <input
            type="text"
            maxlength="100"
            v-model="formFields.fullName"
            placeholder="Enter full name"
            required
            :class="{'errorField': errors.name }"
            @change="checkForms()"
          >
          <div class="error" v-if="errors.name">Missing full name</div>
          <div>Business Name:</div>
          <input
            type="text"
            maxlength="100"
            v-model="formFields.businessName"
            placeholder="Enter business Name"
            required
            :class="{'errorField': errors.business }"
            @change="checkForms()"
          >
          <div class="error" v-if="errors.business">Missing business name</div>
          <div>Phone Number:</div>
          <input
            type="number"
            maxlength="13"
            v-model="formFields.phoneNumber"
            placeholder="Enter UK phone Number"
            pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}"
            required
            :class="{'errorField': errors.phone }"
            @change="checkForms()"
          >
          <div class="error" v-if="errors.phone">Missing phone number</div>
          <div>Email Address:</div>
          <input
            type="text"
            maxlength="80"
            pattern="[@]"
            v-model="formFields.email"
            placeholder="Enter your email"
            required
            :class="{'errorField': errors.noEmail, 'errorField': errors.wrongEmail, }"
          >
          <div class="error" v-if="errors.noEmail">Missing email address</div>
          <div class="error" v-if="errors.wrongEmail">Wrong email format</div>
          <button
            class="formButton"
            v-if="content === 'notSent'"
            @click="sendInfo()"
          >Request a callback</button>
        </div>
        <div class="fieldBlurb">
          <h2>Now we will help with comparing services</h2>
          <h4>Business Energy</h4>
          <ul>
            <li>Save up to £1,027*</li>
            <li>We work with all the suppliers you know and trust</li>
            <li>Get a bespoke FREE quote in minutes and start saving</li>
          </ul>
        </div>
      </div>
      <div v-if="content === 'sent'">
        <h3>Thank you, {{ formFields.fullName }}</h3>
        <p>A member of our team will get in touch with you shortly.</p>
      </div>
      <div v-if="content === 'error'">
        <h3>Oops! it seems there is an error on your submission.</h3>
        <p>A member of our team will get in touch with you shortly.</p>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    let formFields = {
      accountName: "",
      partnerName: "",
      businessName: "",
      fullName: "",
      phoneNumber: "",
      email: ""
    };
    //const reg = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,24}))$/;
    let errorCount = 0;
    let errors = {
      name: false,
      account: false,
      partner: false,
      business: false,
      phone: false,
      noEmail: false,
      wrongEmail: false
    };
    let content = "notSent";
    return {
      formFields,
      content,
      errors
      //reg
    };
  },
  methods: {
    validEmail(el) {
      if (el.includes("@")) {
        return true;
      } else {
        return false;
      }
    },
    checkForms() {
      this.errors = [];
      if (!this.formFields.accountName) {
        this.errorCount++;
        this.errors.account = true;
      }
      if (!this.formFields.partnerName) {
        this.errorCount++;
        this.errors.partner = true;
      }
      if (!this.formFields.businessName) {
        this.errorCount++;
        this.errors.business = true;
      }
      if (!this.formFields.fullName) {
        this.errorCount++;
        this.errors.name = true;
      }
      if (!this.formFields.phoneNumber) {
        this.errorCount++;
        this.errors.phone = true;
      }
      if (!this.formFields.email) {
        this.errorCount++;
        this.errors.noEmail = true;
      } else if (!this.validEmail(this.formFields.email)) {
        this.errorCount++;
        this.errors.wrongEmail = true;
      }
      if (this.errorCount === 0) {
        return true;
      }
    },
    sendInfo() {
      this.checkForms();
      if (this.errorCount === 0) {
        //this.content = "sent";
        axios
          .post(
            "http://mic-leads.dev-test.makeiteasy.com/docs/v1/api.yml/f24028f76c6cc79cf4738056ca742d77"
          )
          .then(response => (this.formFields = response.data))
          .then(() => (this.content = "sent"))
          .catch(error => (this.content = "error"));
      }
    }
  }
};
</script>