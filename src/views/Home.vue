<template>
  <div class="container">
    <form @submit.prevent="onSubmit">
      <div class="form-row">
        <div class="col-md-4 form-group">
          <label>Date</label>
          <DateTimePicker name="date" v-model="formData.date" />
          <div v-if="v$.formData.date.$error" class="error-text">Date is required.</div>
        </div>
        <div class="col-md-4 form-group">
          <label>Source</label>
          <Select2 name="source" v-model="formData.source" :options="sourceData" :settings="selectSettings" @change="myChangeEvent($event)" @select="mySelectEvent($event)" />
          <div v-if="v$.formData.source.$error" class="error-text">Source is required.</div>
        </div>
        <div class="col-md-4 form-group">
          <label>Created On</label>
          <DateTimePicker name="createdOn" v-model="formData.createdOn" />
          <div v-if="v$.formData.createdOn.$error" class="error-text">Created On is required.</div>
        </div>
      </div>
      <div class="form-row">
        <div class="col-md-4 form-group">
          <label>Investor Name</label>
          <Select2 v-model="formData.investerName" :options="investerNames" :settings="selectSettings" @select="investerNameChange($event)" />
          <div v-if="v$.formData.investerName.$error" class="error-text">Invester Name is required.</div>
        </div>
        <div class="col-md-4 form-group">
          <label>Tax Status</label>
          <input type="text" v-model="formData.taxStatus" class="form-control" :disabled="formData.investerName != ''">
          <div v-if="v$.formData.taxStatus.$error" class="error-text">Tax Status is required.</div>
        </div>
        <div class="col-md-4 form-group">
          <label>PAN</label>
          <input type="text" v-model="formData.pan" class="form-control" :disabled="formData.investerName != ''">
          <div v-if="v$.formData.pan.$error" class="error-text">PAN is required.</div>
        </div>
      </div>
      <div class="form-row">
        <div class="col-md-4 form-group">
          <label>DOB</label>
          <input type="text" v-model="formData.dob" class="form-control" :disabled="formData.investerName != ''">
          <div v-if="v$.formData.dob.$error" class="error-text">DOB is required.</div>
        </div>
        <div class="col-md-4 form-group">
          <label>Phone</label>
          <input name="phone" v-model="formData.phone"  type="text" class="form-control" @keypress="isNumber($event)">
          <div class="error-text" v-for="error of v$.formData.phone.$errors" :key="error.$uid">
            <div class="error-msg">{{ error.$message }}</div>
          </div>
        </div>
        <div class="col-md-4 form-group">
          <label>Email</label>
          <input name="email" v-model="formData.email" type="email" class="form-control">
          <div class="error-text" v-for="error of v$.formData.email.$errors" :key="error.$uid">
            <div class="error-msg">{{ error.$message }}</div>
          </div>
        </div>
      </div>
      <div class="form-row">
        <div class="col-md-4 form-group">
          <label>Product</label>
          <Select2 v-model="formData.product" :options="productsList" :settings="selectSettings" @select="productChange($event)" />
          <div v-if="v$.formData.product.$error" class="error-text">Product is required.</div>
        </div>
        <div class="col-md-4 form-group">
          <label>Category</label>
          <!-- <input type="text" v-model="formData.category" class="form-control"> -->
          <select class="form-control" v-model="formData.category">
            <option value="">Select Category</option>
            <option v-for="option in productCategories" :value="option.code" :key="option.code">
              {{ option.name }}
            </option>
          </select>
          <div v-if="v$.formData.category.$error" class="error-text">Category is required.</div>
        </div>
        <div class="col-md-4 form-group">
          <label>Lead Size</label>
          <input type="text" v-model="formData.leadSize" class="form-control" @keypress="isNumber($event)">
          <div v-if="v$.formData.leadSize.$error" class="error-text">Lead Size is required.</div>
        </div>
      </div>
      <div class="form-row">
        <div class="col-md-4 form-group">
          <label>MAT Probability</label>
          <input type="text" v-model="formData.matProbality" class="form-control" @keypress="isNumber($event)">
          <div class="error-text" v-for="error of v$.formData.matProbality.$errors" :key="error.$uid">
            <div class="error-msg">{{ error.$message }}</div>
          </div>
        </div>
        <div class="col-md-4 form-group">
          <label>Followups dates</label>
          <div class="bg-white p-2 w-full border rounded">
            <DatePicker v-model="selected.date" :masks="masks" :model-config="modelConfig">
              <template #default="{ togglePopover, hidePopover }">
                <div class="d-flex flex-wrap">
                  <button type="button"
                    v-for="date in formData.followupsDates"
                    :key="date.date.getTime()"
                    class="inline-flex btn badge badge-light mr-1"
                    @click.stop="dateSelected($event, date, togglePopover)"
                    ref="dateButton"
                  >
                    {{ date.date.toLocaleDateString() }}
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-x" viewBox="0 0 16 16" @click.stop="removeDate(date, hidePopover)">
                      <path d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708z"/>
                    </svg>
                  </button>
                  <button type="button" class="btn badge badge-info" @click.stop="addDate">+ Add Date</button>
                </div>
              </template>
            </DatePicker>
          </div>
          <div v-if="v$.selected.date.$error" class="error-text">Followups Dates is required.</div>
        </div>
        <div class="col-md-4 form-group">
          <label>Status</label>
          <select class="form-control" v-model="formData.status">
            <option value="">Select Status</option>
            <option value="AL">Allocated</option>
            <option value="HO">Hot</option>
            <option value="IN">Inquiry</option>
          </select>
          <div v-if="v$.formData.status.$error" class="error-text">Status is required.</div>
        </div>
      </div>
      <div class="text-center">
        <button type="submit" class="btn btn-primary">Save</button>
      </div>
    </form>

    
    <div class="py-5">
      {{ payload }}
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import DateTimePicker from "@/components/DateTimePicker.vue";
import Select2 from 'vue3-select2-component';
import { Calendar, DatePicker } from 'v-calendar';
import { vSelect } from '@desislavsd/vue-select';
import useVuelidate from '@vuelidate/core'
import { required, email, helpers, between } from '@vuelidate/validators';
import moment from 'moment';

const phoneRegex = helpers.regex(/^\d{10}$/);

export default {
  name: 'Home',
  components: {
    DateTimePicker,
    Select2,
    Calendar,
    DatePicker,
    vSelect
  },
  setup () {
    return { v$: useVuelidate() }
  },
  data() {
    return {
      myValue: '',
      sourceData: ['Offline', 'Online', 'Phone', 'Reference'],
      investerData:  [
        {
          name: 'Rohit Thakur', 
          taxStatus: 'Individual', 
          pan: 'ABCCD1234A', 
          dob: '08-09-1982', 
          phone: 1234567890, 
          email: 'test@example.com'
        },
        {
          name: 'Raj Thakur', 
          taxStatus: 'Individual', 
          pan: 'ABCCD1232A', 
          dob: '08-09-1985', 
          phone: 1234567822,
          email: 'test2@example.com'
        }
      ],
      productsData: [
        {
          code: 'EQ', 
          name: 'Equity', 
          categories: [
            {
              code: 'RS', 
              name: 'Research on Stocks'
            }
          ]
        }, 
        {
          code: 'LI', 
          name: 'Life Insurance', 
          categories: [
            {
              code: 'CP', 
              name: 'Child Plan'
            },
            { 
              code: 'TP', 
              name: 'Term Plan'
            }
          ]
        }
      ],
      productCategories: [],
      date: new Date(),
      selectSettings: {
        width: '100%',
        placeholder: 'Select option'
      },
      selected: {},
      masks: {
        input: 'DD-MM-YYYY'
      },
      modelConfig: {
        mask: 'DD-MM-YYYY'
      },
      formData: {
        date: '',
        source: '',
        createdOn: '',
        investerName: '',
        taxStatus: '',
        pan: '',
        dob: '',
        product: '',
        category: '',
        leadSize: '',
        matProbality: '',
        status: '',
        phone: '',
        email: '',
        followupsDates: []
      },
      payload: ''
    }
  },
  computed: {
    investerNames() {
      return this.investerData.map(el => {
        return { id: el.name, text: el.name }
      });
    },
    productsList() {
      return this.productsData.map(el => {
        return {
          id: el.code,
          text: el.name
        }
      })
    }
  },
  validations() {
    return {
      selected: { 
        date: { required, $autoDirty: true }
      },
      formData: {
        date: { required, $autoDirty: true },
        source: { required, $autoDirty: true },
        createdOn: { required, $autoDirty: true },
        investerName: { required, $autoDirty: true },
        taxStatus: { required, $autoDirty: true },
        pan: { required, $autoDirty: true },
        dob: { required, $autoDirty: true },
        phone: { 
          required: helpers.withMessage('Phone is required.', required), 
          $autoDirty: true,
          phoneRegex: helpers.withMessage('Please enter valid phone no.', phoneRegex)
        },
        category: { required, $autoDirty: true },
        leadSize: { required, $autoDirty: true },
        email: { 
          required: helpers.withMessage('Email is required.', required), 
          email: helpers.withMessage('Plese enter valid email id.', email), 
          $autoDirty: true 
        },
        product: { required, $autoDirty: true },
        matProbality: { 
          required: helpers.withMessage('MAT Probability is required.', required), 
          $autoDirty: true,
          between: helpers.withMessage('The value must be between 0 and 100', between(1, 99)) 
        },
        status: { required, $autoDirty: true },
        followupsDates: { 
          datelength: helpers.withMessage('', (value) => {
            if (value.length > 0) return true;
            return false;
          }), 
          $autoDirty: true 
        }
      }
    }
  },
  methods: {
    myChangeEvent(val) {
      console.log(val);
    },
    mySelectEvent({id, text}) {
      console.log({id, text})
    },
    investerNameChange({id, text}) {
      let obj = this.investerData.filter(el => el.name == id)[0];
      if (obj) {
        this.formData.taxStatus = obj.taxStatus;
        this.formData.pan = obj.pan;
        this.formData.dob = obj.dob;
        this.formData.phone = obj.phone;
        this.formData.email = obj.email;
      }
    },
    productChange({id, text}) {
      let obj = this.productsData.filter(el => el.code == id)[0];
      console.log(obj);
      if (obj) {
        this.productCategories = obj.categories;
      }
    },
    addDate() {
      let date = new Date();
      this.formData.followupsDates.push({
        date: new Date(),
      });
      this.$nextTick(() => {
        const btn = this.$refs.dateButton;
        btn.click();
      });
    },
    removeDate(date, hide) {
      this.formData.followupsDates = this.formData.followupsDates.filter((d) => d !== date);
      hide();
    },
    dateSelected(e, date, toggle) {
      this.selected = date;
      toggle({ ref: e.target });
    },

    onSubmit(event) {
      this.v$.$touch();
      if (this.v$.$error) return;
      let result = JSON.parse(JSON.stringify(this.formData));
      let payload = result;
      payload.followupsDates = result.followupsDates.map(el => {
        return moment(el.date).format('DD-MM-YYYY')
      });
      this.payload = JSON.stringify(payload);
    },

    onDayClick(day) {
      const idx = this.days.findIndex(d => d.id === day.id);
      if (idx >= 0) {
        this.days.splice(idx, 1);
      } else {
        this.days.push({
          id: day.id,
          date: day.date,
        });
      }
    },

    isNumber: function(evt) {
      evt = (evt) ? evt : window.event;
      var charCode = (evt.which) ? evt.which : evt.keyCode;
      if ((charCode > 31 && (charCode < 48 || charCode > 57))) {
        evt.preventDefault();;
      } else {
        return true;
      }
    }
  }
}
</script>