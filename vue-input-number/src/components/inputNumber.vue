<template>
	<div :class="[
		'input-number',
		size ? 'input-size-' + size : '',
		{ 'is-disabled': disabled } 
		]"
	>
		<!-- 左侧减按钮 -->
		<span 
			class="btn left"
			:class="{ 'disabled': minDisabled }"
			@click.stop="decrease"
		>
			<i class="icon-jian iconfont"></i>
		</span>
		<!-- 右侧加按钮 -->
		<span 
			class="btn right"
			:class="{ 'disabled': maxDisabled }"
			@click.stop="increase"
		>
			<i class="icon-jia iconfont"></i>
		</span>
		<!-- 输入框 -->
		<input 
			ref="input"
			type="text"
			class="inner-input"
			:value="displayValue"
			:disabled="disabled"
			@keydown.up.prevent="increase"
			@keydown.down.prevent="decrease"
			@input="onInput"
			@focus="onFocus"
			@blur="onBlur" 
			@change="onChange"
		/>	
	</div>
</template>
<script>
/**
 * 使用方法：
 * @[v-model] { number } 绑定值
 * @[min] { number } 最小值
 * @[max] { number } 最大值
 * @[disabled] { boolean } 是否禁用
 * @[step] { number } 增加或减少的步数（可以为小数，计算结果自动保留两位小数）
 * @[size] { string } 尺寸，默认（高40px）、medium（高36px）、small（高32px）、mini（高28px）
 * @[change] { function } 绑定值被改变时触发，回调参数（改变后的值,点击的按钮类型，传入的参数），按钮类型：点击加按钮值为'increase';点击减按钮值为'decrease';如果没有点击按钮，则按钮类型值为null
 * @[blur] { function } Input 失去焦点时触发，回调参数（改变后的值）
 * @[focus] { function } Input 获得焦点时触发，回调参数（改变后的值）
 * @[extendParams] {  } 传入的参数， 通过change事件返回给父组件，如果未绑定，则是undefined
 */
export default {
  name: 'inputNumber',
	model:{
		prop: 'value',
		event: 'input'
	},
	props: {
		step: {
			type: Number,
			default: 1
		},
		max: {
			type: Number,
			default: Infinity
		},
		min: {
			type: Number,
			default: -Infinity
		},
		value: {},
		disabled: Boolean,
		size: String,
		placeholder: String,
		extendParams: {}
	},
	data() {
		return {
			currentValue: 0,
			userInput: null,
			precision: false,
			currentClickBtn: null,   
    };
	},
	mounted() {
		this.toPrecision();
	},
	methods: {
		toPrecision() {
			const stepString = this.step.toString();
			if (stepString.indexOf('.') !== -1) {
				this.precision = true;
			}
		},
		// 减少输入框的值
		decrease() {
			if (this.disabled || this.minDisabled) return;
			const value = this.value || 0;
			const newVal = this._decrease(value, this.step);
			this.currentClickBtn = 'decrease';
			this.setCurrentValue(newVal);
		},
		// 增加输入框的值
		increase() {
			if (this.disabled || this.maxDisabled) return;
			const value = this.value || 0;
			const newVal = this._increase(value, this.step);
			this.currentClickBtn = 'increase';
			this.setCurrentValue(newVal);
		},
		_decrease(val, step) {
			if (typeof val !== 'number' && val !== undefined) return this.currentValue;
			let newVal = parseFloat(val) - step;
			return newVal
		},
		_increase(val, step) {
			if (typeof val !== 'number' && val !== undefined) return this.currentValue;
			let newVal = parseFloat(val) + step;
			return newVal
		},
		setCurrentValue(newVal){
			const oldVal = this.currentValue;
			if (newVal >= this.max) newVal = this.max;
			if (newVal <= this.min) newVal = this.min;
			if (oldVal === newVal) return; 
			this.userInput = null;
			this.$emit('input', newVal);
			this.$emit('change', newVal, this.currentClickBtn, this.extendParams);
			this.currentValue = newVal;
		},
		onBlur(event) {
			this.$emit('blur',event);
		},
		onFocus(event) {
			this.$emit('focus',event);
		},
		onInput(event) {
			let val = event.target.value;
			this.userInput = val;
		},
		onChange(event) {
			let val = event.target.value;
			const newVal = val === '' ? undefined : Number(val);
			if (!isNaN(newVal)) {
				this.currentClickBtn = null;
				this.setCurrentValue(newVal);
			} else {
				this.userInput = null;
			}
		}
	},
	computed: {
		minDisabled() {
			return parseInt(this.currentValue) - this.step < this.min;
		},
		maxDisabled() {
			return parseInt(this.currentValue) + this.step > this.max;
		},
		displayValue() {
			if (this.userInput !== null) return this.userInput;
			let currentValue = this.currentValue;
			if (typeof currentValue === 'number') {
				if (this.precision) {
					currentValue = currentValue.toFixed(2);
				}
				return currentValue
			}
		}
	},
	watch: {
		value: {
			immediate: true,
			handler(value) {
				let newVal = value === undefined ? value : Number(value);
				if (isNaN(newVal) || newVal === undefined) return;
				if (newVal >= this.max) newVal = this.max;
				if (newVal <= this.min) newVal = this.min;
				this.currentValue = newVal;
				this.userInput = null;
				this.$emit('input', newVal);
			}
		},
		// currentClickBtn(val) {
		// 	console.log('currentClickBtn', val)
		// },
		// userInput(value) {
		// 	console.log('userInput', value)
		// }
	},
};
</script>
<style lang="stylus">

.input-number {
	display inline-block
	position relative
	height 40px
	line-height 38px
	width 180px
	box-sizing border-box
	border-radius 4px
	color #333
	transition border-color 0.2s cubic-bezier(0.645, 0.045, 0.355, 1)

	.btn {
		position absolute
		z-index 100
		width 40px
		height calc(100% - 2px)
		text-align center
		cursor pointer
		box-sizing border-box
		background #f5f7fa

		&:hover {
			color #409eff
		}
		&:hover~input {
			outline none
			border-color #409eff
		}
	}

	.btn.left {
		top 50%
		left 1px
		transform translateY(-50%)
		border-right 1px solid #dcdfe6
		border-radius 4px 0 0 4px
	}

	.btn.right {
		top 50%
		right 1px
		transform translateY(-50%)
		border-left 1px solid #dcdfe6
		border-radius 0 4px 4px 0
	}

	input {
		background-color #ffffff
		width 100%
		height 100%
		display block
		border 1px solid #dcdfe6
		border-radius 4px
		box-sizing border-box
		color #333
		padding 0 50px
		text-align center
		transition border-color 0.2s cubic-bezier(0.645, 0.045, 0.355, 1)

		&:hover {
			border-color #c0c4cc
		}

		&:focus {
			outline none
			border-color #409eff
		}
	}
}

.border-show {
	position absolute
	z-index 1
	top 0
	left 0
	height 100%
	width 100%
	border-radius 4px
	border 1px solid #409eff
	box-sizing border-box
}

.input-number.active {
	border 1px solid #409eff
}

.input-number.input-size-medium {
	height 36px
	line-height 34px

	.btn {
		width 37px
	}

	.inner-input {
		padding 0 43px
	}
}

.input-size-small {
	height 32px
	line-height 30px
	width 130px

	.btn {
		width 33px
	}

	.inner-input {
		padding 0 39px
	}

}

.input-size-mini {
	height 28px
	line-height 26px
	width 130px

	.btn {
		width 29px
	}

	.inner-input {
		padding 0 35px
	}
}

.btn.disabled {
	color #c0c4cc
	cursor not-allowed

	&:hover {
		color #c0c4cc
	}

	&:hover~input {
		outline none
		border-color #dcdfe6
	}
}

.input-number.is-disabled {
	background-color #f5f7fa

	.btn, .inner-input {
		background-color #f5f7fa
		cursor not-allowed
		border-color #e4e7ed
		color #c0c4cc

		&:hover {
			color #c0c4cc
		}
	}
}
</style>
