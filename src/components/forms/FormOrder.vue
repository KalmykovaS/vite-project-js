<template>
  <div class="form-order">
    <div class="form-order__form bg-black">
      <div class="flex items-center justify-between">
        <h2 class="text-xl">Order Form</h2>
        <button
            type="button"
            class="px-2 py-1"
            @click="$emit('close-modal', $event)"
        >
          X
        </button>
      </div>
      <div v-if="!isSuccess && !isError">
        <Form
            :initial-values="initialValues"
            :validation-schema="schema"
            @submit="onSubmit"
            class="flex flex-col gap-2 mt-4"
        >

          <div class="flex flex-col gap-1">
            <Field
                class="rounded-md p-2"
                name="firstName"
                type="text"
                placeholder="First Name"
            />
            <ErrorMessage class="text-red-500 text-sm" name="firstName" />
          </div>

          <div class="flex flex-col gap-1">
            <Field
                class="rounded-md p-2"
                name="lastName"
                type="text"
                placeholder="Last Name"
            />
            <ErrorMessage class="text-red-500 text-sm" name="firstName" />
          </div>

          <div class="flex flex-col gap-1">
            <Field
                class="rounded-md p-2"
                name="email"
                type="email"
                placeholder="Email"
            />
            <ErrorMessage class="text-red-500 text-sm" name="email" />
          </div>

          <div class="flex flex-col gap-1">
            <Field
                as="select"
                name="countries"
                class="rounded-md p-2"
            >
              <option
                  v-for="option in options"
                  :key="option.value"
                  :value="option.value"
              >
                {{ option.text }}
              </option>
            </Field>
          </div>

          <div class="flex flex-col gap-1">
            <Field
                class="rounded-md p-2"
                name="address"
                type="text"
                placeholder="Address"
            />
            <ErrorMessage class="text-red-500 text-sm" name="address" />
          </div>

          <div class="flex items-center justify-between">
            <label>
              <Field
                  name="payment"
                  type="radio"
                  value="card"
              />
              Payment by card
            </label>
            <label>
              <Field
                  name="payment"
                  type="radio"
                  value="cash"
              />
              Payment by cash
            </label>
          </div>

          <div
              class="flex flex-col gap-1"
          >
            <Field
                name="cardNumber"
                class="rounded-md p-2"
                type="text"
                placeholder="Card number"
            />
            <ErrorMessage class="text-red-500 text-sm" name="cardNumber" />
          </div>

          <div class="flex flex-col gap-1">
            <label>
              <Field
                  name="agreementOrder"
                  type="checkbox"
              />
              I agree to order processing
            </label>
            <ErrorMessage class="text-red-500 text-sm" name="agreementOrder" />
          </div>

          <div class="flex items-center justify-between">
            <label>
              <Field
                  name="promo"
                  value="email-promo"
                  type="checkbox"
              />
              Receive promo by email
            </label>
            <label>
              <Field
                  name="promo"
                  value="phone-promo"
                  type="checkbox"
              />
              Receive promo by phone
            </label>
          </div>

          <button
              type="submit"
              class="px-4 py-2 border-slate-300 rounded-2xl mt-4"
          >
            Submit
          </button>
        </Form>
      </div>
      <div v-else-if="isSuccess">
        Your order has been successfully placed!
      </div>
      <div v-else>
        An error occurred :(
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue';
import { Field, Form, ErrorMessage } from 'vee-validate';
import * as yup from 'yup';
import axios from 'axios';

const initialValues = {
  email: '',
  firstName: '',
  lastName: '',
  countries: '01',
  address: '',
  // вводим значение, если нужно предзаполненный radio, можем указать либо один, либо ничего
  payment: 'card',
  cardNumber: '',
  agreementOrder: false,
  // вводим значение, если нужно предзаполненный checkbox, можем указать ни одного или оба
  promo: ['email-promo'],
}

const options = ref([
  { value: '01', text: 'Russia' },
  { value: '02', text: 'China' },
  { value: '03', text: 'Other' },
]);

const schema = yup.object({
  email: yup.string().email().required(),
  firstName: yup.string().required(),
  lastName: yup.string().required(),
  address: yup.string().required(),
  agreementOrder: yup.boolean().oneOf([true]),
});

const isSuccess = ref(false);
const isError = ref(false);

async function onSubmit(values) {
  try {
    const response = await axios.post('https://httpbin.org/anything', values);

    if (response.status === 200) {
      isSuccess.value = true;
      console.log('Response:', response.data);
    }
  } catch (err) {
    isError.value = true;
    console.error('Error:', err);
  }
}
</script>

<style scoped>
.form-order {
  display: none;
  pointer-events: none;
  position: fixed;
  width: 100vw;
  height: 100dvh;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 100;
  background-color: rgba(0,0,0,0.2);
}

.form-order.active {
  display: block;
  pointer-events: auto;
}

.form-order__form {
  width: 500px;
  border-radius: 2rem;
  padding: 1rem;
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);

  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
</style>