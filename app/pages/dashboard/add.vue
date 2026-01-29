<script lang="ts" setup>
import type { FetchError } from "ofetch";
import { toTypedSchema } from "@vee-validate/zod";
import { InsertLocation } from "~~/lib/db/schema";

const { $csrfFetch } = useNuxtApp();

const router = useRouter();
const { handleSubmit, errors, meta, setErrors } = useForm({
  validationSchema: toTypedSchema(InsertLocation),
});
const submitError = ref("");
const loading = ref(false);
const submitted = ref(false);

const onSubmit = handleSubmit(async (values: any) => {
  try {
    submitError.value = "";
    loading.value = true;
    await $csrfFetch("/api/locations", {
      method: "POST",
      body: values,
    });
    submitted.value = true;
    navigateTo(`/dashboard`);
  }
  catch (e) {
    const error = e as FetchError;
    if (error.data?.data) {
      setErrors(error.data?.data);
    }
    submitError.value = error.statusMessage || "Something went wrong";
  }
  finally {
    loading.value = false;
  }
});

onBeforeRouteLeave(() => {
  if (meta.value.dirty && !submitted.value) {
    // eslint-disable-next-line no-alert
    const confirm = window.confirm("Are you sure you want to leave? You have unsaved changes.");
    if (!confirm) {
      return false;
    }
  }
  return true;
});
</script>

<template>
  <div class="container max-w-md mx-auto">
    <div class="my-4">
      <h1 class="text-lg">
        Add Location
      </h1>
      <p class="text-sm">
        A location is a place you have traveled or will travel to.
      </p>
    </div>
    <div
      v-if="submitError"
      role="alert"
      class="alert alert-error"
    >
      <span>{{ submitError }}</span>
    </div>
    <form class="flex flex-col gap-2" @submit.prevent="onSubmit">
      <FormField
        label="Location Name"
        name="name"
        :error="errors.name"
        :disabled="loading"
      />
      <FormField
        label="Description"
        name="description"
        type="textarea"
        :error="errors.description"
        :disabled="loading"
      />
      <FormField
        label="Latitude"
        name="lat"
        :error="errors.lat"
        type="number"
        :disabled="loading"
      />
      <FormField
        label="Longitude"
        name="long"
        :error="errors.long"
        num="number"
        type="number"
        :disabled="loading"
      />

      <div class="flex justify-end gap-4">
        <button
          type="button"
          class="btn btn-outline"
          :disabled="loading"
          @click="router.back()"
        >
          Cancel
          <Icon name="tabler:x" size="24" />
        </button>
        <button
          type="submit"
          class="btn btn-primary"
          :disabled="loading"
        >
          Add Location
          <span v-if="loading" class="loading loading-dots loading-xl" />
          <Icon
            v-else
            name="tabler:circle-plus-filled"
            size="24"
          />
        </button>
      </div>
    </form>
  </div>
</template>
