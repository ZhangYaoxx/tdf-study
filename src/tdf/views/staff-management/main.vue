<template>
  <scroll-area>
    <q-page class="q-pa-md">
      <q-card class="q-py-sm tdf-box-shadow" v-show="page === 1">
        <q-card-section>
          <q-card class="q-pa-md tdf-box-shadow">
            <div class="row justify-between q-gutter-y-lg">
              <div class="col-12 col-md-4">
                <div class="row">
                  <form-lable lable="姓名："></form-lable>
                  <q-input
                    class="col"
                    v-model="myForm.name"
                    placeholder="请输入姓名"
                    outlined
                    dense
                    clearable
                  ></q-input>
                </div>
              </div>
              <div class="col-12 col-md-4">
                <div class="row justify-end">
                  <q-btn
                    class="q-mr-md"
                    color="primary"
                    unelevated
                    :size="$btnSize"
                    @click="queryHandler"
                  >
                    <q-icon
                      :size="$btnIconSize"
                      class="q-mr-sm"
                      name="o_search"
                    ></q-icon>
                    查询
                  </q-btn>
                  <q-btn
                    class="q-mr-md"
                    color="info"
                    unelevated
                    outline
                    :size="$btnSize"
                    @click="resetHandler"
                  >
                    <q-icon
                      :size="$btnIconSize"
                      class="q-mr-sm"
                      name="o_refresh"
                    ></q-icon>
                    重置
                  </q-btn>
                  <q-btn-dropdown
                    v-if="opens"
                    v-model="opens"
                    :size="$btnSize"
                    flat
                    color="primary"
                    persistent
                    :label="opens === false ? '展开' : '收起'"
                  ></q-btn-dropdown>
                </div>
              </div>
            </div>
          </q-card>
        </q-card-section>
        <q-card-section>
          <q-table
            class="q-mt-md table"
            :data="UserGroupList"
            :columns="columns"
            :selection="selection"
            :pagination="pagination"
            :visible-columns="isVisibleColumns"
            @showAdd="showAdd = true"
            @detail="detail"
            @edit="edit"
            @deletes="deletes"
            flat
            bordered
            hide-pagination
          >
            <template v-slot:body-cell-sex="props">
              <q-td :props="props">
                {{
                  props.row.sex === null
                    ? ""
                    : props.row.sex === 0
                    ? "女"
                    : "男"
                }}
              </q-td>
            </template>
            <template v-slot:body-cell-createdDate="props">
              <q-td :props="props">
                {{ props.row.createdDate | parseTime }}
              </q-td>
            </template>
            <template v-slot:body-cell-number="props">
              <q-td :props="props">
                {{ props.row.number }}
              </q-td>
            </template>
            <template v-slot:top="props">
              <q-btn
                v-if="hasPerm('group/add') || hasPerm('group/*')"
                color="primary"
                unelevated
                :size="$btnSize"
                @click="showAdd = true"
              >
                <q-icon
                  :size="$btnIconSize"
                  class="q-mr-sm"
                  name="o_add"
                ></q-icon>
                新增
              </q-btn>
              <q-space />
              <q-btn
                outline
                :size="$btnSize"
                color="primary"
                label="切换全屏"
                :icon="props.inFullscreen ? 'fullscreen_exit' : 'fullscreen'"
                @click="props.toggleFullscreen"
                class="q-mr-sm"
              />
              <q-btn-dropdown
                outline
                :size="$btnSize"
                color="primary"
                label="自选列"
                icon="list"
              >
                <q-list dense padding>
                  <q-item
                    style="height: 40px;"
                    v-show="showQuery"
                    tag="label"
                    v-for="item in columns"
                    :key="item.name"
                  >
                    <q-item-section avatar>
                      <q-checkbox
                        dense
                        v-model="isVisibleColumns"
                        :val="item.name"
                      />
                    </q-item-section>
                    <q-item-section style="margin-left: -25px;">
                      {{ item.label }}
                    </q-item-section>
                  </q-item>
                </q-list>
              </q-btn-dropdown>
            </template>
            <template v-slot:body-cell-actions="props">
              <q-td :props="props">
                <q-btn
                  v-if="hasPerm('group/check') || hasPerm('group/*')"
                  color="primary"
                  :size="$btnSize"
                  icon="o_search"
                  round
                  dense
                  flat
                  @click="detail(props)"
                >
                  <q-tooltip>
                    查看
                  </q-tooltip>
                </q-btn>
                <q-btn
                  v-if="hasPerm('group/edit') || hasPerm('group/*')"
                  color="primary"
                  :size="$btnSize"
                  icon="o_edit"
                  round
                  dense
                  flat
                  @click="edit(props)"
                >
                  <q-tooltip>
                    编辑
                  </q-tooltip>
                </q-btn>
                <q-btn
                  v-if="hasPerm('group/delete') || hasPerm('group/*')"
                  color="warning"
                  :size="$btnSize"
                  icon="o_delete"
                  round
                  dense
                  flat
                  @click="deletes(props.row)"
                >
                  <q-tooltip>
                    删除
                  </q-tooltip>
                </q-btn>
              </q-td>
            </template>
          </q-table>
          <pagination
            :pagination="pagination"
            :pages-number="pagesNumber"
            @changePage="changePage"
            @changeRowsPerPage="changeRowsPerPage"
          ></pagination>
        </q-card-section>
      </q-card>
      <q-dialog v-model="showAdd" persistent>
        <q-card class="tdf-box-shadow full-width">
          <div class="tdf-title-body">
            <div class="q-ml-lg text-subtitle1">新增</div>
            <q-space></q-space>
            <q-btn
              icon="close"
              class="q-mr-lg"
              flat
              round
              dense
              v-close-popup
            ></q-btn>
          </div>
          <q-separator></q-separator>
          <Add @addCancel="showAdd = false" @addSave="addSave"></Add>
        </q-card>
      </q-dialog>
      <q-card class="q-py-sm tdf-box-shadow" v-show="page === 3">
        <q-card-section>
          <Details ref="setDetails" @detailsCancel="detailsCancel"></Details>
        </q-card-section>
      </q-card>
      <q-card class="q-py-sm tdf-box-shadow jus" v-show="page === 4">
        <q-card-section>
          <user-group-edit
            ref="setEdit"
            @editCancel="editCancel"
            @editSave="editSave"
          ></user-group-edit>
        </q-card-section>
      </q-card>
    </q-page>
  </scroll-area>
</template>

<script>
import Add from "./Add";
import Details from "./Details";
import UserGroupEdit from "./Edit";
import * as GroupAPI from "../../api/system-management/group";
import ScrollArea from "@componets/scrollarea/ScrollArea";
import Pagination from "@componets/pagination/Pagination";
import FormLable from "@componets/formlable/FormLable";

export default {
  name: "staffManagement",
  components: {
    FormLable,
    Pagination,
    ScrollArea,
    Add,
    Details,
    UserGroupEdit
  },
  data() {
    return {
      showQuery: true,
      name: "",
      page: 1,
      pagesNumber: 100,
      pagination: {
        page: 1,
        rowsNumber: 10
      },
      selection: "none",
      selected: [],
      isVisibleColumns: [
        "name",
        "sex",
        "createdDate",
        "actions",
        "address",
        "number",
        "job",
        "department"
      ],
      columns: [
        {
          name: "name",
          field: "name",
          label: "姓名",
          align: "left",
          headerStyle: "",
          sortable: false
        },
        {
          name: "sex",
          field: "sex",
          label: "性别",
          align: "left",
          headerStyle: "width: 120px;",
          sortable: false
        },
        {
          name: "address",
          field: "address",
          label: "地址",
          align: "left",
          sortable: false
        },
        {
          name: "department",
          field: "department",
          label: "部门",
          align: "left",
          sortable: false
        },
        {
          name: "number",
          field: "number",
          label: "工号",
          align: "left",
          sortable: false
        },
        {
          name: "job",
          field: "job",
          label: "职务",
          align: "left",
          sortable: false
        },
        {
          name: "createdDate",
          field: "createdDate",
          label: "创建时间",
          align: "left",
          headerStyle: "width: 200px;",
          sortable: false
        },
        {
          name: "actions",
          field: "actions",
          label: "操作",
          align: "center",
          headerStyle: "width: 160px;"
        }
      ],
      UserGroupList: [],
      showAdd: false,
      myForm: {},
      opens: false
    };
  },
  created() {
    this.executeQueryPage();
  },
  methods: {
    changeRowsPerPage() {
      this.executeQueryPage();
    },
    changePage() {
      this.executeQueryPage();
    },
    detail(props) {
      this.$nextTick(() => {
        this.$refs.setDetails.setData(props.row);
      });
      this.page = 3;
    },
    detailsCancel() {
      this.page = 1;
    },
    edit(props) {
      this.$refs.setEdit.setData(props.row);
      this.page = 4;
    },
    editCancel() {
      this.page = 1;
    },
    addSave(addData) {
      this.$q
        .dialog({
          title: "提示",
          message: "确定要保存吗?",
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          GroupAPI.addGroup(addData).then(() => {
            this.$q.notify({
              type: "positive",
              message: "操作成功",
              position: "top"
            });
            this.showAdd = false;
            this.page = 1;
            this.executeQueryPage();
          });
        });
    },
    editSave(editData) {
      GroupAPI.editGroup(editData).then(data => {
        console.info(data);
        this.page = 1;
        this.$q.notify({
          type: "positive",
          message: "操作成功",
          position: "top"
        });
      });
    },
    deletes(row) {
      this.$q
        .dialog({
          title: "提示",
          message: "此操作将永久删除该项，并可能删除关联关系, 是否继续?",
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          var list = [];
          list.push(row.id);
          GroupAPI.delGroup(list).then(() => {
            this.$q.notify({
              message: "删除成功",
              type: "positive",
              position: "top"
            });
            this.executeQueryPage();
          });
        })
        .onCancel(() => {
          this.$q.notify({
            message: "已取消删除",
            type: "info",
            position: "top"
          });
        });
    },
    queryHandler() {
      this.executeQueryPage();
    },
    resetHandler() {
      this.myForm = {};
      this.executeQueryPage();
    },
    executeQueryPage() {
      const params = {
        filters: this.myForm,
        page: this.pagination.page,
        pageSize: this.pagination.rowsNumber,
        sorts: [{ fieldName: "groupIndex", direction: "asc" }]
      };
      GroupAPI.queryPageGroups(params).then(data => {
        console.info("data", data);
        this.UserGroupList = data.list;
        this.pagesNumber = data.total;
      });
    }
  }
};
</script>
