<!-- BluetoothScanner.vue -->
<template>
	<div>
		<h2>Bluetooth Device Scanner</h2>
		
		<!--
			<label for="inputUID">Enter Value:</label>
			<input id="inputUID" class="element" :style="inputStyle" v-model="local_uuid" />
		-->
		<hr>


		<button class="button-5" @click="scanDevices">Scan i.Cee</button>
		<ul>
			<li v-for="device in devices" :key="device.id">
				{{ device.name }}
				<button class="button-5" @click="connectToDevice(device)">Connect</button>
			</li>
		</ul>
		<div v-if="connectedDevice">
			<h2>Connected Device</h2>
			<p>{{ connectedDevice.name }} ( {{ connectedDevice.address }})</p>
				<button class="button-5" @click="readCharacteristic">CPU Temp</button>
			<p v-if="characteristicValue">{{ characteristicValue }}</p>
		</div>
	</div>
</template>
  
<script>
export default {
	data() {
		return {
		devices: [],
		services: [],
		connectedDevice: null,
		characteristicValue: null,
		local_uuid: "00000001-710e-4a5b-8d75-3e5b444bc3cf",
		};
	},
	methods: {
		async scanDevices() {
			try {
				console.log('Scanninf for uuid:', this.local_uuid);
				const device = await navigator.bluetooth.requestDevice({
					filters: [{ namePrefix: "iCeeplus" }, 
								{services: [this.local_uuid]}],
					optionalServices: [this.local_uuid]
				});
				this.devices.push(device);
			} catch (error) {
				console.error('Error scanning for devices:', error);
			}
		},
		async connectToDevice(device) {
			try {
				console.log(device);
				this.selectedDevice = device;
				const server = await device.gatt.connect();
				this.connectedDevice = { name: device.name, server , address: device.id};
				console.log('Devices: ');
				console.log(device);
				//device.addEventListener('gattserverdisconnected', this.handleDisconnect);
			} catch (error) {
				console.error('Error connecting to device:', error);
			}
		},
		async readCharacteristic() {
			try {
				const service = await this.connectedDevice.server.getPrimaryService(this.local_uuid);
				console.log('Service: ');
				console.log(service);
				const characteristic = await service.getCharacteristic("00000002-710e-4a5b-8d75-3e5b444bc3cf");
				console.log(characteristic);
				const value = await characteristic.readValue();
				this.characteristicValue = new TextDecoder().decode(value);
			} catch (error) {
				console.error('Error reading characteristic:', error);
			}
		},
		handleDisconnect(event) {
			console.log('Disconnected from the device:', event.target.device.name || 'Unnamed Device');
			// Additional actions you may want to take on disconnect
			this.selectedDevice = null;
			this.services = [];
			this.devices.pop();
		},
	},
	computed: {
		inputStyle() {
			const length = this.local_uuid.length;

			return {
				width: `${length * 9}px`, // Adjust the multiplier based on your design
				border: `2px solid ${length == 37 ? 'red' : 'green'}` // Example: Red border if length > 10, green otherwise
				// Add more styles as needed
			};
		}
	}
};
</script>
  
<style scoped>
	.element {
		margin-left: 20px; /* Moves the element 10 pixels to the left */
	}

	.button-5 {
		align-items: center;
		background-clip: padding-box;
		background-color: #fa6400;
		border: 1px solid transparent;
		border-radius: .25rem;
		box-shadow: rgba(0, 0, 0, 0.02) 0 1px 3px 0;
		box-sizing: border-box;
		color: #fff;
		cursor: pointer;
		display: inline-flex;
		font-family: system-ui,-apple-system,system-ui,"Helvetica Neue",Helvetica,Arial,sans-serif;
		font-size: 16px;
		font-weight: 600;
		justify-content: center;
		line-height: 1.25;
		margin: 0;
		min-height: 3rem;
		padding: calc(.875rem - 1px) calc(1.5rem - 1px);
		position: relative;
		text-decoration: none;
		transition: all 250ms;
		user-select: none;
		-webkit-user-select: none;
		touch-action: manipulation;
		vertical-align: baseline;
		width: auto;
	}

	.button-5:hover,
	.button-5:focus {
		background-color: #fb8332;
		box-shadow: rgba(0, 0, 0, 0.1) 0 4px 12px;
	}

	.button-5:hover {
		transform: translateY(-1px);
	}

	.button-5:active {
		background-color: #c85000;
		box-shadow: rgba(0, 0, 0, .06) 0 2px 4px;
		transform: translateY(0);
	}
</style>