<template>
<mk-container :style="`height: ${props.height}px;`" :show-header="props.showHeader" :scrollable="true">
	<template #header><fa :icon="faBell"/>{{ $t('notifications') }}</template>
	<template #func><button @click="configure()" class="_button"><fa :icon="faCog"/></button></template>

	<div>
		<x-notifications :include-types="props.includingTypes"/>
	</div>
</mk-container>
</template>

<script lang="ts">
import { faBell, faCog } from '@fortawesome/free-solid-svg-icons';
import MkContainer from '../components/ui/container.vue';
import XNotifications from '../components/notifications.vue';
import define from './define';

export default define({
	name: 'notifications',
	props: () => ({
		showHeader: {
			type: 'boolean',
			default: true,
		},
		height: {
			type: 'number',
			default: 300,
		},
		includingTypes: {
			type: 'array',
			hidden: true,
			default: null,
		},
	})
}).extend({
	components: {
		MkContainer,
		XNotifications,
	},

	data() {
		return {
			faBell, faCog
		};
	},

	methods: {
		async configure() {
			this.$root.new(await import('../components/notification-setting-window.vue').then(m => m.default), {
				includingTypes: this.props.includingTypes,
			}).$on('ok', async ({ includingTypes }) => {
				this.props.includingTypes = includingTypes;
				this.save();
			});
		}
	}
});
</script>
