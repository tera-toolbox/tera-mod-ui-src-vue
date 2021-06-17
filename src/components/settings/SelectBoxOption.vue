<template>
	<v-tooltip bottom>
		<template v-slot:activator="{ on, attrs }">
			<v-select v-on="on" v-bind="attrs" item-value="key" item-text="name" class="mt-1" solo hide-details dense :items="optionObject.options" :prefix="`${optionObject.name}:  `" v-model=val @change="$emit('update', { key: optionObject.key, value: val });"></v-select>
		</template>
		<span> {{ optionObject.key }} </span>
	</v-tooltip>
</template>

<script>
export default {
	props: ["optionObject", "startValue"],
	data: () => ({
		val: 0
	}),
	mounted: function() {
		this.$nextTick(() => {
			this.val = this.startValue;
		});
	},
	name: "SelectBoxOption",
	components: {},
};
</script>