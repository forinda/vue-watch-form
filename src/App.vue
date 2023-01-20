  <!-- Vue form component and watchers to watch for any value in form that changes -->
  <template>
    <div id="app" class="flex justify-center min-h-screen min-w-full items-center">
      <form class="text-xl p-10 flex flex-col gap-4 w-full max-w-[40rem] shadow-xl border border-black">
        <h1 class="text-5xl font-bold text-center my-6">Contact Form</h1>
        <div class="flex flex-col gap-2">
          <label for="name">Name</label>
          <input
            type="text"
            class="border-[2px] border-slate-200 rounded p-2"
            id="name"
            v-model="formData.name"
            placeholder="Enter name"
          />
        </div>
        <div class="flex flex-col gap-2">
          <label for="email">Email address</label>
          <input
            type="email"
            class="border-[2px] border-slate-200 rounded p-2"
            id="email"
            v-model="formData.email"
            placeholder="Enter email"
          />
        </div>
        <div class="flex flex-col gap-2">
          <label for="phone">Phone</label>
          <input
            type="text"
            class="border-[2px] border-slate-200 rounded p-2"
            id="phone"
            v-model="formData.phone"
            placeholder="Enter phone"
          />
        </div>
        <div class="flex flex-col gap-2">
          <label for="message">Message</label>
          <textarea
            class="border-[2px] border-slate-200 rounded p-2"
            id="message"
            rows="3"
            placeholder="Enter message"
            v-model="formData.message"
          ></textarea>
        </div>
        <button
          :disabled="formSubmitable ? false : true"
          type="submit"
          :class="[
            formSubmitable
              ? 'bg-blue-500 text-white border'
              : 'bg-neutral-300 text-neutral-900 cursor-not-allowed',
            'py-2 rounded-md',
          ]"
          @click.prevent="submitForm"
        >
          Submit
        </button>
      </form>
    </div>
  </template>

  <script lang="ts">
  import { defineComponent } from "vue";

  type FormFields = {
    name: string;
    email: string;
    phone: string;
    message: string;
  };

  type DataStateType = {
    formData: FormFields;
    formSubmitable: boolean;
  };

  export default defineComponent({
    name: "App",
    data(): DataStateType {
      return {
        formData: {
          name: "",
          email: "",
          phone: "",
          message: "",
        },
        formSubmitable: false,
      };
    },
    watch: {
      formData: {
        handler(newValue: FormFields) {
          console.log("watcher", {
            data: JSON.stringify(newValue),
            submit: this.formSubmitable,
          });

          this.updateForm(newValue);
        },
        deep: true,
      },
    },
    methods: {
      submitForm() {
        console.log(this.formData);

        console.log("submit form");
      },

      updateForm(newValue: FormFields) {
        this.formData = newValue;
        this.validateForm(this.formData);
      },
      validateForm(data: FormFields) {
        const { name, email, phone, message } = data;
        if (name && email && phone && message) {
          this.formSubmitable = true;
        } else {
          this.formSubmitable = false;
        }
      },
    },
  });
  </script>
