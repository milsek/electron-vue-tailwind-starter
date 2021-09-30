<template>
	<div class="flex bg-gray-800 h-6 w-full justify-between select-none">
		<div class='title text-left text-sm font-sans text-green-500 ml-2 w-full my-auto'>
			app-name
		</div>
		<div class="h-full flex text-white">
			<div class='w-8 h-full hover:bg-gray-700' @click="minimize">
				<font-awesome-icon :icon="['fas', 'window-minimize']" class="p-0.5" />
			</div>
			<div class='w-8 h-full hover:bg-gray-700' @click="resizeWindow">
				<font-awesome-icon v-if="!isMaximized" :icon="['fas', 'expand']" class="p-0.5"/>
				<font-awesome-icon v-else :icon="['fas', 'compress']" class="p-0.5"/>
			</div>
			<div class='w-8 h-full hover:bg-red-700' @click="closeApp">
				<font-awesome-icon :icon="['fas', 'times']"/>
			</div>
		</div>
	</div>
</template>

<script>
import { library } from '@fortawesome/fontawesome-svg-core'
import { faTimes, faWindowMinimize, faExpand, faCompress } from '@fortawesome/free-solid-svg-icons'
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'

const { remote } = window.require('electron');

library.add(faTimes, faWindowMinimize, faExpand, faCompress)
export default {
	components: { FontAwesomeIcon },
	data () {
		return {
			isMaximized: false
		}
	},
	mounted () {
		this.isMaximized = remote.BrowserWindow.getFocusedWindow().isMaximized()
	},
	methods: {
		resizeWindow () {
			if (remote.BrowserWindow.getFocusedWindow().isMaximized()) { 
				this.restore() 
			} else {
				this.maximize()
			}
			this.isMaximized = remote.BrowserWindow.getFocusedWindow().isMaximized()
		},
		minimize () {
			remote.BrowserWindow.getFocusedWindow().minimize()
		},
		restore () {
			remote.BrowserWindow.getFocusedWindow().restore()
		},
		maximize () {
			remote.BrowserWindow.getFocusedWindow().maximize()
		},
		closeApp () {
			remote.BrowserWindow.getFocusedWindow().destroy()
		}
	}
}
</script>

<style>
.title {
	-webkit-app-region: drag;
}
</style>