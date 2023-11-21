<template>
    <div>
        <form class="mx-auto d-flex">
            <div class="input-group ">
                <input class="form-control border-secondary py-2 rounded-0" type="search" placeholder="Tìm kiếm.... "
                    aria-label="Search" :value="modelValue" @input="updateModelValue" @keyup.enter="submit">

            </div>
        </form>
    </div>
</template>

<script>
export default {
    props: {
        modelValue: {
            type: String,
            default: ""
        },
    },
    emits: ["submit", "update:modelValue"],
    methods: {
        updateModelValue(e) {
            this.$emit("update:modelValue", e.target.value);
        },
        submit() {
            this.$emit("submit");
        }
    },
};
</script>