<script lang="ts">
import { defineComponent } from 'vue'
import axios from 'axios'
export default defineComponent({
    data() {
        return {
            search: '' as string,
            searchbox: false,
            partofspeech: '' as string,
            searchtext: '' as string,
        }
    },
    methods: {
        async getSearch() {
            try {
                const res = await axios.get(`https://api.dictionaryapi.dev/api/v2/entries/en/${this.search}`)
                this.searchtext = res.data[0].meanings[0].definitions[0].definition;
                this.partofspeech = res.data[0].meanings[0].partOfSpeech;
                this.searchbox = true;
            }
            catch (err) {
                console.log(err)
            }
        },
        closeSbox() {
            if (!this.search) {
                this.searchbox = false;
            }
        },
    },
})
</script>

<template>
    <div class="py-[20px] px-[20px]">
        <form class="md:space-x-[40px] md:mt-[50px] md:w-[80%] w-full md:flex">
            <div class="flex mt-[30px] md:w-[60%] md:mt-[0px]">
                <div
                    class="py-[10px] border-l-gray-300 border-t-gray-300 border-b-gray-300 border border-r-[0px] pl-[20px]">
                    <img src="/img/us.png" class="" alt="" />
                </div>
                <input type="search" @keyup="closeSbox" v-model="search" placeholder="Search....."
                    class="border-r-gray-300 border-t-gray-300 border-b-gray-300 w-full md:w-[100%] border-l-[0px] border py-[10px] px-[20px] focus:outline-none" />
            </div>
            <button type="button" @click="getSearch"
                class="mt-[8px] flex items-center space-x-[8px] btn text-[#fff] font-[500] text-[17px] rounded-[2px] w-fit h-fit py-[10px] px-[30px] md:mt-[0px]">
                <div>Search</div>
                <div><svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                        stroke="currentColor" class="w-6 h-6">
                        <path stroke-linecap="round" stroke-linejoin="round"
                            d="M15.75 15.75l-2.489-2.489m0 0a3.375 3.375 0 10-4.773-4.773 3.375 3.375 0 004.774 4.774zM21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                    </svg>
                </div>
            </button>
        </form>

        <div v-show="searchbox" class="w-full md:w-[50%] py-[20px] px-[30px] mt-[40px] border-gray-300 border">
            <div class="font-[600] text-[14px]">Part of Speech: <span>{{ partofspeech }}</span> </div>
            <div class="mt-[25px] text-left font-[500] text-[24px]">{{ searchtext }}</div>
        </div>
        <!-- the box search output -->
    </div>
</template>

<style scoped>
.btn {
    background: linear-gradient(112.1deg, rgb(32, 38, 57) 11.4%, rgb(63, 76, 119) 70.2%);
}
</style>