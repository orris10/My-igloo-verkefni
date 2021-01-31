<template>
    <aside class="asideBar">
					<div class="custom-search-input-2 inner-2">
						<div class="form-group">
							<input class="form-control" type="text" placeholder="Staðsetning..." @change="getInputValue($event.target.value)">
							<i class="icon_search"></i>
						</div>
					</div>
					<!-- /custom-search-input-2 -->
					<div id="filters_col">
						<div class="collapse show" id="collapseFilters">
							<div class="filter_type">
                                <h6>Verðbil frá</h6>
                                <select class="pricing_select" v-model="pricingFromText" @change="getPricingValueFrom($event.target.value)">
                                    <option class="pricing_option" v-for="item in pricingRange" :key="item" >{{item}} kr.</option>
                                </select>
                            </div>
                            <div class="filter_type">
                                <h6>Verðbil til</h6>
                                <select class="pricing_select" v-model="pricingToText" @change="getPricingValueTo($event.target.value)">
                                    <option class="pricing_option" v-for="item in pricingRange" :key="item" >{{item}} kr.</option>
                                </select>
                            </div>
							<div class="filter_type">
								<h6>Fjöldi Svefnherbergja</h6>
								 <ul>
                                    <li>
                                        <label>1+</label>
                                        <input type="radio" name="bedroom" class="js-switch" @click="getValueFromBedroomCount(1)">
                                    </li>
                                    <li>
                                        <label>2+</label>
                                        <input type="radio" name="bedroom" class="js-switch" @click="getValueFromBedroomCount(2)">
                                    </li>
                                    <li>
                                        <label>3+</label>
                                        <input type="radio" name="bedroom" class="js-switch" @click="getValueFromBedroomCount(3)">
                                    </li>
                                    <li>
                                        <label>4+</label>
                                        <input type="radio" name="bedroom" class="js-switch" @click="getValueFromBedroomCount(4)">
                                    </li>
                                </ul>
							</div>
                            <div class="filter_type">
								<h6>Fjöldi Baðherbergja</h6>
								 <ul>
                                    <li>
                                        <label>1+</label>
                                        <input type="radio" name="bathroom" class="js-switch" @click="getvalueFromBatnrooCount(1)">
                                    </li>
                                    <li>
                                        <label>2+</label>
                                        <input type="radio" name="bathroom" class="js-switch" @click="getvalueFromBatnrooCount(2)">
                                    </li>
                                </ul>
							</div>
						</div> 
						<!--/collapse -->
					</div>
					<!--/filters col-->
				</aside>
</template>

<script>
export default {
    data(){
        return{
            pricingFrom: 100000,
            pricingTo: 300000,
            pricingFromText: "100000 kr.",
            pricingToText: "300000 kr.",
        }
    },
    props:['pricingRange'],
    methods:{
        getValueFromBedroomCount(value){
            this.$emit('update:countOfBedrooms', value)
        },
        getvalueFromBatnrooCount(value){
            this.$emit('update:countOfBathrooms', value)
        },
        getInputValue(value){
            this.$emit('update:Inputfield', value)
        },
        getPricingValueFrom(value){
            value.substring(0, value.length-4)
            this.pricingFrom = parseInt(value)
            if(this.pricingFrom < this.pricingTo){
                this.$emit('update:pricingValues', this.pricingFrom, this.pricingTo)
            }
            else{
                alert('"Verðtil frá" má ekki vera stærra en "verðbil til"')
            }
        },
        getPricingValueTo(value){
            value.substring(0, value.length-4)
            this.pricingTo = parseInt(value)
            if(this.pricingFrom < this.pricingTo){
                this.$emit('update:pricingValues', this.pricingFrom, this.pricingTo)
            }
            else{
                alert('"Verðtil frá" má ekki vera stærra en "verðbil til"')
            }
        }
    }
}
</script>
<style scoped>
@import '~/assets/css/style.css';
@import '~/assets/css/bootstrap.min.css';
</style>

