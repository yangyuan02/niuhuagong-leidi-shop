<template>
	<div class="pageView">
		<AppHeader :title="title"></AppHeader>
		<div class="scroll-view-wrapper assets-view" :class="{'visibility':!pageView}">
			<div class="asset_view_bg"></div>
			<div class="asset_info">
			  <div class="asset_info_icon">
				  <svg class="ico icon_jinbi"  aria-hidden="true">
					  <use xlink:href="#icon-jinbi"></use>
				  </svg>
			  </div>
				<h5>账户收益</h5>
				<p><i>￥</i><b>{{price | price}}</b></p>
			</div>
			<div class="asset_list">
				<div class="asset_list_item" @click="pageAction('/user/income')">
					<svg class="ico asset_list_icon"  aria-hidden="true">
						<use xlink:href="#icon-shouzhimingxi"></use>
					</svg>
					<span>收支明细</span>
				</div>
				<div class="asset_list_item" @click="pageAction('/user/cash')">
					<svg class="ico asset_list_icon"  aria-hidden="true">
						<use xlink:href="#icon-tixian"></use>
					</svg>
					<span>申请提现</span>
				</div>
			</div>
		</div>
	</div>
</template>


<script>

	import AppHeader from '@/components/common/header'

	import { mapActions, mapGetters } from 'vuex'

	import * as Model from '@/model/user'

	export default {

		components: {
			AppHeader
		},

		data () {
			return {

				title: '我的资产',
				price: ''

			}
		},
		mixin: ['loading'],
		computed: {

			...mapGetters({
				'pageView': 'getPageView'

			})
		},

		beforeCreate () {

			document.title = '我的资产'

		},

		created () {

			this.updatePageView(false)

			this.getUserAssets()

			this.showLoading()

		},

		methods: {

			...mapActions([
				'updatePageView'
			]),

			backFn () {

				this.pageAction('/user/center')

			},

			pageAction (url) {

				this.$router.push(url)

			},

			/**
			 *
			 * 获取用户当前金额
			 */

			getUserAssets () {

				Model.getUserAssets({
					type: 'GET'
				}).then((res) => {

					this.updatePageView(true)
					this.$hideLoading()

					const data = res.data

					if (res.status >= 1) {

						this.price = data.cash

					} else {

					}

				})

			}
		}

	}

</script>

<style lang="scss">

	.asset_list{

		margin-top: 2.8rem;

		padding-top: .46rem;

		display: flex;

	}
	.asset_list_item{

		display: flex;

		flex: 1;

		height: 2.58rem;

		align-items: center;

		justify-content: center;

		flex-direction: column;

		span{

			padding-top: .4rem;

		}

		&:first-child{

			border-right: .01rem solid #e6e6e6;

		}

		.asset_list_icon{

			width: .7rem;
			height: .7rem;

			color: #1ba0e5;

		}

	}

	.asset_info{

		width:90%;

		height: 5.3rem;

		background: #33b9ff;

		position:absolute;

		left: 50%;

		top: 2rem;

		transform: translateX(-50%);

		text-align: center;

		h5{

			font-size: .32rem;

			color: #fff;

			padding-bottom: .75rem;
		}
		p{
			color: #fff;

			i{
				font-size: .36rem;

				position: relative;

				top: -0.3rem;

			}
			b{

				font-weight: bold;
				font-size: .8rem;
			}
		}

	}

	.asset_info_icon{

		width: 1.86rem;

		height: 1.86rem;

		background: #0287cc;

		position: relative;

		left: 50%;

		top:0;

		transform: translate(-50%,-50%);

		border-radius: 50%;

		display: flex;

		align-items: center;

		justify-content: center;

		.icon_jinbi{

			color:#fff;

			width: .85rem;
			height: .85rem;

		}

	}

	.assets-view{

		background: #fff;

	}

	.asset_view_bg{

		background: #1ba0e5;

		height: 4.78rem;

	}

</style>
