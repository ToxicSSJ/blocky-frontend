<NotificationDisplay />

<script>
	
	import { Base64 } from 'js-base64';
	import { onMount } from 'svelte';
	import { HSplitPane, VSplitPane } from 'svelte-split-pane';
	import { TextInput } from "carbon-components-svelte";
	import { NotificationDisplay, notifier } from '@beyonk/svelte-notifications'

	let files;
	let tdelete = '';
	let tdownload = '';
	let thostname = '';
	let ahostname = 'http://127.0.0.1:5050';

	let listFiles;

	function setHostname () {
		ahostname = thostname
		notifier.success('Hostname set to: ' + ahostname + '!')
	}

	function uploadFile () {

		console.log(files[0])
		{#await fetchImage}
		<p>...waiting</p>
		{:then data}
		<img src={data.message} alt="Dog image" />
		{/await}
		console.log(files[0].text())
		console.log(Base64.encode('lel'))

	}

	function downloadFile () {
		ahostname = thostname
	}

	function deleteFile () {
		ahostname = thostname
		
	}

	onMount(()=>{
		listFiles = () =>{
            console.log(ahostname)
			fetch(ahostname + "/list")
			.then(response => response.json())
			.then(data => {
					console.log(data);
			}).catch(error => {
				console.log(error);
				return [];
			});
        }
	});

</script>

<style>
	main {
		font-family: tahoma;
		text-align: center;
	}
	.wrapper {
		font-family: tahoma;
		text-align: center;
	}
	.space {
		margin-top: 80px;
		margin-bottom: 80px;
	}
</style>

<main>
	<h1>TELEMATICA RETO N2</h1>
	<p>Use las diferentes secciones para probar la comunicación Cliente/Servidor vía el protocolo HTTP</p>
	<TextInput
		placeholder="Enter server hostname..."
		bind:value={thostname}
	/>
	<button on:click={setHostname}>
		Set Hostname
	</button>
</main>

<div class="wrapper space">
	<VSplitPane>
		<top slot="top">
			<div class="space">
				<HSplitPane updateCallback={() => {
					console.log('HSplitPane Updated!');
				}}>
					<left slot="left">
						<button on:click={listFiles}>
							List Files
						</button>
					</left>
					<right slot="right">
						<input type="file" bind:files>
						<button on:click={uploadFile}>
							Upload File
						</button>
					</right>
				</HSplitPane>
			</div>
		</top>
		<down slot="down">
			<div class="space">
				<HSplitPane updateCallback={() => {
					console.log('HSplitPane Updated!');
				}}>
					<left slot="left">
						<input value={tdownload}>
						<button on:click={downloadFile}>
							Download File
						</button>
					</left>
					<right slot="right">
						<input value={tdelete}>
						<button on:click={deleteFile}>
							Delete File
						</button>
					</right>
				</HSplitPane>
			</div>
			
		</down>
	</VSplitPane>
	
</div>



