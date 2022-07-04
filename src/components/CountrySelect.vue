<template>
    <select 
        v-model="select" 
        class="form-select mt-5 mb-5 block w-full border-neutral-400 p-3 rounded cursor-pointer bg-blue-100 text-xl">
            <option
                disabled>
                Select Country
            </option>
            <option
                v-for="(country,index) in countries" 
                :key="index"
                @click="selectedCountry(index)">
                {{country.Country}}
            </option>
    </select>
</template>

<script>
export default {
    name: 'CountrySelect',
    props: {
        countries: [] 
    },
    data() {
        return {
            select: 'Select Country'
        }
    },
    methods: {
        selectedCountry(index) {
            this.$emit('selected-country', this.select,index)
        }
    }
}
</script>