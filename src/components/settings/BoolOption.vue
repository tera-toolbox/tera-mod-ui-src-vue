<template>
	<v-tooltip bottom>
		<template v-slot:activator="{ on, attrs }">
			<v-checkbox v-on="on" v-bind="attrs" dense hide-details :label="optionObject.name" v-model=val @change="$emit('update', { key: optionObject.key, value: val });"></v-checkbox>
		</template>
		<span> {{ optionObject.key }} </span>
	</v-tooltip>
</template>

<script>
export default {
	props: ["optionObject", "startValue"],
	data: () => ({
		val: false
	}),
	mounted: function() {
		this.$nextTick(() => {
			this.val = this.startValue;
		});
	},
	name: "BoolOption",
	components: {},
};
</script>