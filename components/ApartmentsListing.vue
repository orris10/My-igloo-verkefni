<template>
    <div id="showgrid">
        <div class="row1">
            <div v-for="item in apartments.items" :key="item.id">
                        <div class="isotope-item popular gridView">
								<div class="box_grid">
									<figure>
										<a href="#0" class="wish_bt"></a>
										<a href="hotel-detail.html"><img :src="item.images[0].image.file_field" class="img-fluid" alt="" width="800" height="533"><div class="read_more"><span>Read more</span></div></a>
										<small>{{item.address.city.name}}</small>
									</figure>
									<div class="wrapper">
										<div class="cat_star"><i class="icon_star"></i><i class="icon_star"></i><i class="icon_star"></i><i class="icon_star"></i></div>
										<h3><a href="hotel-detail.html">{{item.title}}</a></h3>
										<span class="price">Verð <strong>{{item.price_formatted}}</strong> kr. {{item.price.price_type.name}}</span>
                                        <br>
                                        <span class="price"><strong>{{item.address.postal_code.code}} {{item.address.city.name}}</strong></span>
									</div>
									<ul>
										<li><i class="ti-eye"></i>{{item.size}}Fm.</li>
										<li><div class="score"><span style="font-size:15px">Fjöldi herbergja</span><strong>{{item.bedrooms}}</strong></div></li>
									</ul>
								</div>
							</div>
            </div>
            <InfiniteScroll  :enough="enough" @load-more="infiniteScroll()" />
							<!-- /box_grid -->
        </div>
    </div>
</template>

<script>
export default {
    props: ['apartments','enough'],
    methods:{
        infiniteScroll() {
            this.$emit('update:apartmentsList')
        }
    }
}
</script>


<style scoped>
@import '~/assets/css/style.css';
@import '~/assets/css/bootstrap.min.css';
</style>

