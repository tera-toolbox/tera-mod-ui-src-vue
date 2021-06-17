<template>
	<v-app>
		<template v-if="!settings && !structure">
			<v-container fluid fill-height>
				<v-row align="center" justify="center">
					<v-progress-circular :size="70" indeterminate></v-progress-circular>
				</v-row>
			</v-container>
		</template>
		<template v-else>
			<v-system-bar class="elevation-1 mb-2 pa-0" window>
				<span style="-webkit-app-region: drag;">{{ name }}</span>
				<v-spacer style="-webkit-app-region: drag;"></v-spacer>
				<v-btn icon @click="close"> 
					<v-icon class="ml-1" > {{ mdiClose }}</v-icon>
				</v-btn>
			</v-system-bar>
			<v-container fluid class="pa-0">
				<v-row>
					<v-col>
						<div class="scroller">
							<component 
								:is="selectComponent('bool')"
								v-bind="{ optionObject: elem, startValue: settings[elem.key] }"
								v-for="(elem, i) of selectComponentsTypes('bool', structure)" :key="i"
								@update="handleSettingChange">
							</component>
							<v-divider class="mb-2 mt-2"></v-divider>
							<component 
								:is="selectComponent('number')"
								v-bind="{ optionObject: elem, startValue: settings[elem.key] }"
								v-for="(elem, i) of selectComponentsTypes('number', structure)" :key="i"
								@update="handleSettingChange">
							</component>
							<v-divider class="mb-2 mt-2"></v-divider>
							<component 
								:is="selectComponent('select')"
								v-bind="{ optionObject: elem, startValue: settings[elem.key] }"
								v-for="(elem, i) of selectComponentsTypes('select', structure)" :key="i"
								@update="handleSettingChange">
							</component>
						</div>
					</v-col>
				</v-row>
			</v-container>
		</template>
	</v-app>
</template>

<script>
/* eslint-disable default-case */
/* eslint-disable no-case-declarations */
import BoolOption from "@/components/settings/BoolOption";
import SelectBoxOption from "@/components/settings/SelectBoxOption";
import NumberOption from "@/components/settings/NumberOption";

import { mdiClose } from "@mdi/js";

export default {
	name: "App",
	components: {
		BoolOption,
		SelectBoxOption,
		NumberOption
	},
	data: () => ({
		settings: null,
		structure: null,
		name: "Settings",
		mdiClose
	}),
	mounted: function () {
		window.renderer.on("static", staticObj => { this.staticData = staticObj; });
		window.renderer.on("init", (name, theme, structure, settings) => { 
			this.name = `${name} - Settings`;
			this.$vuetify.theme.dark = !(theme === "light");
			this.structure = structure;
			this.settings = settings;
		});
		window.renderer.send("init");
	},
	methods: {
		selectComponentsTypes(type, arr) {
			return arr.filter(elem => elem.type === type);
		},
		selectComponent(type) {
			switch(type) {
			case "bool": return "BoolOption";
			case "select": return "SelectBoxOption";
			case "number": return "NumberOption";
			//case "range": return "RangeOption";
			//case "string": return "TextBoxOption";
			}
		},
		handleSettingChange(val) {
			this.settings[val.key] = val.value;
			const copied = { ...this.settings };
			window.renderer.send("update", copied);
		},
		close() {
			window.renderer.send("close");
		}
	}
};
</script>