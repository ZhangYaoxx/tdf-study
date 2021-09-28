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
                <q-input
                  class="col"
                  v-model="detail.sex"
                  standout
                  dense
                  clearable
                  disable
                ></q-input>
              </div>
            </div>
            <div class="col-12">
              <div class="row">
                <div class="tdf-label">部门</div>
                <q-input
                  class="col"
                  v-model="detail.department"
                  standout
                  dense
                  clearable
                  disable
                ></q-input>
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
                <q-input
                  class="col"
                  v-model="detail.job"
                  standout
                  dense
                  clearable
                  disable
                ></q-input>
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
                  standout
                  dense
                  clearable
                  disable
                ></q-input>
              </div>
            </div>
          </q-form>
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
      id: "",
      num: "",
      name: "",
      remarks: "",
      detail: {},
      selection: "none"
    };
  },
  methods: {
    setData(data) {
      data.sex = data.sex === 0 ? "女" : "男";
      this.detail = data;
    },
    save() {
      this.$refs.myForm.validate().then(success => {
        if (success) {
          this.$emit("detailsSave");
        } else {
          //
        }
      });
    },
    cancel() {
      this.$emit("detailsCancel");
    }
  }
};
</script>
