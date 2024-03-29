<template>
  <a-drawer
    v-model:visible="visible"
    :title="isAdd ? '新建' : '修改'"
    width="380px"
    @cancel="visible = false"
    @before-ok="handleSubmit"
  >
    <a-form
      ref="formRef"
      :model="form"
      label-align="left"
      layout="horizontal"
      auto-label-width
    >
      <a-form-item
        field="name"
        label="用户姓名"
        :rules="[
          {
            required: true,
            message: '用户姓名是必填项',
          },
        ]"
        :validate-trigger="['change', 'input']"
      >
        <a-input v-model="form.name" allow-clear placeholder="请输入用户姓名" />
      </a-form-item>

      <a-form-item
        field="phone"
        label="用户电话"
        :rules="[
          {
            required: true,
            message: '用户电话是必填项',
          },
        ]"
        :validate-trigger="['change', 'input']"
      >
        <a-input
          v-model="form.phone"
          allow-clear
          placeholder="请输入用户电话"
        />
      </a-form-item>

      <a-form-item
        field="email"
        label="用户邮箱"
        :rules="[
          {
            required: true,
            message: '用户邮箱是必填项',
          },
        ]"
        :validate-trigger="['change', 'input']"
      >
        <a-input
          v-model="form.email"
          allow-clear
          placeholder="请输入用户邮箱"
        />
      </a-form-item>

      <a-form-item
        field="gender"
        label="用户性别"
        :rules="[
          {
            required: true,
            message: '用户性别是必填项',
          },
        ]"
        :validate-trigger="['change', 'input']"
      >
        <a-radio-group v-model="form.gender">
          <template v-for="(val, key) in $genderList" :key="key">
            <a-radio :value="key">{{ val }}</a-radio>
          </template>
        </a-radio-group>
      </a-form-item>

      <a-form-item
        field="role_ids"
        label="用户角色"
        :rules="[
          {
            required: true,
            message: '用户角色是必填项',
          },
        ]"
        :validate-trigger="['change', 'input']"
      >
        <a-cascader
          v-model="form.role_ids"
          multiple
          check-strictly
          :options="[roles]"
          placeholder="请选择用户角色"
          :field-names="{ value: 'id', label: 'name' }"
          allow-search
        />
      </a-form-item>

      <a-form-item
        field="department_id"
        label="用户部门"
        :rules="[
          {
            required: true,
            message: '用户部门是必填项',
          },
        ]"
        :validate-trigger="['change', 'input']"
      >
        <a-cascader
          v-model="form.department_id"
          check-strictly
          :options="[departments]"
          :field-names="{ value: 'id', label: 'name' }"
          placeholder="请选择用户部门"
          allow-search
        />
      </a-form-item>

      <!-- <a-form-item
        field="status"
        label="用户状态"
        :rules="[
          {
            required: true,
            message: '用户状态是必填项',
          },
        ]"
        :validate-trigger="['change', 'input']"
      >
        <a-radio-group v-model="form.status">
          <a-radio :value="true">启用</a-radio>
          <a-radio :value="false">禁用</a-radio>
        </a-radio-group>
      </a-form-item> -->
    </a-form>
  </a-drawer>
</template>

<script lang="ts" setup>
  import { ref, watch } from 'vue';
  import { CascaderOption } from '@arco-design/web-vue';
  import { User } from '@/api/basic/types/user';

  const formRef = ref();
  const visible = ref(false);
  const isAdd = ref(false);

  const props = defineProps<{
    departments: CascaderOption;
    roles: CascaderOption;
    form: User;
  }>();

  const form = ref({});
  const emit = defineEmits(['add', 'update']);

  watch(
    () => props.form,
    (val) => {
      form.value = val;
    }
  );

  const showAddDrawer = () => {
    visible.value = true;
    isAdd.value = true;
  };

  const showUpdateDrawer = () => {
    visible.value = true;
    isAdd.value = false;
  };

  const closeDrawer = () => {
    visible.value = false;
  };

  defineExpose({ showAddDrawer, showUpdateDrawer, closeDrawer });

  const handleSubmit = async () => {
    const isError = await formRef.value.validate();
    if (isError) {
      return false;
    }

    if (isAdd.value) {
      emit('add', { ...form.value });
    } else {
      emit('update', { ...form.value });
    }
    return true;
  };
</script>
