<template>
  <div>
    <q-form
      ref="myForm"
      :model="form"
      class="row justify-center q-gutter-y-md q-pa-lg"
    >
      <div class="col-12 col-md-10">
        <div class="row">
          <form-lable requst lable="姓名:"></form-lable>
          <q-input
            class="col"
            v-model="form.name"
            :rules="[val => !!val || '请输入姓名']"
            hide-bottom-space
            outlined
            dense
            clearable
          ></q-input>
        </div>
      </div>
      <div class="col-12 col-md-10">
        <div class="row">
          <form-lable requst lable="性别:"></form-lable>
          <q-select
            class="col"
            v-model="form.sex"
            transition-show="jump-up"
            transition-hide="jump-up"
            :options-dense="true"
            placeholder="请选择性别"
            outlined
            dense
            clearable
            emit-value
            map-options
            :options="options"
          ></q-select>
        </div>
      </div>
      <div class="col-12 col-md-10">
        <div class="row">
          <form-lable requst lable="地址:"></form-lable>
          <q-select
            class="col"
            v-model="form.address"
            transition-show="jump-up"
            transition-hide="jump-up"
            :options-dense="true"
            outlined
            dense
            clearable
            emit-value
            map-options
          >
            <choose-address
              @getAddress="getAddress"
              @resetAddress="resetAddress"
            />
          </q-select>
        </div>
      </div>
      <div class="col-12 col-md-10">
        <div class="row">
          <form-lable requst lable="部门:"></form-lable>
          <q-select
            class="col"
            v-model="form.department"
            transition-show="jump-up"
            transition-hide="jump-up"
            :options-dense="true"
            placeholder="请选择部门"
            outlined
            dense
            clearable
            emit-value
            map-options
            :options="departmentOptions"
          ></q-select>
        </div>
      </div>
      <div class="col-12 col-md-10">
        <div class="row">
          <form-lable requst lable="工号:"></form-lable>
          <q-input
            class="col"
            v-model="form.number"
            :rules="[val => !!val || '请输入工号']"
            hide-bottom-space
            outlined
            dense
            clearable
          ></q-input>
        </div>
      </div>
      <div class="col-12 col-md-10">
        <div class="row">
          <form-lable requst lable="职务:"></form-lable>
          <q-select
            class="col"
            v-model="form.job"
            transition-show="jump-up"
            transition-hide="jump-up"
            :options-dense="true"
            placeholder="请选择职务"
            outlined
            dense
            clearable
            emit-value
            map-options
            :options="jobOptions"
          ></q-select>
        </div>
      </div>
      <div class="col-12 col-md-10">
        <div class="row">
          <form-lable requst lable="备注:"></form-lable>
          <q-input
            class="col"
            v-model="form.remark"
            outlined
            clearable
            type="textarea"
          ></q-input>
        </div>
      </div>
    </q-form>
    <q-separator></q-separator>
    <div class="col-12 q-pa-md">
      <div class="row justify-end items-center">
        <q-btn
          class="q-mr-lg"
          color="primary"
          unelevated
          :size="$btnSize"
          @click="save"
        >
          保存
        </q-btn>
        <q-btn color="info" unelevated outline :size="$btnSize" @click="cancel">
          取消
        </q-btn>
      </div>
    </div>
  </div>
</template>

<script>
import FormLable from "@componets/formlable/FormLable";
import ChooseAddress from "../examples/formpage/components/ChooseAddress.vue";

export default {
  name: "Add",
  components: { FormLable, ChooseAddress },
  data() {
    return {
      form: {
        id: null,
        sex: null,
        department: null,
        number: null,
        job: null,
        address: ""
      },
      options: [
        {
          label: "男",
          value: 1
        },
        {
          label: "女",
          value: 0
        }
      ],
      departmentOptions: [
        {
          label: "产品部",
          value: "产品部"
        },
        {
          label: "项目部",
          value: "项目部"
        },
        {
          label: "技术部",
          value: "技术部"
        }
      ],
      jobOptions: [
        {
          label: "初级工程师",
          value: "初级工程师"
        },
        {
          label: "中级工程师",
          value: "中级工程师"
        },
        {
          label: "高级工程师",
          value: "高级工程师"
        }
      ]
    };
  },
  methods: {
    // 选择省市县
    getAddress(v) {
      this.form.address = v;
    },
    resetAddress() {
      this.form.address = "";
    },
    save() {
      this.$refs.myForm.validate().then(success => {
        if (success) {
          this.$emit("addSave", this.form);
        } else {
          //
        }
      });
    },
    cancel() {
      this.$emit("addCancel");
    }
  }
};
</script>
