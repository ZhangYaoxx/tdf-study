<template>
  <q-list>
    <q-btn
      flat
      round
      icon="arrow_back"
      dense
      class="q-mb-md"
      @click="cancel"
    ></q-btn>
    <q-expansion-item
      switch-toggle-side
      expand-separator
      :value="true"
      label="员工信息"
    >
      <q-card>
        <q-card-section>
          <q-form ref="myForm" class="row justify-center q-gutter-y-md q-pb-md">
            <div class="col-12">
              <div class="row">
                <div class="tdf-label">ID</div>
                <q-input
                  class="col"
                  v-model="detail.id"
                  standout
                  dense
                  clearable
                  disable
                ></q-input>
              </div>
            </div>
            <div class="col-12">
              <div class="row">
                <div class="tdf-label">姓名</div>
                <q-input
                  class="col"
                  v-model="detail.name"
                  standout
                  dense
                  clearable
                  disable
                ></q-input>
              </div>
            </div>
            <div class="col-12">
              <div class="row">
                <div class="tdf-label">性别</div>
                <q-select
                  class="col"
                  v-model="detail.sex"
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
            <div class="col-12">
              <div class="row">
                <div class="tdf-label">部门</div>
                <q-select
                  class="col"
                  v-model="detail.department"
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
            <div class="col-12">
              <div class="row">
                <div class="tdf-label">工号</div>
                <q-input
                  class="col"
                  v-model="detail.number"
                  standout
                  dense
                  clearable
                  disable
                ></q-input>
              </div>
            </div>
            <div class="col-12">
              <div class="row">
                <div class="tdf-label">职务</div>
                <q-select
                  class="col"
                  v-model="detail.job"
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
            <div class="col-12">
              <div class="row">
                <div class="tdf-label">地址</div>
                <q-input
                  class="col"
                  v-model="detail.address"
                  standout
                  dense
                  clearable
                  disable
                ></q-input>
              </div>
            </div>
            <div class="col-12">
              <div class="row">
                <div class="tdf-label">备注</div>
                <q-input
                  class="col"
                  v-model="detail.remark"
                  outlined
                  clearable
                  type="textarea"
                ></q-input>
              </div>
            </div>
          </q-form>
          <div class="col-12 q-pa-md">
            <div class="row justify-end items-center">
              <q-btn
                class="q-mr-lg"
                color="primary"
                unelevated
                :size="$btnSize"
                @click="save"
              >
                修改
              </q-btn>
              <q-btn
                color="info"
                unelevated
                outline
                :size="$btnSize"
                @click="cancel"
              >
                取消
              </q-btn>
            </div>
          </div>
        </q-card-section>
      </q-card>
    </q-expansion-item>
  </q-list>
</template>

<script>
export default {
  name: "Details",
  components: {},
  data() {
    return {
      detail: {},
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
    setData(data) {
      this.detail = data;
    },
    save() {
      this.$refs.myForm.validate().then(success => {
        if (success) {
          this.$emit("editSave", this.detail);
        } else {
          //
        }
      });
    },
    cancel() {
      this.$emit("editCancel");
    }
  }
};
</script>
