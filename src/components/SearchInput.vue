<template>
    <input 
        name="search" 
        id="search"
        :value="value" 
        @input="handleChange"
        :class = "{ dark }"
    />
</template>

<script>
export default {
    name: 'SearchInput',
    props: {
        value: {
            type: String,
            required: true,
        },
        dark: {
            type: Boolean,
            required: false,
        },
    },
    methods: {
        handleChange(e) {
            this.$emit('input', e.target.value);
        }
    }
}
</script>

<style lang="scss" scoped>

    input {
        margin-top: 50px;
        display: flex;
        flex-direction: column;
        width: 250px;
        color: #fff;
        text-align: center;
        font-size: 18px;
        font-weight: 300;
        height: 30px;
        border: 0;
        border-bottom: 1px solid #fff;
        background: none;
        transition: box-shadow .5s ease-out;

        
        @media (min-width: 1024px) {
            font-weight: 400;
        }

        &:focus {
            outline: none;
            box-shadow: 0 5px 10px -8px rgba(255, 255, 255, .5);
        }
    }

    .dark {
        color: #1e3d4a;
        border-bottom-color: #1e3d4a;
    }

    .dark:focus {
         outline: none;
        box-shadow: 0 5px 10px -8px rgba(#1e3d4a, .2);            
    }

</style>
