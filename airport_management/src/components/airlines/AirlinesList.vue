<template>
	<el-table
		:data="cList"
		v-loading="tableLoading"
		style="width: 100%"
		:header-cell-style="{
			padding: '6px',
			'text-align': 'center',
			background: '#F5F5F5',
		}"
		:cell-style="{ padding: '5px', 'text-align': 'center' }"
	>
		<el-table-column prop="numb" label="航班号" sortable>
			<template slot-scope="scope">
				<div class="numb_column">
					<a href="">{{ scope.row.numb }}</a>
					<br />
					<span>{{ scope.row.type }}</span>
				</div>
			</template>
		</el-table-column>
		<el-table-column prop="startTime" label="起飞时间" sortable width="300">
			<template slot-scope="scope">
				<div class="time_column">
					<el-row :gutter="1">
						<el-col :span="10">
							<span>
								<p class="time_text">{{ scope.row.startTime }}</p>
								<p class="airport_text">{{ scope.row.startAirport }}</p>
							</span>
						</el-col>
						<el-col :span="4" class="arrow_col">
							<div
								class="arrow"
								style="margin-top: 50%; transform: translateY(-50%)"
							></div>
						</el-col>
						<el-col :span="10">
							<span>
								<p class="time_text">{{ scope.row.endTime }}</p>
								<p class="airport_text">{{ scope.row.endAirport }}</p>
							</span>
						</el-col>
					</el-row>
				</div>
			</template>
		</el-table-column>
		<el-table-column prop="from" label="航线">
			<template slot-scope="scope">
				<div class="place_column">
					<el-row :gutter="1">
						<el-col :span="8" :offset="2">
							<div>{{ scope.row.from }}</div>
						</el-col>
						<el-col
							:span="6"
							class="arrow_col"
							style="
								display: flex;
								justify-content: center;
								align-items: center;
							"
						>
							<div class="arrow"></div>
						</el-col>
						<el-col :span="8">
							<div>{{ scope.row.to }}</div>
						</el-col>
					</el-row>
				</div>
			</template>
		</el-table-column>
		<el-table-column
			prop="ontimeRate"
			label="准点率"
			sortable
			:sort-method="onTimeRateSort"
			width="100"
		></el-table-column>
		<el-table-column prop="date" label="本周参考班期">
			<template slot-scope="scope">
				<div class="date_column">
					<span
						v-for="(item, index) in dateList"
						:key="index"
						:class="{ blue: scope.row.date[index] == '1' }"
					>
						{{item}}
					</span>
				</div>
			</template>
		</el-table-column>
	</el-table>
</template>
  
<script>
export default {
	name: "AirlinesList",
	data() {
		return {
			tableLoading: true,
			dateList: ["一", "二", "三", "四", "五", "六", "日"],
		}
	},
	methods: {
		// 准时率排序
		onTimeRateSort(a, b) {
			const r1 =
				a.ontimeRate == "-"
					? -1
					: a.ontimeRate.substr(0, a.ontimeRate.length - 1)
			const r2 =
				b.ontimeRate == "-"
					? -1
					: b.ontimeRate.substr(0, b.ontimeRate.length - 1)
			return r1 - r2
		},
	},
	props: {
		cList: {
			type: Array,
		},
		cIsInit: {
			type: Boolean,
		},
	},
	watch: {
		cList(val) {
			if (val.length > 0) {
				this.tableLoading = false
			}
		},
	},
	created() {
		if (this.cIsInit) this.tableLoading = false
	},
}
</script>

<style lang="less" scoped>
.el-table td {
	padding: 0px 0px !important;
}
/*第一列*/
.numb_column {
	a {
		color: @homeBlue;
		text-decoration: none;
		font-weight: 600;
		font-size: 0.8rem;
	}
	span {
		color: #ccc;
		font-size: 0.75rem;
	}
}
/*第二列*/
.time_column {
	span {
		text-align: center;
		p {
			margin: 0;
		}
		.time_text {
			font-size: 1.2rem;
			font-weight: 700;
		}
		.airport_text {
			font-size: 0.7rem;
		}
	}
}
/*第五列*/
.date_column {
	span {
		color: #ccc;
		font-size: 0.75rem;
		font-weight: 600;
		&.blue {
			color: @homeBlue;
		}
	}
}
.arrow {
	width: 55px;
	height: 20px;
	background: url("../../assets/images/schedule_sprits.png") no-repeat left -100px;
}
</style>