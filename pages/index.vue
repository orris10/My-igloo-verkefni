<template>
    <main>		
		<!--/hero_in-->
		<div class="container margin_60_35">
			<div class="row">
                <ApartmentsHeader />
                <AsideBar 
                    @update:countOfBedrooms="updateBedroomCount"
                    @update:countOfBathrooms="updateBathroomCount"
                    @update:Inputfield="updateInputField"
                    @update:pricingValues="updatePricingValues"
                    :pricingRange="pricingRange"
                />
                <ApartmentsListing
                    :apartments="apartments"
                    @update:apartmentsList="loadMoreApartments"
                    :enough="enough"
                 />
            </div>
        </div>
    </main>
</template>


<script>

import AsideBar from '../components/AsideBar.vue';
import ApartmentsListing from '../components/ApartmentsListing.vue';
import ApartmentsHeader from '../components/ApartmentsHeader';
export default {
    head:{
        title:'Finnur þú þína leiguíbúð?'
    },
    data(){
        return{
            apartments:[],
            inputValue: '',
            page:1,
            maxPage:1,
            countOfBedrooms: 1,
            countOfBathrooms: 0,
            min_price:100000,
            max_price:300000,
            enough: false,
            pricingRange: [],
        }
    },
    css: [
        '~/assets/css/style.css',
    ],
    components:{
        AsideBar,
        ApartmentsListing,
        ApartmentsHeader
    },
    methods:{
        fetchAllApartments: async function(){
            this.page = 1
            this.apartments = await fetch(
            `https://api-rent.myigloo.is/api/2019-10/listings/?min_bedrooms=${this.countOfBedrooms}&min_bathrooms=${this.countOfBathrooms}&min_price=${this.min_price}&max_price=${this.max_price}&page=${this.page}&include_locations=True`
            ).then(res => res.json())
            console.log('apartments count: ' + this.apartments.items.length)
            this.maxPage = Math.floor(this.apartments.count / 25) + 1;
        },
        updateBedroomCount: async function(value){
            this.countOfBedrooms = value;
            //set page and enough to default
            this.page = 1
            this.enough = false
            await this.fetchAllApartments();
            if(this.inputValue !== ''){
                this.filterApartmentsWithInputValue()
            }
        },
        updatePricingValues: async function(v1, v2){
            this.min_price = v1;
            this.max_price = v2;
            //set page and enough to default
            this.page = 1
            this.enough = false
            await this.fetchAllApartments();
            if(this.inputValue !== ''){
                this.filterApartmentsWithInputValue()
            }
        },
        updateBathroomCount: function(value){
            this.countOfBathrooms = value;
            //set page and enough to default
            this.page = 1
            this.enough = false
            this.fetchAllApartments();
            if(this.inputValue !== ''){
                this.filterApartmentsWithInputValue()
            }
        },
        updateInputField: async function(value){
            var upperCaseValue =value.charAt(0).toUpperCase() + value.substr(1)
            this.inputValue = upperCaseValue
            await this.fetchAllApartments()
            this.filterApartmentsWithInputValue()
        },
        filterApartmentsWithInputValue:function(){
            var result = []
            result = this.apartments.items.filter(apartment => apartment.address.city.name.startsWith(this.inputValue));
            this.apartments.items = result
        },
        loadMoreApartments: async function() {
            //use his function for infinite scroll
            //get mode data from diffrent page and add to the apartments array.
            if(this.page >= this.maxPage){
                this.enough = true;
                return;
            }
            this.page +=1
            var result = await fetch(
            `https://api-rent.myigloo.is/api/2019-10/listings/?min_bedrooms=${this.countOfBedrooms}&min_bathrooms=${this.countOfBathrooms}&min_price=${this.min_price}&max_price=${this.max_price}&page=${this.page}&include_locations=True`
            ).then(res => res.json())
            .catch(err =>{console.log(err)})
            if(result.items.length > 1){
                result.items.forEach(element => this.apartments.items.push(element));
            }
            if(this.inputValue !==''){
                this.filterApartmentsWithInputValue()
            }
        },
        fillUpPricingRange:function(){
            var startPrice = 10000
            for(var i = startPrice; i < 400000; i+=10000){
                this.pricingRange.push(i)
            }
        }

    },
    beforeMount(){
        this.fetchAllApartments();
        this.fillUpPricingRange();
    }
}
</script>

<style scoped>
@import '~/assets/css/style.css';
@import '~/assets/css/bootstrap.min.css';
</style>

